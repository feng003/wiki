### 初识nginx 

> 三个应用场景


    静态资源服务 通过本地文件系统提供服务
    反向代理服务 nginx的强大性能 缓存 负载均衡
    API服务 OpenResty

> 优点


    高并发 高性能
    可扩展性好
    高可靠性
    热部署
    BSD许可证

> 组成


    nginx二进制可执行文件 有各模块源码编译出的一个文件
    nginx.conf配置文件 控制nginx行为
    access.log访问日志 记录每一条http请求信息
    error.log错误日志 定位问题

> 版本发布 feature bugfix change


    nginx.org
    nginx.com
    Tengine
    OpenResty

> install (08) 

> nginx语法(09)


    http
    server
    location
    upstream

> 重载 热部署 日志切割


    nginx -s reload
    帮助 -? -h
    使用制定的配置文件 -c 
    指定配置指令 -g
    指定运行目录 -p 
    发送信号 -s (stop quit reload reopen)
    测试配置文件是否有语法错误 -t -T 
    打印nginx的版本 编译信息 -v -V

    热部署
    替换二进制文件 objs/nginx 
    kill -USR2 13195(master 进程端口号)
    kill -WUNCH 13195 老的worker进程退出

    日志切割 shell
    #!/bin/bash
    #rotate the nginx log
    LOGS_PAHT=/usr/local/nginx/logs/history
    CUR_LOGS_PAHT=/usr/local/nginx/logs
    YESTERDAT=$(date -d "yesterday" + %Y-%m-%d)
    mv ${CUR_LOGS_PAHT}/access.log ${LOGS_PAHT}/access_${YESTERDAT}.log
    mv ${CUR_LOGS_PAHT}/error.log ${LOGS_PAHT}/error_${YESTERDAT}.log
    kill -USR1 $( cat /usr/local/nginx/logs/nginx.pid)

##### nginx: [error] invalid PID number "" in "/usr/local/nginx/logs/nginx.pid"的解决办法

    /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf

##### nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)

    sudo netstat -ntlp
    sudo kill -9 20400 (80端口号对应的)
    sudo netstat -ntlp
    sudo /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf

> 静态资源服务器


    gzip on;
    gzip_min_length 1;
    gizp_comp_level 2;
    gize_types text/plain text/html;
    server{
        listen 8080;
        server_name static.com;
        location / {
            alias dlib/;
            autoindex on;
            set $limit_rate 1k;
            index index.html index.php;
        }
    }

> 反向代理


    server 127.0.0.1:8080;
    server {
        listen 127.0.0.1:8080;
        root html/2/
    }
    server 127.0.0.1:8081;
    server {
        listen 127.0.0.1:8081;
        root html/1/
    }

    upstream  local {
        server 127.0.0.1:8080;
        server 127.0.0.1:8081;
    }
    server{
        server_name a.com;
        listen 80;
        location / {
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

            proxy_cache my_cache;
            proxy_cache_key $host$uri$is_args$args;
            proxy_cache_valid 200 304 302 ld;

            proxy_pass http://local;
        }
    }
    http  {
        //缓存设置
        proxy_cache_path /tmp/nginxcache levels=1:2 keys_zone=my_cache:10m max_size=10g inactive=60m use_temp_path=off;
    }

> access 日志 Goaccess

> TLS/SSL


    TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
    密钥交换算法
    身份验证算法
    对称加密算法、强度、分组模式
    签名hash算法

> 加密 对称与非对称

> SSL证书 CA


    DV domain validated
    OV organization validated
    EV extended validated

> SSL协议


    验证身份
    达成安全套件共识
    传递密钥
    加密通讯