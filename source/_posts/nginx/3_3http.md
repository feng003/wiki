> HTTP请求处理时的11个阶段


![HTTP请求处理阶段](D54B11CA07F542CAA205ACA02EB0ACFA)


![11个阶段顺序](1884F9F34316461B9853C39C3D054BB1)


> postread阶段  获取真实客户端地址的realip模块

- [x] 如何拿到真实的用户IP地址


    TCP连接四元组: scr ip, src port, dst ip, dst port
    HTTP 头部 X-Forwarded-For 用于传递IP
    HTTP 头部 X-Real-IP       用于传递用户IP
    网络中存在很多反向代理

    用户 内网IP 192.168.0.x
    ADSL 运营商公网IP 115.204.33.1
    CDN  IP地址 1.1.1.1
    某反向代理 IP地址 2.2.2.2
    Nginx      用户地址 115.204.33.1 remote_addr 变量 2.2.2.2

- [x] 拿到真实用户IP后如何使用


    基于变量 binary_remote_addr remote_adddr 这样的变量，其值就为真实的IP，这样做连接限制(limit_conn 模块)才有意义
    
- [x]     realip模块


    修改客户端地址
    默认不会编译进nginx 通过 --with-http_realip_module启用功能
    变量 realip_remote_addr realip_remote_port
    指令 set_real_ip_from  real_ip_header real_ip_recursive

    Syntax set_real_ip_from address |CIDR | unix:;
    Default -
    Context http,server,location

    Syntax real_ip_header field | X-Real-IP | X-Forwarded-For| proxy_protocol;
    Default real_ip_header X-Real-IP;
    Context http,server,location

    Syntax real_ip_recursive on | off;
    Default real_ip_recursive off;
    Context http,server,location
    

---

> rewrite 阶段的 rewrite模块 

- [x] return指令


    Syntax return code[text] | code URL | URL ;
    Default -
    Context server,location,if

    返回状态码
    nginx自定义  444 关闭连接
    HTTP1.0标准 301永久重定向 302临时重定向 禁止被缓存
    HTTP1.1标准 303 临时重定向 允许改变方法 禁止被缓存
                307 临时重定向 不允许改变方法 禁止被缓存
                308 永久重定向 不允许改变方法

    Syntax error_page code [=[response] uri;
    Default -
    Context http,server,location,if in location

    error_page 404 /404.html;
    error_page 500 502 503 /50x.html;
    error_page 404 =/404.php;
    location / {
        error_page 404=@fallback;
    }
    location @fallback{
        proxy_pass http://backend;
    }
    error_page 403 http://e.com/forbidden.html;

- [x] 重写URL rewrite指令


    Syntax rewrite regex replacement [flag];
    Default -
    Context server,location,if

1.     将regex 指定的url替换成 replacement 这个新的url
2.     当replacement 以http://或者https://或者$schema开头,则直接返回302重定向
3.     替换后的url根据flag指定的方式进行处理

        --last    用replacement 这个url进行新的loaction匹配
        --break   break 指令停止当前脚本指令的执行
        --redirect   返回302重定向
        --permanent  返回301重定向

    server{
        root html/;
        location /first {
            rewrite /first(.*)/second$1 last;
            return 200 "first";
        }
        location /second {
            rewrite /second(.*)/third$1 break;
            return 200 "first";
        }
        location /third {
            return 300 "third";
        }
        location /redirect {
            rewrite /redirct(.*) $1 permanent;
        }
    }
        
- [x] 条件判断 if指令


    Syntax if (condition){...}
    Default - 
    Context server,location

![if指令](A41AE0B5753F4F8DAC5191CFDE4384D9)

    if($http_user_agent~MSIE){
        rewrite ^(.*)$/msie/$1 break;
    }
    if($http_cookie!*"id=([^;]+)(?:;|$)"){
        set $id $1;
    }
    if($request_method=POST) {
        return 405;
    }
    if($slow){
        limit_rate 10k;
    }
    if($invalid_referer) {
        return 403
    }

---

> find_config阶段 location指令块


    Syntax location [=|~|~*|^~]uri{...}
           location @name{...}
    Default -
    Context server,location

    Syntax merge_slashes on | off;
    Default merge_slashes on;
    Context http,server

- [x] location 匹配规则 仅匹配URI 忽略参数


    前缀字符串
        常规
        = 精确匹配
        ^~ 匹配上后则不再进行正则表达式匹配
    正则表达式
        ~  大小写敏感的正则匹配
        ~* 忽略大小写的正则匹配
    合并连续的 / 符号
        merge_slashes on
    用于内部跳转的命名location
        @

    location ~/Test1/$ {}
    location 


![location](69CAC3363DCD48EF9904BC1363955824)

---

> preaccess阶段 

- [x] 如何限制每个客户端的并发连接数


    ngx_http_limit_conn_module模块
    生效阶段 NGX_HTTP_PREACCESS_PHASE阶段
    模块 ngx_http_limit_conn_module
    默认编译进nginx 通过--without-http_limit_conn_module禁用
    生效范围
        全部worker进程(基于共享内存)
        进入preaccess阶段前不生效
        限制的有效性取决于key的设计：依赖postread阶段的realip模块取到的真实ip

- [x] limit_conn 指令


    定义共享内存(包括大小)以及key关键字
    Syntax limit_conn_zone key zone=name:size;
    Default -
    Context http

    限制并发连接数
    Syntax limit_conn zone number;
    Default -
    Context http,server,location

    限制发生时的日志级别
    Syntax limit_conn_log_level info|notice|warn|error;
    Default limit_conn_log_level error;
    Context http, server, location

    限制发生时向客户端返回的错误码
    Syntax limit_conn_status code;
    Default limit_conn_status 503;
    Context http,server,location

- [x] 如何限制每个客户端的每秒处理请求数


    ngx_http_limit_req_module模块
    生效阶段 NGX_HTTP_PREACCESS_PHASE阶段
    模块 ngx_http_limit_req_module
    默认编译进nginx 通过--without-http_limit_req_module禁用
    生效算法 leaky bucket算法
    生效范围
        全部worker进程(基于共享内存)
        进入preaccess阶段前不生效

- [x] limit_req 指令


    定义共享内存(包括大小) 以及key关键字和限制速率
    Syntax limit_req_zone key zone=name:size rate=rate;
    Default -
    Context http (rate 单位 r/s r/m)

    限制并发连接数
    Syntax limit_req zone=name [burst=number] [nodelay];
    Default -
    Context http,server,location
    burst 默认为0； nodelay 对burst 中的请求不在采用延时处理的做法，而是立刻处理

    限制发生时的日志级别
    Syntax limit_req_log_level info|notice|warn|error;
    Default limit_req_log_level error;
    Context http, server, location

    限制发生时向客户端返回的错误码
    Syntax limit_req_status code;
    Default limit_req_status 503;
    Context http,server,location

    conf
    limit_conn_zone $binary_remote_addr zone=addr:10m;
    limit_req_zone $binary_remote_addr zone=one:10m rate=2r/m; (每分钟两条)
    server{
        server_name limit.com;
        root html/;
        error_log logs/error.log info;
        location / {
            limit_conn_status 500;
            limit_conn_log_level warn;
            limit_rate 50;
            limit_conn addr 1;
            limit_req zone=one burst=3 nodelay;
            # limit_req zone=one;
        }
    }