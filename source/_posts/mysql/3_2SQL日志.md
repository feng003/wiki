---
title: "3_2SQL日志"
date: 2020-03-10
tag: "binlog slow query log redo log undo log" 
---

> 二进制日志(binlog)

- [x]     格式


    statuement SQL语句的原文
    row 两个 event：Table_map 和 Delete_rows
    mixed
- [x]     命令


    show binary logs;

    show global variables like '%log_bin%';
    +---------------------------------+--------------------------------+
    | Variable_name                   | Value                          |
    +---------------------------------+--------------------------------+
    | log_bin                         | ON                             |
    | log_bin_basename                | /var/lib/mysql/mysql-bin       |
    | log_bin_index                   | /var/lib/mysql/mysql-bin.index |
    | log_bin_trust_function_creators | OFF                            |
    | log_bin_use_v1_row_events       | OFF                            |
    +---------------------------------+--------------------------------+

    reset master; //清空binlog日志文件 
    show master/slave status;
    flush logs;
    mysqlbinlog bin-log.000001; //查看binlog日志
    show binlog events in 'mysql-bin.000009'; //命令行解析

- [x]     配置


    /etc/my.cnf.d/server.cnf
    [mysqld]
    # bin_log
    log_bin          = /var/lib/mysql/bin-log
    log_bin_index    = /var/lib/mysql/mysql-bin.index
    expire_logs_days = 28
    server_id        = 0001
    binlog_format    = ROW

    systemctl restart mariadb.service
- [x]     恢复(指定pos点恢复/部分恢复)


    mysqlbinlog   --start-position=1847  --stop-position=2585  mysql-bin.000008  > test.sql
    mysql> source /var/lib/mysql/3306/test.sql
    
    
[为 MySQL/MariaDB 开启 Binlog 功能](https://www.mf8.biz/enable-binlog/)

> 慢查询日志(slow query log)


    show variables like '%query%'
    +------------------------------+--------------------------+
    | Variable_name                | Value                    |
    +------------------------------+--------------------------+
    | have_query_cache             | YES                      |
    | long_query_time              | 10.000000                |
    | slow_query_log               | OFF                      |
    | slow_query_log_file          | VM_33_92_centos-slow.log |
    +------------------------------+--------------------------+

    /etc/my.cnf.d/server.cnf
    # query_log
    slow_query_log  = 1
    long_query_time = 2
    slow_query_log_file = /tmp/logs/slow_query.log

> 错误日志(error log)


> 一般查询日志(general log)


> 重做日志(redo log)


    作用：确保事务的持久性
    redo log 是innodb层产生的，只记录该存储引擎中表的修改。

    show variables like 'innodb_log%';
    +---------------------------+---------+
    | Variable_name             | Value   |
    +---------------------------+---------+
    | innodb_log_block_size     | 512     |
    | innodb_log_buffer_size    | 8388608 |
    | innodb_log_file_size      | 5242880 |
    | innodb_log_files_in_group | 2       |
    | innodb_log_group_home_dir | ./      |
    +---------------------------+---------+


> 回滚日志(undo log)


    show variables like 'innodb_data%';
    +-----------------------+------------------------+
    | Variable_name         | Value                  |
    +-----------------------+------------------------+
    | innodb_data_file_path | ibdata1:10M:autoextend |
    | innodb_data_home_dir  |                        |
    +-----------------------+------------------------+


> 中转日志(relay log)


    show variables like '%relay%';
    +---------------------------+--------------------------------------+
    | Variable_name             | Value                                |
    +---------------------------+--------------------------------------+
    | max_relay_log_size        | 0                                    |
    | relay_log                 | slave-relay-bin                      |
    | relay_log_basename        | /var/lib/mysql/slave-relay-bin       |
    | relay_log_index           | /var/lib/mysql/slave-relay-bin.index |
    | relay_log_info_file       | relay-log.info                       |
    | relay_log_info_repository | FILE                                 |
    | relay_log_purge           | ON                                   |
    | relay_log_recovery        | OFF                                  |
    | relay_log_space_limit     | 0                                    |
    | sync_relay_log            | 10000                                |
    | sync_relay_log_info       | 10000                                |
    +---------------------------+--------------------------------------+

[mysql](https://www.cnblogs.com/wy123/category/1243550.html)

[带你深入解析 MySQL binlog](https://zhuanlan.zhihu.com/p/33504555)

[详细分析MySQL事务日志(redo log和undo log)](https://juejin.im/entry/5ba0a254e51d450e735e4a1f)