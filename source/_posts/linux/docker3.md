---
title: "docker(3): Dockderfile 语法详细介绍"
date: 2016-08-13 15:10
---

> Dockerfile 语法示例

###### 注释 和 命令+参数

    # Line blocks used for commenting
    command argument argument ..

> 命令

###### FROM 定义了使用哪个基础镜像启动构建流程，FROM命令必须是Dockerfile的首个命令。

    # Usage: FROM  <image>   or   FROM <image>:<tag>
    FROM  ubuntu

###### MAINTAINER 声明作者，并应该放在FROM的后面

    # Usage: MAINTAINER [name]
    MAINTAINER authors_name

###### RUN 接受命令作为参数并用于创建镜像,RUN命令用于创建镜像（在之前commit的层之上形成新的层）

    # Usage: RUN [command]
    RUN aptitude install -y riak

###### ADD  两个参数 源 和 目标，基本作用是从源系统的文件系统上复制文件到目标容器的文件系统

    # Usage: ADD [source directory or URL] [destination directory]
    ADD /my_app_folder /my_app_folder

###### CMD 用于执行特定的命令，这些命令不是在镜像构建的过程中执行，而是在用镜像构建容器后被调用

    # Usage 1: CMD application "argument", "argument", ..
    CMD "echo" "Hello docker!"

###### ENV  设置环境变量。这些变量以”key=value”的形式存在，并可以在容器内被脚本或者程序调用

    # Usage: ENV key value
    ENV SERVER_WORKS 4

###### EXPOSE 指定端口，使容器内的应用可以通过端口和外界交互

    # Usage: EXPOSE [port]
    EXPOSE 8080

###### USER 设置运行容器的UID

    # Usage: USER [UID]
    USER 751

###### VOLUME 让你的容器访问宿主机上的目录

    # Usage: VOLUME ["/dir_1", "/dir_2" ..]
    VOLUME ["/my_files"]

###### WORKDIR 设置CMD指明的命令的运行目录

    # Usage: WORKDIR /path
    WORKDIR ~/

###### ENTRYPOINT

[参考文档](http://www.alauda.cn/2015/07/17/dockerfileinstructions/)
