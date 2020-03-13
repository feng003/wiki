---
title: "1_5SQL事务"
date: 2020-03-11
tag: "事务"
---

> transaction 事务

##### 事务的特性 ACID

- [x] Atomicicty

- [x] Consistency

- [x] Isolation

- [x] Durability


##### 事务的控制

    show engines;

    START TRANSCATION/BEGIN 显示开启一个事务
    COMMIT 提交事务
    ROLLBACK/ROLLBACK TO [SAVEPOINT] 回滚事务
    SAVEPOINT 事务中创建保存点
    RELEASE SAVEPOINT 删除某个保存点
    SET TRANSACTION   设置事务的隔离级别

    隐式事务和显示事务
    set autocommit = 0;//关闭自动提交
    set autocommit = 1;//开启自动提交

    completion_type=0 // COMMIT
    completion_type=1 // COMMIT AND CHAIN
    completion_type=2 // COMMIT AND RELEASE
    
> 事务隔离

- [x] 四种隔离级别


    Read Uncommitted 
        读取未提交的数据 称为dirty read
    Read committed 
        不可重复读 在一个事务的两次查询之中数据不一致。这可能是两次查询过程中间插入一个事务更新的原有数据
    Repeatable Read
        幻读 指当用户读取某一范围的数据行时，另一事务又在该范围内插入新行，当用户再读取改范围的数据行时，会发现有新的 "幻影"行
    Serializable

    CREATE TABLE `test` ( 
      `id` int(11) UNSIGNED NOT NULL auto_increment, 
      `num` int(11) NOT NULL DEFAULT 0, 
      `a` int(11) DEFAULT NULL, 
      PRIMARY KEY (`id`) 
    ) ENGINE=InnoDB DEFAULT CHARSET=utf8;

    DROP PROCEDURE IF EXISTS test_initData; 
    DELIMITER $ 
    CREATE PROCEDURE test_initData() 
    BEGIN 
        DECLARE i INT DEFAULT 1; 
        WHILE i<=10 DO 
            INSERT INTO test(num,a) VALUES(i*2,i); 
            SET i = i+1; 
        END WHILE; 
    END $ 
    CALL test_initData();

    SELECT @@tx_isolation;//查看隔离级别
    //幻读
    事务A start TRANSACTION;
          select * from test ;
    事务B start TRANSACTION; 
          update test set num=10 where id=1;
          select * from test ;
    事务A select * from test ; //数据未被修改
    事务B COMMIT;
    事务A select * from test; //数据未被修改
    事务B INSERT into test (num) VALUE (11);
    事务A select * from test; //读的不是最新数据
    事务A COMMIT;
          select * from test;
    //不可重复读
    set session transaction isolation level read committed;
    事务A start TRANSACTION;
          select * from test ;
    事务B start TRANSACTION; 
          update test set num=9 where id=1;
          select * from test ;
    事务A select * from test ; //数据未被修改
    事务B COMMIT;
    事务A select * from test; //数据发生变化，说明B提交的修改被事务A读到了
    //脏读
    set session transaction isolation level read uncommitted;
    事务A start TRANSACTION;
          select * from test ;
    事务B start TRANSACTION; 
          update test set num=7 where id=1;
    事务A select * from test ; //数据已修改 "脏读"
    事务B ROLLBACK;
    事务A select * from test ; //数据回到初始状态

[理解Mysql的四种隔离级别](https://www.jianshu.com/p/8d735db9c2c0)