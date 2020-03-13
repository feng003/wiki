---
title: "2_5SQL主从同步"
date: 2020-03-12
tag: "主从同步 master slave" 
---

> 主从同步 配置

##### master 主服务器

    log-bin=mysql-bin 
    server-id = 0001 
    binlog-do-db            = project
    binlog-ignore-db        = mysql,information_schema,performance_schema
    binlog_format           = mixed


    select version();
    +-----------------------------+
    | version()                   |
    +-----------------------------+
    | 5.7.28-0ubuntu0.16.04.2-log |
    +-----------------------------+
    show master status;
    +------------------+----------+--------------+---------------------------------------------+-------------------+
    | File             | Position | Binlog_Do_DB | Binlog_Ignore_DB                            | Executed_Gtid_Set |
    +------------------+----------+--------------+---------------------------------------------+-------------------+
    | mysql-bin.000007 |      154 | project      | mysql,information_schema,performance_schema |                   |
    +------------------+----------+--------------+---------------------------------------------+-------------------+
    
##### slave 从服务器

    log_bin         = mysql-bin
    server_id       = 2
    expire_logs_days= 28
    binlog_format   = ROW

    select version();
    +----------------+
    | version()      |
    +----------------+
    | 5.5.64-MariaDB |
    +----------------+

    change master to master_host='118.126.112.138', master_user='guest',
    master_password='guest',
    master_log_file='mysql-bin.0000007',
    master_log_pos=154;

    start slave;
    stop slave;
    show slave status\G;
    //Slave_IO_Running: Yes    //此状态必须YES
      Slave_SQL_Running: Yes     //此状态必须YES
        Slave_IO_State: 
                      Master_Host: 118.126.112.138
                      Master_User: guest
                      Master_Port: 3306
                    Connect_Retry: 60
                  Master_Log_File: mysql-bin.0000007
              Read_Master_Log_Pos: 154
                   Relay_Log_File: mariadb-relay-bin.000001
                    Relay_Log_Pos: 4
            Relay_Master_Log_File: mysql-bin.0000007
                 Slave_IO_Running: No
                Slave_SQL_Running: No
                  Replicate_Do_DB: 
              Replicate_Ignore_DB: 
               Replicate_Do_Table: 
           Replicate_Ignore_Table: 
          Replicate_Wild_Do_Table: 
      Replicate_Wild_Ignore_Table: 
                       Last_Errno: 0
                       Last_Error: 
                     Skip_Counter: 0
              Exec_Master_Log_Pos: 154
                  Relay_Log_Space: 245
                  Until_Condition: None
                   Until_Log_File: 
                    Until_Log_Pos: 0
               Master_SSL_Allowed: No
               Master_SSL_CA_File: 
               Master_SSL_CA_Path: 
                  Master_SSL_Cert: 
                Master_SSL_Cipher: 
                   Master_SSL_Key: 
            Seconds_Behind_Master: NULL
    Master_SSL_Verify_Server_Cert: No
                    Last_IO_Errno: 2003
                    Last_IO_Error: error connecting to master 'guest@118.126.112.138:3306' - retry-time: 60  retries: 86400  message: Can't connect to MySQL server on '118.126.112.138' (4)
                   Last_SQL_Errno: 0
                   Last_SQL_Error: 
      Replicate_Ignore_Server_Ids: 
                 Master_Server_Id: 0

[mysql主从复制配置](https://blog.csdn.net/Wuhaotian1996/article/details/84313193)

> 主服务器Master 处理写请求，从服务器Slave处理读请求

##### 为什么需要主从同步

    主从同步设计不仅可以提高数据库的吞吐量
    可以读写分离
    数据备份(热备份机制)
    高可用性
    
##### 主从同步的原理 Binlog二进制日志

    主从同步的原理就是基于Binlog进行数据同步，在主从复制过程中，会基于3个线程来操作，一个主库线程，两个从库线程

    二进制日志转储线程(Binlog dump thread) 是一个主库线程。当从库线程连接的时候，主库可以将二进制日志发送给从库，当主库读取事件的时候，会在Binlog上加锁，读取完成之后，再讲锁释放掉。

    从库 I/O 线程会连接到主库，向主库发送请求更新 Binlog。这时从库的 I/O 线程就可以读取到主库的二进制日志转储线程发送的 Binlog 更新部分，并且拷贝到本地形成中继日志（Relay log）。

    从库 SQL 线程会读取从库中的中继日志，并且执行日志中的事件，从而将从库中的数据与主库保持同步。
    
![主从同步](73A605A9D5CD4FDB8237C5B6DF1AF243)

##### 如何解决主从同步的数据一致性问题

- [x]     异步复制


    客户端提交 COMMIT 之后不需要等从库返回任何结果，而是直接将结果返回给客户端，
    
- [x]     半同步复制


    客户端提交 COMMIT 之后不直接将结果返回给客户端，而是等待至少有一个从库接收到了 Binlog，并且写入到中继日志中，再返回给客户端

![半同步复制](49E4FBBC7D3B4B20BA515765001AC0B3)

- [x]     组复制 MGR Mysql Group Replication 


    基于Paxos协议的状态机复制

> 数据恢复

##### InnoDB存储引擎的表空间