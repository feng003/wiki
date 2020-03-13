---
title: "5_2SQLErrpr"
date: 2020-03-13
tag: "error" 
---

> 主从同步 

##### Last_Errno: 1594

##### Last_IO_Errno: 1236

    binlog日志文件不存在，或者binlog的名称没有写对

    change master to master_host='x.x.x.x', master_user='guest', master_password='guest', master_log_file='mysql-bin.000009', master_log_pos=245;

    master_log_file
    master_log_pos
    
##### Slave I/O: Master command COM_REGISTER_SLAVE failed 

    master 主机权限问题
    grant replication slave,replication client on *.* to 'guest'@'%' identified by 'guest';


[MySQL复制异常大扫盲](https://dbaplus.cn/news-11-1289-1.html)

[Mysql主从复制搭建](https://www.cnblogs.com/jtfr/p/10533769.html)

[Binlog中最容易踩到的坑](https://www.cnblogs.com/276815076/p/7993712.html)