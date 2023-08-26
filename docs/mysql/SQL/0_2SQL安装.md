---
title: "0_2SQL安装"
date: 2020-03-11
tag: "install" 
---

> mysql install


    sudo apt-get install mysql-server
    sudo apt install mysql-client
    sudo apt install libmysqlclient-dev
    
#####   安装成功后可以通过下面的命令测试是否安装成功：

    sudo netstat -tap | grep mysql
    
#####    现在设置mysql允许远程访问，首先编辑文件/etc/mysql/mysql.conf.d/mysqld.cnf：

    sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf
    注释掉bind-address = 127.0.0.1

    grant all on *.* to root@'%' identified by '你的密码' with grant option;
    flush privileges;    

##### Ubuntu中安装MySQL，更改默认用户密码
    
- [ ] 进入目录：cd /etc/mysql,查看debian.cnf文件

- [ ] mysql -u debian-sys-maint -p 

- [ ] 设置账号密码:


    update user set authentication_string=PASSWORD("自定义新密码") where user='root';

- [ ] 执行一下语句：


    update user set plugin="mysql_native_password";
    flush privileges
    /etc/init.d/mysql restart; 


[Ubuntu16.04彻底卸载MySQL](
https://www.cnblogs.com/mjhblog/p/10499772.html)

[CentOS7安装mysql后无法启动服务，提示Unit not found](https://my.oschina.net/iyinghui/blog/2246986)