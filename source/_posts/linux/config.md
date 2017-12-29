---
title: "ubuntu config"
date: 2016-08-09 09:30
draft: true
---

> 查看ubuntu 版本号

    uname -a   // Linux 4e4af05b5976 3.13.0-63-generic #103-Ubuntu SMP Fri Aug 14 21:42:59 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux

    uname -r   // 3.13.0-63-generic

    cat /etc/issue      // Ubuntu 16.04.1 LTS

    cat /proc/version   // Linux version 3.13.0-63-generic (buildd@lgw01-18) (gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1) ) #103-Ubuntu SMP Fri Aug 14 21:42:59 UTC 2015

    lsb_release -a

> install atom

	$ sudo apt-add-repository ppa:webupd8team/atom
	$ sudo apt-get update
	$ sudo apt-get install atom
  ## 查看 && 安装中文字体
  $ fc-list :lang=zh
  $ sudo apt-get install ttf-wqy-microhei

> install java

  $ sudo apt-add-repository ppa:webupd8team/java
  $ sudo apt-get update
  $ sudo apt-get install oracle-java8-installer
  $ java -version
  $ sudo apt-get install oracle-java7-installer
  $ java -version
  $ sudo update-alternatives --config java

> install phpstorm

  1. java -version
  2. 官网下载linux版本
  3. 解压文件到 /usr/lib/ 目录下 sudo tar -zxvf PhpStorm-2016.2.tar.gz -C /usr/lib/
  4. 重命名文件夹 sudo mv /usr/lib/PhpStorm-162.1121.38/ /usr/lib/phpstorm
  5. 进入到 /usr/lib/phpstrom/bin 目录  ./phpstrom.sh
  6. 激活网站： http://idea.lanyus.com/

[install phpstorm](http://www.linuxdiyf.com/linux/16895.html)

[install url](http://my.oschina.net/keywindy/blog/224104#OSC_h5_10)
