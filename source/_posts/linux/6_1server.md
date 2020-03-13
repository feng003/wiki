> firewall


    包过滤防火墙和应用层防火墙
    centos 6 默认的防火墙是iptables
    centos 7 默认的防火墙是 firewallD (底层使用netfilter)

- [x] iptables 的表和链


    规则表
        filter nat(网络地址转换) mangle raw
    规则链
        INPUT OUTPUT FORWARD(转发)
        PREROUTING(路由前转换) POSTROUTING(路由后转换)

- [x] iptables 的filter表


    LANG=c man iptables 
    iptables -t filter 命令 规则链 规则
    命令：  -L(list 显示)
            -A(已有之后追加) -I(追加到第一条)
            -D(删除规则) -F(清除默认规则) -P(修改默认规则)
            -N(添加自定义规则链) -X(删除自定义规则链) -E
    -s source 来源
    -d destination 目的
    -i in  流入 -i eth0
    -o out 流出
    -p tcp/udp 协议 
    --dport 端口  --dport 80
    iptables -t filter -A INPUT -i eth0 -s 10.0.0.2 -p tcp --dport 80 -j ACCEPT

    sudo iptables -t filter -vnL

    Chain INPUT (policy ACCEPT 1068 packets, 69346 bytes)
     pkts bytes target     prot opt in     out     source               destination         
        0     0 ACCEPT     all  --  *      *       10.0.0.1             0.0.0.0/0           

    Chain FORWARD (policy DROP 0 packets, 0 bytes)
     pkts bytes target     prot opt in     out     source               destination         
        0     0 DOCKER-USER  all  --  *      *       0.0.0.0/0            0.0.0.0/0           
        0     0 DOCKER-ISOLATION-STAGE-1  all  --  *      *       0.0.0.0/0            0.0.0.0/0           
        0     0 ACCEPT     all  --  *      docker0  0.0.0.0/0            0.0.0.0/0            ctstate RELATED,ESTABLISHED
        0     0 DOCKER     all  --  *      docker0  0.0.0.0/0            0.0.0.0/0           
        0     0 ACCEPT     all  --  docker0 !docker0  0.0.0.0/0            0.0.0.0/0           
        0     0 ACCEPT     all  --  docker0 docker0  0.0.0.0/0            0.0.0.0/0           

    Chain OUTPUT (policy ACCEPT 1074 packets, 98219 bytes)
     pkts bytes target     prot opt in     out     source               destination  

    iptables -t filter -A INPUT -s 10.0.0.1 -j ACCEPT //添加
    iptables -t filter -I INPUT -s 10.0.0.1 -j DROP   //追加到第一条
    iptables -P INPUT ACCPET
    
- [x] iptables 的nat表


    iptables -t nat 命令 规则链 规则
         PREROUTING(路由前转换) 目的地址转换
         POSTROUTING(路由后转换) 源地址转换
         
    iptables -t nat -A PREROUTING -i eth0 -d 114.115.10.1 -p tcp --dport 80 -j DNAT --to-destination 10.0.0.1 //目的地址转换
    iptables -t nat -A POSTROUTING -s 10.0.0.0/24 -o eth1 -j SNAT --to-source 11.11.11.12
 
- [x] iptables 配置文件


    /etc/sysconfig/iptables
    Centos6 service iptables save| start | stop | restart
    Cetnso7 yum install iptables-services
    
- [x] firewallD服务



    支持“zone”区域概念
    firewall-cmd
        firewall-cmd --state
        firewall-cmd --list-all
        firewall-cmd --list-interfaces
        firewall-cmd --list-ports
        firewall-cmd --list-services
        firewall-cmd --get-zones
        firewall-cmd --add-service=https
        firewall-cmd --add-port=80/txp --permanent(添加永久生效)
        firewall-cmd --remove-port=80/txp --permanent(移除永久生效)
    systemctl start | stop | reload | enabeled |disabled firewalld.service