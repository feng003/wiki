---
title: 'mysql  index type'
date: 2016-09-17 09:00
tags: mysql
---

> mysql index type

1. 普通索引

        create index indexname ON table(username(length));

        alter table add index [indexname] ON (username(length));

        create table tablenane(
            id int not null,
            username varchar(16) not null,
            index [indexname] (username(length))
        );

    drop index [indexname] ON table;

2. 唯一索引 唯一索引值必须唯一，但准许有空值。

        create unique index [indexname] on table(username(length));

3. 主键索引 primary key

4. 组合索引 将 name, city, age建到一个索引。

        create table tablenane(
            id int not null,
            username varchar(16) not null,
            city varchar(50) not null,
            age int not null
        ),

        alter table tablename add index name_city_age(name(10),city,age);

#### 建立这样的组合索引，其实是相当于分别建立了下面三组组合索引：

    username city age
    username city
    username

[参考文档](http://database.51cto.com/art/200910/156685.htm)
