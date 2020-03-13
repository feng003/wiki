---
title: "2_2SQL索引"
date: 2020-03-13
tag: "索引 聚集索引 非聚集索引 联合索引" 
---

> 索引


    利 提升查询效率
    弊 维护索引所需的代价(占用存储空间、降低数据库写操作的性能)

#### 索引是万能的吗？

- [x] 数据行数少的情况下

- [x] 性别(男女)字段不应该创建索引(区分度)


#### 索引种类

###### 功能逻辑上分为

- [x] 普通索引

- [x] 唯一索引 UNIQUE

- [x] 主键索引 NOT NULL + UNIQUE

- [x] 全文索引

###### 物理实现方式分为

- [x] 聚集索引 可以按照主键来排序存储数据

- [x] 非聚集索引(二级索引或辅助索引)


    在数据库系统会有单独的存储空间存放非聚集索引，这些索引项是按照顺序存储的，但索引项指向的内容是随机存储的。
    也就是说系统会进行两次查找，第一次先找到索引，第二次找到索引对应的位置取出数据行。
    非聚集索引不会把索引指向的内容像聚集索引一样直接放到索引的后面，而是维护单独的索引表（只维护索引，不维护索引指向的数据），为数据检索提供方便。

    二者区别：
    聚集索引的叶子节点存储的是数据记录，非聚集索引的叶子节点存储的数据位置。非聚集索引不会影响数据表的物理存储顺序
    
- [x] MyISAM 和 InnoDB都是用B+树来实现索引


    MyISAM的索引与数据是 分开存储的(叫做非聚集索引 Unclustered Index)
    MyISAM的索引叶子 存储指针，主键索引和普通索引无太大区别

    InnoDB 的聚集索引 和 数据行统一存储 (Clustered Index)
    InnoDB 的聚集索引 存储数据行本身，普通索引 存储主键
    InnoDB 一定 有且只有一个 聚集索引


[索引，一文搞定 | 数据库系列](https://mp.weixin.qq.com/s?__biz=MjM5ODYxMDA5OQ==&mid=2651962936&idx=1&sn=2f4a97187134ed584273550104672694&chksm=bd2d0be48a5a82f2e5703e55272f6e3ee60954efd8d08b46232c3e24ec5632691ccc66973553&mpshare=1&scene=1&srcid=&sharer_sharetime=1572050350742&sharer_shareid=1cd8d1b19020a748afda300e4bbbbe41#rd)


###### 按照字段个数进行划分

- [x] 单一索引

- [x] 联合索引 最左匹配原则


    1）SQL条件语句中的字段顺序不重要，因为在逻辑查询优化阶段会自动进行 查询重写。
    2）如果我们遇到了范围条件查询，比如<、<=、>、>=、between等。那么范围列后的列就无法使用到索引

---

> 索引的原理 B+树

##### B树 Balance Tree 平衡的多路搜索树

    适用于 文件系统和数据库系统中的索引结构
    
##### B+树

---


> Hash索引(散列函数) 在键值型（Key-Value）数据库中，Redis 存储的核心就是 Hash 表。

##### 统计Hash检索效率

    hash 检索数据的算法复杂度为O(1)
    数组 检索数据的算法复杂度为O(n)

##### Mysql中的Hash索引

##### Hash索引 和 B+树索引区别

    Hash索引不能进行范围查询，而B+树可以。Hash索引指向的数据是无序的，而B+树的叶子节点是个有序的链表

    Hash索引不支持联合索引的最左侧原则，而B+树可以。

    Hash索引不支持Order by、Group by、< >  排序，因为Hash索引指向的数据是无序的，无法起到排序优化的作用。无法用Hash索引进行模糊查询。


---

> 索引的使用原则

##### 创建索引的规律

- [x] 字段的数值有唯一性的限制 比如用户名

- [x] 频繁作为WHERE 查询条件的字段

- [x] 需要经常GROUP BY 和 ORDER BY的列

- [x] UPDATE DELETE 的WHERE 条件列，一般需要创建索引

- [x] DISTINCT 字段需要创建索引

- [x] 多表join连接操作时


    连接表的数据尽量不要超过3张，每增加一张表就相当于增加了一次嵌套的循环
    对WHERE 条件创建索引
    对用于连接的字段创建索引，并且该字段在多张表中的类型必须一致

##### 不需要创建索引

    WHERE条件(包括group by、order by)里用不到的字段不需要创建索引。

    表记录太少，比如少于1000个

    字段中如果有大量重复数据，比如性别字段

    频繁更新的字段不一定要创建索引

##### 索引失效

- [x] 索引进行了表达式计算

- [x] 对索引使用函数

- [x] 在where子句中，如果在OR前的条件列进行了索引，而在OR后的条件列没有进行索引，那么索引失效

- [x] 使用LIKE进行模糊查询的时候，前面不能是 %

- [x] 索引列尽量设置为 NOT NULL约束

- [x] 使用联合索引的时候注意最左原则



---

    查找 不经常使用的索引
    SELECT OBJECT_SCHEMA, OBJECT_NAME, INDEX_NAME,COUNT_STAR FROM performance_schema.table_io_waits_summary_by_index_usage
    WHERE INDEX_NAME IS NOT NULL
    AND COUNT_STAR = 0
    AND OBJECT_SCHEMA != 'mysql' AND OBJECT_SCHEMA != 'performance_schema'
    

---

> 数据页的角度理解B+


    索引信息以及数据记录都保存在文件上，确切说是存储在页结构上

##### 数据库中存储结构

![行、页、区、段、表空间的关系](BA6D987D717644139910BBA3E379579A)

    一个表空间包括一个或多个段
    一个段包括了一个或多个区
    一个区(64个连续的页 1M)包括了多个页
    一个页(Innodb 中默认大小是 16Kb)中可以存储多个行记录(Row)

    查看Innodb的表空间类型：
        show variables like 'innodb_file_per_table';
        innodb_file_per_table=ON，这就意味着每张表都会单独保存为一个.ibd 文件

##### Page

    常见的有数据页(保存B+树节点)、系统页、Undo页和事务数据页

    Mysql的InnoDB存储引擎中，默认页的大小是16Kb
        show variables like '%innodb_page_size%';
        
    数据库I/O操作的最小单元是页。数据页包括七个部分，分别是：
    文件头(file header  38b)
    页头 (page header 56b)
    最大最小记录 (infimun + supermum 26b)
    用户记录 (user records)
    空闲空间 (free space)
    页目录 (page directory)
    文件尾 (file tailer 8b)

##### 从数据页的角度来看，B+树如何进行查询

    一颗B+树按照节点类型可以分成两部分
    叶子节点 B+树最底层的节点，节点的高度为0，存储行记录
    非叶子节点 节点的高度大于0，存储索引键和页面指针，并不存储行记录本身
    
- [x] B+树如何进行记录检索的

- [x] 普通索引和唯一索引在查询效率上有什么不同

---

> 磁盘I/O角度理解SQL查询的成本



    SELECT comment_id, comment_text, user_id FROM product_comment where user_id BETWEEN 100000 AND 200000
    