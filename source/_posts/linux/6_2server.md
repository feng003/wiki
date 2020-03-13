> SSH 远程管理

- [x]     selinux 强制安全访问控制

- [x]     telnet


    yum install telnet telnet-server xinetd -y
    systemctl start xinetd.service
    systemctl start telnet.socket
    ifconfig eth0
    telnet 106.14.192.228

    查看telnet服务 grep telnet /etc/services
    telnet		23/tcp
    rtelnet		107/tcp				# Remote Telnet
    rtelnet		107/udp
    telnets		992/tcp				# Telnet over SSL
    iptables -I INPUT -p tcp --dport 23 -j ACCEPT

    抓包 tcpdump -i any port 23 -s 1500 -w /tmp/a.dump

- [x] sshd_config /etc/ssh/sshd_config


    Port 22 默认端口
    PermitRootLogin yes 是否准许root登录
    AuthorizedKeysFile .ssh/authorized_keys
    systemctl status | start | stop | restart | enable | disable sshd.service

    ssh [-p port] 用户@远程ip
    ssh -p 22 user@12.0.0.1
    netstat -ntpl | grep 22

    ssh公钥认证
        ssh-keygen -t rsa (客户端生成)
        ssh-copy-id (copy到服务端) // public key .ssh/id_rsa.pub
        ssh-copy-id -i .ssh/id_rsa.pub root@10.0.0.1(远程服务器端)

    scp a.txt root@10.0.0.1:/tmp/ 本地文件copy到远程服务器

> FTP vsftpd

- [x] ftp协议 文件传输协议 port 21


    主动模式 和 被动模式
    
- [x] vsftpd 服务器


    yum install vsftpd ftp
    systemctl start vsftpd.service
    将selinux 改为 permissive
        getsebool -a | grep ftpd
        setsebool -P <sebool> 1
        
- [x] vsftdp 服务配置文件


    /etc/vsftpd/vsftpd.conf
    /etc/vsftpd/ftpusers
    /etc/vsftpd/user_list
    getsebool -a | grep ftpd
    man 5 vsftpd

- [x] vsftp 虚拟用户


    guest_enable=YES
    guest_username=vuser
    user_config_dir=/etc/vsftpd/vuserconfig
    allow_writeable_chroot=YES
    pam_service_name=vsftpd.vuser

    useradd vuser -d /data/ftp -s /sbin/nologin
    grep vuser /etc/passwd
    db_load -T -t hash -f /etc/vsftpd/vuser.temp /etc/vsftpd/vuser.db
    chmod 600 /etc/vsftpd/vuser.db
    vim /etc/pam.d/vsftpd.vuser