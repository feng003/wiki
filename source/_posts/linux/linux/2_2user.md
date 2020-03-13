> 用户与权限管理

- [x] useradd 


    useradd guest
    id root 
    id guest

    vim /etc/passwd
    tail -10 /etc/shadow
    
- [x] userdel


    userdel -r guest

- [x] passwd


    passwd guest

- [x] usermod


    usermod -d /home/guest1 guest
    usermod -g group1 user1
    
- [x] chage


- [x] groupadd 
 

- [x] groupdel


- [x] su


    su - guest 使用login shell 方式切换用户

- [x] sudo


    visudo
    sudo vim /etc/sudoers  赋予普通用户权限
    
> 用户和用户组 /etc/passwd /etc/shadow /etc/group


- [x]     /etc/passwd


    root: x: 0: 0: root: /root: /bin/bash
    名称：是否设置密码：uid：gid：注释：用户根目录：用户登录后命令解释器(/sbin/nologin 用户不能登录)
- [x]     /etc/shadow


    root: ! :16462: 0: 99999: 7: : :
- [x]     /etc/group


    root: x :0 :
    
> 文件和目录权限


    类型 权限      所属用户和组                   文件名
    -rw-r--r--   1 root root     612 Nov  9 14:54 hosts
    -rw-r--r--   1 root root     411 Jan 27  2015 hosts.allow
    drwxr-xr-x   2 root root   12288 Nov 22 15:13 init/
    
- [x] 文件类型


    - 普通文件
    d 目录文件
    b 块特殊文件
    c 字符特殊文件
    l 符号链接
    f 命名管道
    s 套接字文件
    
- [x]     文件权限


    普通文件权限
    r  4  read
    w  2  write
    x  1  excute
    文件属主的权限
    文件属组的权限
    其他用户的权限
    创建新文件有默认权限，根据umask值计算，属主和属组根据当前进程的用户来设定

    目录权限
    x  进入目录
    rx 显示目录内的文件名
    wx 修改目录内的文件名
    
- [x]     修改权限


    chmod 修改文件、目录权限
        chmod u+x /temp/logs  // u(user) g(group) o(other) a(all)  + - = 
        chmod 755 /temp/logs
    chown 更改属主、属组
        chown -R ubuntu:ubuntu /home/ubuntu
    chgrp 更改属组
    
- [x]   特殊权限


    SUID 用于二进制可执行文件 执行命令时去的文件属主权限 /usr/bin/passwd
    -rwsr-xr-x 1 root root 59640 Mar 23  2019 /usr/bin/passwd

    SGID 用于目录 在该目录下创建新的文件和目录 权限自动更改为该目录的属组

    SBIT 用于目录 该目录下新建的文件和目录 仅root和自己可以删除
    /tmp
    drwxrwxrwt  18 root root 12288 Feb 15 16:50 tmp
    
[Linux 用户和用户组管理](https://www.cnblogs.com/cisum/p/8005641.html)