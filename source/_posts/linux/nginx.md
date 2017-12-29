---
title: "nginx 负载均衡 以及 反向代理服务器配置"
date: 2016-08-25 15:30
---

> Nginx 负载均衡(load balance) 配置

        http{
            upstream sampleapp {
                // 可选配置项，如 least_conn，ip_hash
                server 127.0.0.1:3000 weight =10;  // weight 权重
                server 127.0.0.1:3001 weight =15;
                server 127.0.0.1:3002 weight =20;
                server 127.0.0.1:3003 weight =30;
                // ... 监听更多端口
            }
            ....
            server{
                listen 80;
                ...
                location / {
                    proxy_pass http://sampleapp; // 监听 80 端口，然后转发
                }
            }

##### 默认的负载均衡规则是把网络请求依次分配到不同的端口，我们可以用 least_conn 标志把网络请求转发到连接数最少的 Node.js 进程，也可以用 ip_hash 保证同一个 ip 的请求一定由同一个 Node.js 进程处理。

> Nginx 反向代理服务器

            location / {
                #设置主机头和客户端真实地址，以便服务器获取客户端真实IP
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For
                $proxy_add_x_forwarded_for;

                #禁用缓存
                proxy_buffering off;

                #设置反向代理的地址
                proxy_pass http://192.168.1.1;
            }

> Nginx 缓存设置   expires、etag、if-modified-since指令来进行浏览器缓存控制

            expires
            location /img {
                alias /export/img/;
                expires 1d;
            }

            if-modified-since

> Nginx 开启gzip压缩


            gzip on;   // on off 是否开启Gzip
            gzip_min_length 1k; // 不压缩临界值，大于1K的才压缩
            gzip_buffers 4 16k; // 
            #gzip_http_version 1.0; //默认 HTTP/1.1
            gzip_comp_level 2;  // 压缩级别 1-10
            gzip_types text/plain application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;  //压缩的文件类型
            gzip_vary off; //Squid等缓存服务有关
            gzip_disable "MSIE [1-6]\."; //IE6 不进行Gzip


[参考地址](http://www.linuxdiyf.com/linux/10205.html)

[缓存参考文档](http://mp.weixin.qq.com/s?__biz=MzIwODA4NjMwNA==&mid=2652897955&idx=1&sn=de2d8984f6d0f9e061d1da35df84b182&scene=0#wechat_redirect)

[Gzip](http://www.cnblogs.com/mitang/p/4477220.html)