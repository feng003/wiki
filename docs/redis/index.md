
## 数据类型

| 结构类型 |	结构存储的值 |	结构的读写能力 | 实现 |
| ---- | ---- | ---- | ---  |
| String（字符串） | 	可以是字符串、整数或浮点数 |	对整个字符串或字符串的一部分进行操作；对整数或浮点数进行自增或自减操作； | int raw embstr |
| List（列表） |	一个链表，链表上的每个节点都包含一个字符串 | 对链表的两端进行 push 和 pop 操作，读取单个或多个元素；根据值查找或删除元素；| ziplist linkedlist |
| Hash（哈希） |	包含键值对的无序散列表 |	包含方法有添加、获取、删除单个元素 | ziplist hashtable |
| Set（集合） |	包含字符串的无序集合 |	字符串的集合，包含基础的方法有看是否存在添加、获取、删除；还包含计算交集、并集、差集等 | intest hashtable |
| Sorted sets（有序集合） |	类似于集合，但每个元素都有一个分数，根据分数可以对元素进行排序。| 	字符串成员与浮点数分数之间的有序映射；元素的排列顺序由分数的大小决定；包含方法有添加、获取、删除单个元素以及根据分值范围或成员来获取元素 | ziplist skiplist |


### 字符串

    buf[]: 字符数组，用于存放字符串

    GET/SET APPEND MGET/MSET  GETSET
    INCR/DECR INCRBY/DECRBY  INCRBYFLOAT DEL 
    STRLEN STRRANGE GETRANGE/SETRANGE

    缓存，计数器，消息队列，分布式锁

### 列表对象

    LPUSH/RPUSH LPOP/RPOP
    LINDEX LLEN 
    LINSERT LREM/LTRIM LSET

### 哈希

    HSET/HGET HEXISTS HDEL
    HLEN HGETALL

### 集合对象

    SADD/SCARD SISMEMBER SMEMBERS
    SRANDMEMBER SPOP SREM

### 有序集合对象

    ZADD/ZCARD ZCOUNT ZRANGE 
    ZREVRANGE ZRANK ZREVRANK
    ZREM ZSCORE

### 类型检查与命令多态

    DEL
    EXPIRE
    RENAME
    TYPE
    OBJECT


## 数据结构和对象

#### 2简单动态字符串 SDS simple dynamic string

    SET msg "hello string"
    RPUSH fruits "apple" "banana" "cherry"
    STRLEN msg

    struct sdsdr{
        int len;
        // 记录buf数组中未使用字节的数量
        int free;

        char buf[];
    }
    printf("%s", s->buf)
    
#### 缓存区溢出问题
    
    char *strcat(char *dest, const char *src);
    strcat(s, "cluster");

    sdcat(s, "cluster");
    

| c字符串  |    SDS   |
|  ---    |    ---   |
|获取字符串长度复杂度为 O(N) |  O(1)|
|API是不安全的，会造成缓冲区溢出| 安全的，不会溢出 |
|修改字符串长度N次必须执行N次内存重分配| 最多需要执行N次内存重分配|
|只能保存文本数据|可以保存文本和二进制数据|
|可以使用<string.h>库中所有函数| 使用部分函数|

### 3链表 

    LLEN integers
    LRANGE integers 0 10
    发布与订阅 慢查询 监视器等功能都用到链表

    typedef struct listNode {
        struct listNode *prev;
        struct listNode *next;

        void *value;
    }listNode;

    typedef struct list{
        listNode *head;
        listNode *tail;
        unsigned long len;
        void *(*dup) (void *ptr);  //节点复制
        void (*free) (void *ptr);  //节点释放
        int (*match) (void *ptr, void *key); //节点值对比
    } list;

#### redis链表实现的特性

    双端  链表节点带有 prev 和 next指针
    无环  prev 和 next指针都指向NULL，对链表的访问以NULL为终点
    带表头指针和表尾指针  head tail  O(1)
    带链表长度计数器  len O(1)
    多态  void*指针保存节点值

### 4字典  symbol table / associative array / map

    SET msg "hello dict"
    HLEN website
    HGETALL website

    //哈希表
    typedef struct dictht {

        dictEntry **table;
        unsigned long size;

        unsigned long sizemask;
        unsigned long used;
    } dictht;
    //哈希表节点
    typedef struct dictEntry{
        void *key;
        union {
            void *val;
            uint64_t u64;
            int64_t s64;
        } v;
        struct dictEntry *next;  //解决冲突问题
    } dictEntry;
    //字典
    typedef struct dict {
        dictType *type;
        void *privdate;
        dictht ht[2];
        int trehashidx; /* rehashing not in progress if rehashing == -1 */
    }
    typedef struct dictType{

    }dictType;

#### 哈希算法

    根据键值对的键计算出 哈希值 和 索引值
    redis 使用 MurmurHash2 算法来计算哈希值

#### 解决键冲突

    Redis的哈希表使用链地址法 separate chaining 来解决键冲突，每个哈希节点都有一个next指针

### 5跳跃表

    skiplist 一种有序数据结构，通过在每个节点中维持多个指向其他节点的指针，从而达到快速访问节点的目的
    Redis 使用跳跃表 作为有序集合键的底层实现之一 (在 集群节点 中用跳跃表作内部数据结构)

    ZRANGE fruit-price 0 2 WITHSCORES
    ZCARD fruit-price
    // 跳跃表节点
    typedef struct zskiplistNode {
        struct zskiplistNode *backward;
        double score;
        robj *obj;
        struct zskiplistLevel {
            struct zskiplistNode *forward;
            unsigned int span;
        } level[];
    }zskiplistNode;
    // 跳跃表信息 （表头节点， 表尾节点，长度）
    typedef struct zskiplist{
        structz zskiplistNode *header, *tail;
        unsigned long length;
        int level;
    } zskiplist;

### 6整数集合  intest 

    intset 是集合键的底层实现之一

    SADD numbers 1 3 5 7 9
    OBJECT ENCODING numbers

    整数集合是Redis用于保存整数值的集合抽象数据结构，可以保存类型为 int16_t, int32_t, int64_t的整数值，并且保证集合中不会出现重复元素
    typedef struct intset {
        uint32_t encoding;
        uint32_t length;
        int8_t contents[];
    } intset;

### 7压缩列表  ziplist

    RPUSH 1st 1 3 5 10086 "hello" "ziplist"
    OBJECT ENCODEING 1st

    HMSET profile "name" "ziplist" "hash" "job" "0916"
    OBJECT ENCODEING profile

    压缩列表是Reids为了节约内存而开发的，由一系列特殊编码的连续内存块组成的顺序型数据结构
    一个压缩列表可以包含多个节点 entry ，每个节点可以保存一个字节数组 或 一个整数值

| zlbytes |  zltail |  zllen  |  entry1  |  entry2  |  entryN |  zlend  |
|   ---   |   ---   |   ---   |   ---   |   ---   |   ---   |   ---   |
|uint32_t 4字节 | uint32_t 4字节 | uint16_t 2字节 | 列表节点 不定 |不定|不定| uint8_t 1字节 |