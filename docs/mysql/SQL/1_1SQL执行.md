---
title: "1_1SQL执行"
date: 2020-03-2
tag: "DDL DML DQL DCL" 
---

> SQL语法基础 structured query language

##### DDL Data Definition Language

    数据定义语言，定义数据库对象。包括数据库、数据表和列。

##### DML Data Manipulation Language

    数据操作语言 操作和数据库相关的记录

##### DCL Data Control Language 

    数据控制语言 定义访问权限和安全级别

##### DQL Data Query Language

    数据查询语言 查询语句
    
> ER 图 Entity RelatiionShiop Diagram


    实体 管理的对象
    属性 每个实体的属性
    关系 对象之间的关系

    表名、表别名、字段名、字段别名等都小写
    sql保留字、函数名、绑定变量等都大写
    
[MySQL开发规范](https://zerolee1993.github.io/mysql-guide)

---

> DBMS DataBase Management System


    关系型数据库 Oracle Mysql SQL Server

    键值型数据库 用于内容缓存 redis
        优点是查找速度快，缺点是不能使用条件过滤(where),查找数据需要遍历所有的键

    文档型数据库 一个文档就相当于一条记录 mongodb

    搜索引擎 全文搜索 倒排索引 elasticsearch

    列存储 (相对于行式数据库)  将数据按照列存储到数据库中，可以降低系统的I/O 适合于分布式文件系统

    图形数据库 存储实体(对象)之间的关系，比如社交网络中人与人的关系，数据模型主要是以节点和边来实现

    时序数据库 存储时间序列的信息，应用于时间序列预测等场景。比如股市的预测，比特币预测等，也可以预测交通流量，PM2.5，都是包含有时间维度的
    

---

> SQL 如何执行的

##### Oracle 中的SQL是如何执行的

![SQL执行流程](297E1331591844F2B57C37158D94DBA1)


    Oracle 采用了共享池来判断 SQL 语句是否存在缓存和执行计划，通过这一步骤我们可以知道应该采用硬解析还是软解析
    //硬解析
    select * from player where player_id = 10001;
    //绑定变量 软解析
    select * from player where player_id = :player_id;
    

##### MySQL 中的SQL是如何执行的

![Mysql执行流程](B39A046AA35D4B43A08FA234A07692AD)

    Client/Server架构 服务器端程序使用 mysqld
    连接层 客户端和服务器端建立连接，客户端发送SQL至服务器端
    SQL层 对SQL语句进行查询处理
    存储引擎层 与数据库文件打交道，负责数据的存储和读取
    
![SQL结构](C175AF62A1184D00BC739328D5620ED6)

    与 Oracle 不同的是，MySQL 的存储引擎采用了插件的形式，每个存储引擎都面向一种特定的数据库应用环境。
    
##### 存储引擎

    InnoDB 支持事务、行级锁定、外键约束
    MyIsam
    Memory
    NDB
    Archive


    select @@profiling;
    profiling=0 关闭
    set profiling=1; //打开
    show profiles;
    select version()
    
    
![MYSQL](E560FBD6C33C46759A316161D638DE1E)


![SGA](08527B54568A467B82376C1F3DBF6211)

---

> DDL 创建数据库&数据表

##### DDL 基础语法及设计工具

    CREATE DROP ALTER
    CREATE DATABASE product;
    DROP DATABASE product;
    create table product (
        `product_id` int not null auto_increment,
        `name` varchar(64) not null,
        `price` float(6,2) null default 0.00,
        primary key(`product_id`) USING BTREE,
        unique index `name` USING BTREE
    ) engine=innodb character set =uft8 collate=utf8_general_ci row_format=Dynamic

    添加
    ALTER table product ADD (created timestamp() not null );
    修改
    ALTER table product RENAME Column name to title;
    ALTER table product MODIFY price decimal(12,2);
    删除
    alter table product DROP column name;
    添加索引
    ALTER TABLE table_name ADD INDEX idx_column1_column2(`column1`,`column2`);
    ALTER TABLE table_name ADD UNIQUE uk_column1_column2(`column1`,`column2`);
    删除索引
    ALTER TABLE table_name DROP INDEX idx_xxx;

##### DDL 定义数据表 约束性

    主键约束 unique + not null
    外键约束
    字段约束
    唯一性约束
    NOT NULL约束
    DEFAUL默认值
    CHECK约束

##### DDL 设计数据库原则

    数据表的个数越少越好

    数据表中的字段个数越少越好

    数据表中联合主键的字段个数越少越好

    使用主键和外键越多越好