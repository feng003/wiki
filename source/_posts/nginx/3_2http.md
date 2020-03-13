> Listen 指令


    Syntax: listen address[:port]
            listen unix:path
    Default: Listen *.80 | *.8080;
    Context: server

    listen unix:/var/run/nginx.sock;
    listen 127.0.0.1:8000;
    listen 127.0.0.1;
    listen 80;
    listen *.8080;
    listen localhost:8000 bind;
    listen [::]:8000 ipv6only=on;
    listen [::1];
    

---

> 处理 http 请求头部的流程


![接收请求事件模块](33E6C99A3B7C43F4958CFB9348969C29)


![接收请求http模块](9235DD62AE534362A9D8D6AE9DE3337F)

---

> nginx 正则表达式

- [x] 元字符


    . 匹配除换行符以外的任意字符
    \w 匹配字母或数字或下划线或汉字 [A-Za-z0-9_]
    \s 匹配任何不可见字符 包括空格、制表符、换页符
    \d 匹配数字 [0-9]
    \b 匹配单词的开始或结束
    ^ 匹配字符串的开始
    $ 匹配字符串的结束
- [x] 重复


    * 重复零次或更多次
    + 重复一次或更多次
    ? 重复零次或一次
    {n} 重复n次
    {n,} 重复 n次或更多次
    {n,m} 重复 n到m次

- [x] 其他例子 测试工具  pcretest


    \ 转义符号 取消元字符的特殊含义
    ( ) 分组与取值

    原始URL：/admin/website/article/12/change/uploads/party/5.jpg
    转换后的URL：/static/uploads/party/5.jpg

    匹配原始URL
    /^\/admin\/website\/article\/(\d+)\/uploads\/(\w+)\/(\w+)\.(png|jpg|gif|jpeg|bmp)$ /

    rewrite ^/admin/website/solution/(/d+)/change/uploads/(.*)\.(png|jpg|gif|jpeg|bmp)$
    /static/uploads/$2/$3.$4 last;
    
> 如何找到处理请求的server指令块


- [x]     server_name 指令 指令后可以跟多个域名，第一个是主域名


    Syntax server_name_in_redirect on | off;
    Default server_name_in_redirect off;
    Context http,server,location

    server {
        server_name *.tb.com
        server_name www.tb.com ~^www\d+\.tb.com$ //正则要加 ~ 前缀
        server_name_in_redirect off;
    }
    server {
        server_name ~^(wwww\.)?(.+)$;
        location / { root /sites/$2; }
    }
    server {
        server_name ~^(wwww\.)?(?<domain>.+)$;
        location / {root /sites/$domain;}
    }
    _ 匹配所有

- [x]     Server 匹配顺序


    精确匹配
    *在前的泛域名
    *在后的泛域名
    按文件中的顺序匹配正则表达式域名
    default server 第一个 listen 指定default
    
