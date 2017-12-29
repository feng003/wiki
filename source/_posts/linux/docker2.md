---
title: "docker(2): build own image "
date: 2016-08-13 10:10
---

> Write Dockerfile

##### Write a Dockerfile

    mkdir dockerbuild && cd dockerbuild
    sudo vim Dockerfile

    FROM docker/whalesay:latest
    RUN apt-get -y update && apt-get install -y fortunes
    CMD /usr/games/fortune -a | cowsay

##### Build an image from your Dockerfile

    docker build -t feng003/test . (注意最后有个点 .)
    docker run feng003/test

> Delete images

1. 停止所有的  container

    docker ps -a -q        // 994ecc0d5205
    sudo docker stop 994ecc0d5205

2. 删除 images，通过image 的id来确定删除

    docker images
    sudo docker rmi <image id>


[参考地址](https://docs.docker.com/engine/getstarted/step_four/#step-1-write-a-dockerfile)
