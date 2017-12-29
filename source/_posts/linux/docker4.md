---
title: "docker(4): build own image node "
date: 2016-08-17 11:10
---
## 创建镜像

> 把容器创建为一个新的镜像

    docker run -it ubuntu:latest sh -c '/bin/bash'
    对容器进行修改
    退出容器，docker ps -a  查找刚刚创建的 容器ID
    docker commit <container id> ubuntu:lnmp
    docker save -o lnmp.tar.gz ubunt:lnmp //将镜像导出
    docker import lnmp.tar.gz //导入镜像

> Dockerizing a Node.js web app

##### 准备替换文件  package.json 和 server.js

    package.json

    {
          "name": "docker_web_app",
          "version": "1.0.0",
          "description": "Node.js on Docker",
          "author": "zhang <411152544@qq.com>",
          "main": "server.js",
          "scripts": {
            "start": "node server.js"
          },
          "dependencies": {
            "express": "^4.13.3"
          }
    }

    server.js

    'use strict';
    const express = require('express');
    // Constants
    const PORT = 8080;
    // App
    const app = express();
    app.get('/', function (req, res) {
      res.send('Hello world\n');
    });
    app.listen(PORT);
    console.log('Running on http://localhost:' + PORT);

> Dockerfile

    FROM node:argon

    # Create app directory
    RUN mkdir -p /usr/src/app
    WORKDIR /usr/src/app

    # Install app dependencies
    COPY package.json /usr/src/app/
    RUN npm install

    # Bundle app source
    COPY . /usr/src/app

    EXPOSE 8080
    CMD [ "npm", "start" ]

> Build

    docker build -t feng003/node .

> Tag and Push

    docker tag <image id> feng003/node:v1
    docker login
    docker push feng003/node:v1

    docker images
    docker tag 7d9495d03763 feng003/ubuntu:14.04
    docker login
    docker push feng003/ubuntu

[官方文档 - push/tag](https://docs.docker.com/engine/getstarted/step_six/)

> Run    docker run -it ubuntu sh -c '/bin/bash'  创建新的容器

    -i  表示是一个交互容器，会把当前标准输入重定向到容器的标准输入中
    -t 为这个容器分配一个终端

###### Docker mapped the 8080 port inside of the container to the port 49160 on your machine.

    docker run -p 49160:8080 -d feng003/node:v1

    docker ps  //get container id
    docker logs <container id>  // print app output
    curl http://localhost:49160

    docker run -it ubuntu:14.04 /bin/bash
    docker run -it -m 300M --memory-swap -1 ubuntu:14.04 /bin/bash
    docker run -it -m 300M ubuntu:14.04 /bin/bash

[官方文档 - run](https://docs.docker.com/engine/reference/run/)

> start docker start -i <container name>   启动容器

    docker ps -a
    docker start -i test

> exec  If you need to go inside the container

    docker exec -it <container id> /bin/bash

> Docker容器运行期间，对文件系统的所有修改都会以增量的方式反映在容器使用的联合文件系统中，并不是真正的对只读层数据信息修改。每次运行容器对它的修改，都可以理解成对夹心饼干又添加了一层奶油，这层奶油仅供当前容器使用。当删除docker 容器，或通过该镜像重新启动时，之前的更改将会丢失。

    docker run -it -v /home/www ubuntu:lastest sh -c '/bin/bash'
    -v 指定奖容器内的某个目录作为数据卷加载

    docker inspect -f {{.Volumes}} <container name>
    找到数据卷在主机上真正的存储位置

    docker run -it -v [host_dir]:[container_dir]
    将本地文件目录挂载到容器内的位置   host_dir 主机的目录   container_dir  容器的目录

> Docker 容器内的系统工作起来就像是一个虚拟机，容器内开放的端口并不会真正开放在主机上。想要奖docker容器的端口开放到主机上，可以使用类似端口映射的方式

    docker run -it -p 34666:22 ubuntu sh -c '/bin/bash'
    -p [主机端口]:[容器端口]

    docker run --name wp --link mysql:mysql -p 8080:80 -d wp
    --link 将两个容器连接起来，实现容器之间的网络通讯

[官网node](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)

[DaoCloud](http://docs.daocloud.io/faq/docker101)
