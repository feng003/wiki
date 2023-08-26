---
title: "3_1SQL用户权限"
date: 2020-03-11
tag: "用户权限 grant revoke" 
---

> 用户权限管理

- [x] 用户


    //查看用户
    select * from mysql.user;
    select host,user from mysql.user;

    //添加 删除用户
    create user 'guest@localhost' IDENTIFIED by "guest";
    DROP user 'guest@localhost';

    create user 'guest'@'localhost' IDENTIFIED by "guest";
    create user 'guest'@'x.x.x.x' identified by 'guest';
    FLUSH PRIVILEGES;

    //账号重命名
    rename user '旧用户名'@'旧主机' to '新用户名'@'新主机'
    rename user 'test'@'localhost' to 'txt'@'localhost';

    //修改密码
    set password for '用户名'@'主机' = password("new password")
    FLUSH PRIVILEGES;

    grant select on 数据库名.数据表名 to 用户名@主机 identified by '新密码' with grant option;

- [x] 权限


    //查看权限
    show grants;
    show grants for guest@localhost;

    授予权限：
    格式：grant 权限列表 on 数据库名.数据表名 to '用户名'@'主机' identified by '密码' with grant option; 
    // * 表示所有数据库或所有数据表; % 表示任何主机地址

    grant all PRIVILEGES on *.* to 'guest'@'localhost' IDENTIFIED BY 'guest' with grant option;
    grant all PRIVILEGES on *.* to 'guest'@'%' IDENTIFIED BY 'guest' with grant option;
    grant replication slave,replication client on *.* to 'guest'@'%' identified by 'guest';

    收回权限：
    格式：revoke 权限列表 on 数据库名.数据库表 from 用户名@主机

    revoke drop on *.* from guest@localhost;   //回收drop权限
    revoke insert on *.* from guest@localhost; //回收insert权限
    
    
    

权限         | 权限级别 | 权限说明
---          |      --- | ---
alter        | 表        | 更改表 添加、修改字段或索引
delete       | 表        | 删除数据
index        | 表        | 索引
insert       | 表        | 插入
select       | 表        | 查询
update       | 表        | 更新
create view  | 视图      | 创建视图
show view    | 视图      | 查看视图
create routine | 存储过程 |  创建存储过程
alter routine| 存储过程   |  更改存储过程
execute      | 存储过程   |  执行存储过程
create       | 数据库、表、索引 | 创建数据库、表、索引
drop         | 数据库、表 |  删除数据库、表
grant option | 数据库、表 保存的程序 | 授予权限
references   | 数据库、表 | 外键
file         | 文件访问   | 文件访问
create temporary tables | 服务器管理  | 创建临时表
locl tables  | 服务器管理 | 锁表
create user  | 服务器管理 | 创建用户
process      | 服务器管理 | 查看进程
reload       | 服务器管理 | flush-hosts、flush*
replication client | 服务器管理 | 复制
replication slave  | 服务器管理 | 复制
show database      | 服务器管理 | 查看数据库
shutdown     | 服务器管理 | 关闭数据库
super        | 服务器管理 | 执行kill线程