---
title: "php5.5 update 5.6"
date: 2017-05-06 11:40
draft: true
---

ubuntu 14.04 lnmp环境php5.5版本升级到php5.6

> 由于 const Administration    = 0x1 << 1;  由于php版本低于5.6导致的语法错误

          sudo apt-get install software-properties-common
          sudo add-apt-repository ppa:ondrej/php5-5.6
          sudo add-apt-repository ppa:ondrej/php
          sudo apt-get install php5.6 php5.6-fpm php5.6-xml 
          sudo apt-get update
          sudo apt-get install php5.6 php5.6-mbstring php5.6-mcrypt php5.6-mysql php5.6-xml
          sudo a2dismod php5

          sudo apt-get install php5.6-fpm
          sudo apt-get remove php5
          sudo apt-get remove php5-fpm
          sudo service nginx restart

          之后修改nginx.conf或者虚拟主机中的配置 

          which php5.6-fpm
          find / -name php5.6-fpm
          fastcgi_pass unix:/run/php/php5.6-fpm.sock; //修改路径


[参考地址](http://blog.csdn.net/jian1jian_/article/details/51296280)
