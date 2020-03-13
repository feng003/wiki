---
title: "mysql lock mechanism"
date: 2016-09-17 09:30
tags: mysql
---

> mysql 多用户 多线程的关系型数据库


1. 一次性封锁 与 两段锁

    一次性封锁：sql语句的开始执行的时候，已预先知道要涉及那些数据，然后全部锁住，执行完毕之后，再全部解锁！

    两段锁：每个事务的执行分为两个阶段：加锁阶段 和 解锁阶段。

    加锁阶段：在对任何数据进行读操作之前要申请并获得S锁(读锁 共享锁)，进行写操作之前要申请并获得X锁(写锁 排它锁)。加锁不成功，则事务进入等待状态，直到加锁成功才继续执行。

    解锁阶段：当事务释放一个封锁以后，事务进入解锁阶段。



2. 锁定机制

#### 行级锁定

    innodb的锁是建立在索引基础上的，必要的时候会由行锁升级为表锁，所以，innodb既支持表锁也支持行锁

    由于锁定资源的颗粒度很小，所以每次获取锁和释放锁需要做的事情也很多，带来的消耗自然也就更大。行级锁定也最容易死锁。

#### 表级锁定

    myisam

#### 页级锁定

    BDB存储引擎采用的是页面锁（page-level locking），但也支持表级锁



3. 实例

        lock table tablename write;        unlock tables;

        lock table tablename read;         unlock tables;

        lock table tablename read local;    unlock tables;

[参考文档](https://segmentfault.com/a/1190000004507047)

[参考文档](http://tech.meituan.com/innodb-lock.html)
