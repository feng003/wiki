---
title: "mysql show"
date: 2016-09-17 09:30
tags: mysql
---

> 1. show VARIABLES like "%slow%"

    show variables like 'log_output';  //确认日志信息输出到操作系统文件还是数据库的表中

    set global slow_query_log  = on;
    set global long_query_time = 1;
    set global slow_query_log_file = "/var/log/mysql-slow.log";

    show global status like 'Slow_queries';   // 查询出现慢查询次数的累计值

>  show

    show processlist;
    show status;
    show status like '%下面变量%';

    Connections 试图连接MySQL服务器的次数。

    Created_tmp_tables 当执行语句时，已经被创造了的隐含临时表的数量。
    Delayed_insert_threads 正在使用的延迟插入处理器线程的数量。
    Delayed_writes 用INSERT DELAYED写入的行数。
    Delayed_errors 用INSERT DELAYED写入的发生某些错误(可能重复键值)的行数。

    Flush_commands 执行FLUSH命令的次数。
    Handler_delete 请求从一张表中删除行的次数。
    Handler_read_first 请求读入表中第一行的次数。
    Handler_read_key 请求数字基于键读行。
    Handler_read_next 请求读入基于一个键的一行的次数。
    Handler_read_rnd 请求读入基于一个固定位置的一行的次数。
    Handler_update 请求更新表中一行的次数。
    Handler_write 请求向表中插入一行的次数。
    Key_blocks_used 用于关键字缓存的块的数量。

>  my.conf

    [mysqld]

    log-slow-queries = /usr/local/mysql/var/slowquery.log
    long_query_time = 1  #单位是秒
    log-queries-not-using-indexes

> mysqldumpslow –help

    mysql/bin目录，输入 mysqldumpslow –help 或--help可以看到这个工具的参数

    mysqldumpslow  [ OPTS... ] [ LOGS... ]

    -s  ORDER     what to sort by (t, at, l, al, r, ar etc), 'at' is default

    -t NUM       just show the top n queries

    -r           reverse the sort order (largest last instead of first)

    mysqldumpslow -s c -t 20 host-slow.log
    mysqldumpslow -s r -t 20 host-slow.log


[参考文档1](http://blog.csdn.net/wenbingcai/article/details/40340867)

[参考文档2](http://blog.chinaunix.net/uid-16844903-id-332524.html)

[参考地址3](http://www.2cto.com/database/201210/162509.html)

[参考文档4](http://blog.jobbole.com/105792/)
