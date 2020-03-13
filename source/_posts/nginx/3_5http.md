> access 日志

- [x]     access日志格式


    Syntax: log_format name[escape=default|json|none] string ...;
    Default: log_format combined "...";
    Context http

- [x]     默认的combined日志格式


    log_format combined '$remote_addr-$remote_user[$time_local]'
    ' "$request" $status $body_bytes_sent ' '"$http_referer"
    "$http_user_agent"';

- [x]     配置日志文件路径


    Syntax: access_log path [format[buffer=size] [gzip[=level]] [flush=time] [if=condition] ];
            access_log off;
    Default: access_log logs/access.log combined;
    Context: http, server, location, if in location, limit_except

    path 路径可以包含遍历 不打开cache是每记录一条日志都需要打开、关闭日志文件
    if 通过变量值控制请求日志是否记录
    日志缓存
    日志压缩

- [x]     对日志文件名包含变量时的优化


    Syntax: open_log_file_cache max=N[incative=time][min_user=N][valid=time];
                    open_log_file_cache off;
    Default: open_log_file_cache off;
    Context: http, server, location
    max 缓存内的最大文件句柄数，超出后用LRU算法淘汰
    inactive 文件访问完后在这段时间内不会被关闭。默认10s
    min_uses 在inactive时间内使用次数超过min_uses才会继续存在内存中。默认1
    valid 超出valid时间后，将对缓存的日志文件检查是否存在。默认60秒
    off 关闭缓存

---

> HTTP 过滤模块


    copy_filter 复制包体内容
    postpone_filter 处理子请求
    header_filter 构造响应头部
    write_filer 发送响应

    limit_req zone=req_one
    brust=120;
    limit_conn c_zone 1;
    satify any;
    allow 192.168.0.1/32;
    auth_basic_user_file
    accsss pass;

---

> sub模块 替换响应中的字符串


    ngx_http_sub_filter_module 模块 默认未编译进Nginx 通过--with-http_sub_module启用
    sub模块的指令
    Syntax sub_filter string replacement;
    Default - 
    Context http,server, location

    Syntax sub_filter_last_modified on | off;
    Default  sub_filter_last_modified off;
    Context http,server, location

    Syntax sub_filter_once on | onff;
    Default sub_filter_once on;
    Context http,server, location

    Syntax sub_filter_type mime-type ...;
    Default sub_filter_type text/html;
    Context http,server, location

---

> addition 模块 在响应前后添加内容


    ngx_http_addition_filter_module 默认未编译进Nninx 通过--with-http_addition_module启用
    addition模块的指令
    Syntax add_before_body uri;
    Default - 
    Context http, server, location

    Syntax add_after_body uri;
    Default - 
    Context http, server, location

    Syntax addition_types mime-type ...;
    Default addition_types text/html;
    Context http, server, location

---

> nginx 变量


    变量的提供模块与使用模块
    存放变量的哈希表
    Syntax: variables_hash_bucket_size size;
    Default: variables_hash_bucket_size 64;
    Context: http

    Syntax: variables_hash_max_size size;
    Default: variables_hash_max_size 1024;
    Context: http

---

> http框架提供的请求相关的变量


    set $limit_rate 10k;
- [x]     HTTP请求相关的变量


    arg_参数名
    query_string
    args 全部url参数
    is_args
    content_length
    content_type

- [x]     TCP连接相关的变量


    uri
    document_uri
    request_uri
    scheme
    request_method
    request_length
    remote_user

- [x]     Nginx处理请求过程中产生的变量


    request_body_file
    request_body
    request

- [x]     发送HTTP响应时相关的变量


        host 
- [x]     Nginx系统变量


    http_头部名字 返回一个具体请求头部的值
    特殊 http_host/http_user_agent/http_referer/http_via/http_x_forwarded_for/http_cookie

---

> http框架提供的其他变量

- [x]     TCP连接相关的变量


    binary_remote_addr
    connection
    connection_requests
    remote_addr
    remote_port
    proxy_protocol_addr
    proxy_protocol_port
    server_addr
    server_port
    TCP_INFO
    server_protocol
- [x]     Nginx处理请求过程中产生的变量


    request_time
    server_name
    https
    request_completion
    request_id
    request_filename
    document_root
    realppath_root
    limit_rate
- [x]     发送http响应时相关的变量


    body_bytes_sent
    bytes_sent
    status
    sent_trailer_名字
    sent_http_头部名字 响应中某个具体头部的值
        特殊处理 sent_http_content_type/sent_http_content_length/sent_http_location/sent_http_last_modified/sent_http_connection/sent_http_keep_alive/sent_http_transfer_encoding/sent_http_cache_control/sent_http_link
- [x]     Nginx系统变量


    time_local
    time_iso8601
    nginx_version
    pid
    pipe
    hostname
    msec

---

> referer模块 防盗链


    通过referer模块，用invalid_referer变量根据配置判断referer头部是否合法
    referer模块 默认编译进Nginx 通过--without-http_referer_module禁用

    Syntax valid_referers none | blocked |server_name | string ...;
    Default - 
    Context server, location

    Syntax referer_hash_bucket_size size;
    Default referer_hash_bucket_size 64;
    Context server, location

    Syntax referer_hash_max_size size;
    Default referer_hash_max_size 2048;
    Context server, location

    valid_refereres 可同时携带多个参数 表示多个referer头部都生效
    none   允许缺失referer头部的请求访问
    block  允许referer头部没有对应的值的请求访问
    server_name 若referer 中站点域名与server_name 中本机域名某个匹配，则允许该请求访问

    invalid_referer 1 不允许访问
    if($invalid_referer){
        return 403;
    }

---

> sceure_link模块 使用变量实现防盗链


    通过验证URL中哈希值的方式防盗链
    secure_link
    secure_link_expires

    ngx_http_secure_linke_module 默认未编译进Nginx 通过--with-http_secure_link_module 添加

    Syntax secure_link expression;
    Default - 
    Context http, server, location

    Syntax secure_link_md5 expression;
    Default - 
    Context http, server, location

    Syntax secure_link_secret word;
    Default - 
    Context location

    secure_link $arg_md5,$arg_express;
    secure_linke_md5 "$secure_link_expires$uri$remote_addr secret";

    /prefix/hash/link 
    hash生成方式 对link密钥 做md5哈希求值
    secure_link_secret secret;

---

> map 模块 生成新的变量


> split_client模块 实现AB测试


> geo 模块 根据客户端地址创建新变量


> geoip模块 使用变量获取用户的地理位置