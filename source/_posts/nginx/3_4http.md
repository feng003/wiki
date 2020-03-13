> access阶段

- [x] 对ip做限制的access模块


    ngx_http_access_module 模块
    生效阶段 NGX_HTTP_ACCESS_PHASE
    模块 http_access_module
    默认编译进nginx 通过 --without-http_access_module禁用功能
    生效范围
        进入access阶段前不生效
        
    Syntax allow address |CIDR |unix: |all;
    Default -
    Context http,server,location,limit_except;

    Syntax deny address |CIDR |unix: |all;
    Default -
    Context http,server,location,limit_except;

    location / {
        deny 192.168.1.1;
        allow 192.13.1.1/24;
    }

- [x] 对用户名密码做限制的auth_basic模块


    auth_basic 模块的指令
    基于HTTP Basic Authentication 协议进行用户名密码的认证
    默认编译进Nginx 通过--without_http_auth_basic_module禁用功能

    Syntax auth_basic string | off;
    Default auth_basic off;
    Context http, server, location, limit_except;

    Syntax auth_basic_user_file file;
    Default -
    Context http, server, location, limit_except;

- [x] 使用第三方做权限控制的auth_request模块


    统一的用户权限验证系统 auth_request 模块
    功能：向上游的服务转发请求，若上游服务返回的响应码是2xx，则继续执行，若上游服务返回的是401或403 则将响应返回给客户端
    原理：收到请求后，生成子请求，通过反向代理技术把请求传递给上游服务
    默认末编译进nginx --with-http_auth_request_module

    Syntax auth_request uri |off;
    Default auth_request off;
    Context http, server, location

    Syntax auth_request_set $variable value;
    Default -
    Context http, server, location
    
- [x] satisfy指令


    Syntax satisfy all | any;
    Default satisfy all;
    Context http, server, location
    access 阶段的模块 access模块 auth_basic模块 auth_request 模块

---

> precontent阶段


- [x] try_files 按序访问资源


    ngx_http_try_files_module模块
    依次试图访问多个url对应的文件(由root或alias指令指定)，当文件存在时直接返回文件内容，如果文件不存在，则按最后一个URL结果或者code返回

    Syntax try_files file ... uri;
           try_files file ... =code;
    Default -
    Context server location

- [x] mirror 实时拷贝流量


    ngx_http_miror_module模块
    默认编译进Nginx 通过--without-http_mirror_module移除模块
    处理请求时，生成子请求访问其他服务，对子请求的返回值不作处理

    Syntax mirror uri | off;
    Default mirror off;
    Context http, server, location

    Syntax mirror_request_body on | off;
    Default mirror_request_body on ;
    Context http, server, location

---

> content阶段

- [x] root 和 alias指令


    将url映射为文件路径，以返回静态文件内容
    root 会将完整url映射进文件路径中
    alias 只会讲location后的URL映射到文件路径

    Syntax alias path;
    Default -
    Context location

    Syntax root path;
    Default root html;
    Context http, server, location, if in location
- [x] static模块提供三个变量


    request_filename 待访问文件的完整路径
    document_root    由URI和root/alias规则生成的文件夹路径
    realpath_root    将document_root z中的软连接等换成真实路径
    location /RealPath/ {
        alias html/realpath;
        return 200 '$request_filename:$document_root:$realpath_root\n';
    }
- [x] 静态文件返回时的content-type


    Syntax type {};
    Default types {text/html html; image/gif gif; image/jpeg jpg};
    Context http, server, location

    Syntax default_type mine-type;
    Default default_type text/plain;
    Context http, server, location

    Syntax types_hash_bucket_size size;
    Default types_hash_bucket_size 64;
    Context http, server, location

    Syntax types_hash_max_size size;
    Default types_hash_max_size 1024;
    Context http, server, location

- [x] 未找到文件时的错误日志


    Syntax log_not_found on | off;
    Default log_not_fount on;
    Context http, server, location

- [x] 重定向跳转的域名


    Syntax server_name_in_redirect on | off;
    Default server_name_in_redirect off;
    Context http, server, location

    Syntax port_in_redirect on | off;
    Default port_in_redirect on;
    Context http, server, location

    Syntax absolute_redirect on | off;
    Default absolute_redirect on;
    Context http, server, location

    server {
        server_name a.com;
        server_name_in_redirect on;
        listen 8080;
        port_in_redirect on;
        #absolute_redirect off;
        root html/
    }
- [x] index 和 autoindex模块


    index模块 对访问/时的处理 指定/访问时返回index文件内容
    ngx_http_index_module

    Syntax index file;
    Default index index.html;
    Context http, server, location

    autoindex模块 显示目录内容
    当URL以/结尾时，尝试以html/xml/json/jsonp等格式返回root/alias中指向目录的目录结构
    ngx_http_index_module
    默认编译进nginx --without-http_autoindex_module取消

    Syntax autoindex on | off;
    Default autoindex off;
    Context http, server, location

    Syntax autoindex_exact_size on | off;
    Default autoindex_exact_size on;
    Context http, server, location

    Syntax autoindex_format  html | xml| json| jsonp;
    Default autoindex_format html;
    Context http, server, location

    Syntax autoindex_localtime on | off;
    Default autoindex_localtime off;
    Context http, server, location