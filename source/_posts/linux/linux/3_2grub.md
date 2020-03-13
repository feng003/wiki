> 升级内核

##### rmp格式内核

    unanme -r

    yum install kernel-3.10.0

    yum update

    lscpu
    

##### grub

- [x] grub 配置文件


    /etc/default/grub
    /etc/grub.d/
    /boot/grub2/grub.cfg
    grub2-mkconfig -o /boot/grub2/grub.cfg
    

---

> 进程管理

##### 进程的概念和进程查看

- [x]     C程序的启动是从main函数开始


    int main(int agrc, char *argv[])
    正常终止分为从main返回、调用exit等
    异常终止分为调用abort、接收信号等
        
- [x]     ps


    ps 
    PID   TTY      TIME     CMD
    1848  pts/2    00:00:00 ps
    15988 pts/2    00:00:00 bash

    ps -ef | more
    UID        PID  PPID  C STIME TTY          TIME CMD
    root         1     0  0  2019 ?        00:00:00 /sbin/init
    root         2     0  0  2019 ?        00:00:00 [kthreadd]


- [x]     pstree


- [x]     top


    top -p port
    top
    top - 16:20:54 up 63 days,  1:54,  4 users,  load average: 0.00, 0.00, 0.00
    Tasks: 132 total,   2 running, 130 sleeping,   0 stopped,   0 zombie
    Cpu(s):  0.7%us,  0.3%sy,  0.0%ni, 98.8%id,  0.2%wa,  0.0%hi,  0.0%si,  0.0%st
    Mem:   3924416k total,  3788548k used,   135868k free,   315732k buffers
    Swap:        0k total,        0k used,        0k free,  1583700k cached

    PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND                              
    1 root      20   0 19232  356   84 S  0.0  0.0   0:00.81 init   
    2 root      20   0     0    0    0 S  0.0  0.0   0:00.00 kthreadd

[Linux top命令的用法详细详解](https://www.cnblogs.com/zhoug2020/p/6336453.html)

##### 进程的控制命令


    调整优先级
    nice -n 10 ./a.sh
    renice -n 15 pid

    进程的作业控制
    jobs   fg 1
    & 符号

##### 进程的通信方式 - 信号


    kill -l
    signit Ctrl+c
    sigkill kill -9 pid

##### 守护进程 和 系统日志

- [x]     nohup 与 & 使进程忽略hangup信号


    ps -ef | grep sshd
    root       723     1  0  2019 ?        00:04:33 /usr/sbin/sshd -D
    cd /proc/723
    sudo ls -l cwd
    sudo ls -l fd
    
- [x]     screen -ls 查看 -r 恢复会话


    tail -f dmesg
    tail -f secure
    tail -f cron

##### 服务管理工具 systemctl


    chkconfig --list
    httpd          	0:off	1:off	2:on	3:on	4:off	5:on	6:off
    ip6tables      	0:off	1:off	2:on	3:on	4:on	5:on	6:off
    0 终端 3 关机 6 重启

- [x]     service


- [x]     systemctl


    systemctl status/start/stop/restart/reload/enable/disable 服务名称
    /usr/lib/systemd/system/

##### selinux MAC 强制访问控制 DAC 自主访问控制


    vim /etc/selinux/config
    getenforce
    /usr/sbin/sestatus
    ps -Z And ls -Z And id -Z

    setenforce 0 临时
    /etc/selinux/sysconfig
    
