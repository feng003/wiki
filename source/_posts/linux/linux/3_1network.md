> 网络状态查看

##### net-tools

- [x]     ifconfig 查看网络


    eth0      Link encap:Ethernet  HWaddr 00:16:3E:1A:CB:CB  
          inet addr:172.19.23.198  Bcast:172.19.31.255  Mask:255.255.240.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:37753619 errors:0 dropped:0 overruns:0 frame:0
          TX packets:30765802 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000 
          RX bytes:7538432525 (7.0 GiB)  TX bytes:28355854347 (26.4 GiB)

    lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:27982178 errors:0 dropped:0 overruns:0 frame:0
          TX packets:27982178 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0 
          RX bytes:21041169903 (19.5 GiB)  TX bytes:21041169903 (19.5 GiB)

    ech0 网络接口 修改网络接口命名：
    /etc/default/grub 增加 biosdevname=0 net.ifnames=0
    grub2-mkconfig -o /boot/grub2/grub.cfg
    reboot

- [x]     route 查看网关


    route -n

- [x]     netstat

##### iproute

- [x]     ip


    ip addr ls = ifconfig
    ip link set dev eth0  = up ifup eth0
    ip addr add 10.0.0.1/24 dev eth1  =  ifconfig eth1 10.0.0.1 netmask 255.255.255.0
    ip route add 10.0.0/24 via 192.168.0.1 = route add -net 10.0.0.0 netmask 255.255.255.0 gw 192.168.0.1
    
- [x]     ss
    

---

> 网络配置

    
- [x]     ip地址配置


    ifconfig <接口> <IP地址> [netmask 子网掩码]
    ifup <接口>
    ifdown <接口>

- [x] 网关配置命令    


    route add default gw <网关ip>
    route add -host <指定ip> gw <网关ip>
    route add -net <指定网段> netmask <子网掩码> gw <网关ip>

---

> 路由命令
> 网络故障排除


- [x]     ping
- [x]     traceroute
- [x]     mtr
- [x]     nslookup nslookup www.baidu.com
- [x]     telnet  telnet www.baidu.com 80
- [x]     tcpdump tcpdump -i any -n port 80
- [x]     netstat netstat -ntpl
- [x]     ss ss -ntpl
    

---

> 网络服务管理


    两种网络配置方式 sysv AND systemd 只能使用一种
    通过   chkconfig -list network 查看

    service network start|stop|restart

    systemctl list-unit-files NetworkManager.service
    systemctl start|stop|restart NetworkManager
    systemctl enable|disabled NetworkManager

> 常用网络配置文件


- [x]   /etc/sysconfig/network-scripts/


    ifcfg-eth0
    DEVICE=eth0
    ONBOOT=yes        // 开机启动
    BOOTPROTO=static  // dhcp
    IPADDR=172.19.23.198
    NETMASK=255.255.240.0

- [x]   /etc/hosts


    hostname ubuntu
    hostnamectl set-hostname ubuntu

---

> software

- [x] yum rpm包和rpm命令 (apt deb)


    vim-common-7.4.10-5.el7.x86-64.rpm
    软件名称 软件版本 系统版本 平台

    rpm -q 查询 -i 安装 -e 卸载
    

- [x] yum仓库


    /etc/yum.repos.d/CentOS-Base.repo
    yum install
    yum remove
    yum list | grouplist
    yum update

- [x] 源代码编译安装


    wget openresty.gz
    tar -zxf openresty.gz
    cd openresty
    ./configure --prefix=/usr/local/openresty
    make -j2
    make install