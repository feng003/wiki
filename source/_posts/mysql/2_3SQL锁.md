---
title: "2_3SQL锁"
date: 2020-03-08
tag: "锁" 
---

> 锁

##### 锁的划分

- [x] 按照颗粒度进行划分


    行锁
    页锁
    表锁
    
- [x] 从数据库管理的角度对锁进行划分


    共享锁/读锁(S锁) 共享锁锁定的资源可以被其他用户读取，但不能修改
        SELECT comment_id, product_id, comment_text, user_id FROM product_comment WHERE user_id = 912178 LOCK IN SHARE MODE
    排它锁/写锁(X锁) 排它锁锁定的数据只允许进行锁定操作的事务使用，其他事务无法对已锁定的数据进行查询或修改
        //数据表添加排它锁
        LOCK TABLE product_comment WRITE;
        UNLOCK TABLE;
        //数据行添加排它锁
        SELECT comment_id, product_id, comment_text, user_id FROM product_comment WHERE user_id = 912178 FOR UPDATE;

- [x] 从看待数据并发的思维方式


    乐观锁 认为对同一数据的并发操作不会总发生，属于小概率事件，不用每次都对数据上锁，不采用数据库自身的锁机制，通过程序来实现。采用版本号机制或者时间戳机制

    悲观锁 对数据被其他事务的修改持保守态度，会通过数据库自身的锁机制来实现，从而保证数据操作的排它性
    
> MVCC 采用乐观锁思想的一种方式


##### Multiversion Concurrency Control 多版本并发控制技术

    快照读 读取的是快照数据
        SELECT * FROM player WHERE ...
    当前读 读取最新数据。加锁的select 或者对数据进行增删改都会进行当前读
        SELECT * FROM player LOCK IN SHARE MODE;
        SELECT * FROM player FOR UPDATE;
        INSERT INTO player values ...
        DELETE FROM player WHERE ...
        UPDATE player SET ...

##### InnoDB中的MVCCC是如何实现的

    Multi Version 数据包括事务版本号、行记录中的隐藏列和Undo Log
    事务版本号 事务ID
    
- [x] 行记录的隐藏列


    Innodb的叶子段存储了数据页，数据页中保存了行记录，而行记录中字段有：
        db_row_id 隐藏的行ID 生产默认聚集索引
        db_trx_id 操作这个数据的事务ID 
        db_roll_ptr 回滚指针 指向这个记录的Undo Log信息
    
- [x] Undo Log InnoDB将行记录快照保存在Undo Log里

##### Read View 是如何工作的

    Read View 保存了不应该让这个事务看到的其他事务ID列表
    Read View 中几个重要的属性
        trx_ids         系统当前正在活跃的事务ID集合
        low_limit_id    活跃的事务中最大的事务ID
        up_limit_id     活跃的事务中最小的事务ID
        creator_trx_id  创建这个Read View 的事务ID

    假设当前有事务 creator_trx_id 想要读取某个行记录，这个行记录的事务 ID 为 trx_id，那么会出现以下几种情况。

    如果 trx_id < 活跃的最小事务 ID（up_limit_id），也就是说这个行记录在这些活跃的事务创建之前就已经提交了，那么这个行记录对该事务是可见的。

    如果 trx_id > 活跃的最大事务 ID（low_limit_id），这说明该行记录在这些活跃的事务创建之后才创建，那么这个行记录对当前事务不可见。

    如果 up_limit_id < trx_id < low_limit_id，说明该行记录所在的事务 trx_id 在目前 creator_trx_id 这个事务创建的时候，可能还处于活跃的状态，因此我们需要在 trx_ids 集合中进行遍历，如果 trx_id 存在于 trx_ids 集合中，证明这个事务 trx_id 还处于活跃状态，不可见。否则，如果 trx_id 不存在于 trx_ids 集合中，证明事务 trx_id 已经提交了，该行记录可见。
    
##### InnoDB 是如何解决幻读

    在可重复读的情况下，InnoDB通过Next-Key锁 + MVCC 来解决幻读
    InnoDb三种行锁的方式
        记录锁 Record Locking 针对当个行记录添加锁
        间隙锁 Gap Locking 锁住一个范围(索引之间的间隙)，不包括记录本身
        Next-key锁 锁定一个范围，同时锁定记录本身，相当于间隙锁+记录锁
