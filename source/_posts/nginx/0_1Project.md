> Nginx Rewrite规则

##### 当访问的文件和目录不存在时，重定向到 某个文件

    -f和!-f用来判断是否存在文件
    -d和!-d用来判断是否存在目录
    -e和!-e用来判断是否存在文件或目录
    -x和!-x用来判断文件是否可执行

    if(!-e $request_filename){
        rewrite ^/(.*)$ index.php last;
    }
    
##### 如果客户端使用的是ie浏览器，重定向到/ie目录下

    last：相当于Apache里德(L)标记，表示完成rewrite，浏览器地址栏URL地址不变
    break：本条规则匹配完成后，终止匹配，不再匹配后面的规则，浏览器地址栏URL地址不变
    redirect：返回302临时重定向，浏览器地址会显示跳转后的URL地址
    permanent：返回301永久重定向，浏览器地址栏会显示跳转后的URL地址

    last 一般写在server和if中，而break 一般使用在location中
    last 不终止重写后的url匹配，即新的url会再从server走一遍匹配流程，而break终止重写后的匹配
    break和last都能组织继续执行后面的rewrite指令

    if($http_user_agent ~ MSIE){
        rewrite ^(.*)$ /ie/$1 break;
    }
    
##### 禁止访问

    location ~^/(cron|templates)/ {
        deny all;
        break;
    }

    location ~^/data {
        deny all;
    }

    location ~.*\.(sh|flv|mp3)$ {
        return 403
    }
    
##### 设置某些类型文件的浏览器缓存时间

    location ~.*\.(gif|jpg|jpeg|png|bmp|swf)$ {
        expires 30d;
    }

    location ~.*\.(js|css) $ {
        expires 1h;
    }