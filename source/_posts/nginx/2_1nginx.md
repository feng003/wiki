> 使用信号管理nginx 父子进程


    master进程
    监控worker进程 CHLD
    管理worker进程
    接受信号
        TERM INT
        QUIT
        HUP
        USR1
        USR2
        WINCH

    worker进程
        接受信号
            TERM  INT
            QUIT
            USR1
            WINCH

    NGINX命令行
        reload HUP
        reopen USR1
        stop TERM
        quit QUIT 

> reload 重载配置文件流程(24)

> 优雅的关闭worker进程 -- http请求

> 网络收发与nginx事件


    读事件 accept建立连接 read读消息
    写事件 write写消息

> NGINX网络事件

    