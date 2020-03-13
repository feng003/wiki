- [x] 配置块的嵌套


    main
    http{
        upstream{}
        split_clients{}
        map{}
        geo{}
        server{
            if(){}
            location {
                limit_except{}
            }
            location {
                location {
                    
                }
            }
        }
        server{
            
        }
    }
    
- [x] 指令的Context


    Syntax: log_format name [] string;
    Default: log_format combined "...";
    Context: http

    Syntax: access_log path [format [buffer=size][gzip[=level]]] ;
            access_log off;
    Default: access_log logs/access.log combined;
    Context: http,server,location,if in location,limit_except
    
- [x] 指令的合并


    值指令 存储配置项的值 可以合并
        root access_log gzip

    动作类指令 指定行为 不可以合并
        rewrite proxy_pass
        生效阶段 server_rewrite rewrite content

- [x] 存储值的指令继承规则：向上覆盖


    子配置不存在时 直接使用父配置块
    子配置存在时，直接覆盖父配置
    server{
        listen 8080;
        root /home/ubuntu;
        access_log logs/access.log main;
        location /test{
            root /home/ubunt/test;
            access_log logs/access_log.log main;
        }
        location /dlib {
            alias dlib/;
        }
        location / {
            
        }
    }

- [x] http 模块合并配置的实现


    指令在哪个块下生效？
    指令允许出现在哪些块下？
    在server块内生效，从http向server合并指令：
        char *(*merge_srv_conf)();
    配置缓存存在内存
        char *(*merge_loc_conf)();