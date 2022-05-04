---
title: Docker
top: false
abbrlink: f5f9fa9b
date: 2021-03-01 16:57:19
tags:
categories:
---

Docker 使用

<!-- more -->

## 官方网站和帮助文档

[官方网站](https://www.docker.com)
[帮助文档](https://docs.docker.com)

## Docker为什么会出现？

解决开发上线应用环境配置不一致的问题。

## Docker与虚拟机的区别？

虚拟机：虚拟硬件模拟一个完整的操作系统，占用大量内存与存储容量
Docker：使用容器技术隔离运行在宿主机的内，容器没有自己的内核无虚拟硬件.

## 重要的概念

镜像（image）：镜像是创建容器的基础，本质上类似于系统，容器基于镜像进行搭建。一个镜像可以搭建多个容器。
容器（container）：容器是Docker的一种技术，用于使容器在镜像上创建应用。
仓库（repository）：与GitHub功能相当，存放镜像的地方，分为私用和公有。

## Docker使用

### 1,常用帮助命令

- docker version      #显示版本信息
- docker info         #显示系统信息，包含镜像容器数量
- docker 命令 --help  #查询该命令的帮助信息

### 2,镜像命令

1. docker images #查看本机镜像
    - docker images -a 列出所有镜像
    - docker images -q 列出所有镜像ID

2. docker search #搜索镜像

3. docker pull   #拉去下载镜像
    - docker pull 镜像名:版本号

4. docker rmi    #删除镜像
    - docker rmi -f 镜像ID
    - docker rmi -f 镜像ID 镜像ID...
    - docker rmi if $(docker images -aq) #递归删除所有镜像

### 3,容器常用命令

**3.1,新建容器运行**

1. docker run 镜像ID
    - --name 添加容器名称
    - -d 后台运行
    - -p
        - 主机端口:容器端口 指定容器端口
        - ip 主机端口：容器端口
        - 容器端口

- -P 随机端口
- -it 使用交互方式运行，进入容器查看内容

**3.2,列出所有运行的容器**

docker ps

**3.3,删除容器**

1. docker rm
    - docker rm 镜像ID      #删除不在运行的容器
    - docker rm -f $(docker ps -aq) #删除指定容器
    - docker ps -a -q|xargs docker rm #删除所有容器

**3.4,启动停止重启强制停止**

docker start 镜像ID     #启动容器
docker restart 镜像ID   #重启容器
docker stop 镜像ID      #停止容器
docker kill 镜像ID      #强制停止容器

**3.5,退出容器**

exit        #容器直接退出
ctrl p q    #容器退出不停止

**3.6,查看日志**

docker logs -tf  容器ID       #一直更新显示日志信息
docker logs --tail 数字 容器ID #显示需要的日志条数
docker logs -ft 容器ID  #跟着日志

**3.7,查看容器源数组**

docker inspect 容器ID

**3.8,进入运行的容器**

docker exec -it 容器ID bashshell #进入容器开启一个新终端
docker attach 容器ID #进入容器正在执行的终端

**3.9,拷贝容器数据到主机**

docker 容器ID：容器内路径 主机目的路径

### 4,容器数据卷

**4.1,什么是容器数据卷解决了什么问题？**

容器是将应用和数据打包在内，如果容器不存在了那么数据也就不存在。为了解决这个问题需要将容器同步到一个指定位置，使用挂载到主机上来实现。

**4.2,数据卷的三种挂载方式**

匿名挂载 docker -v 容器内路径
具名挂载 docker -v 名称:容器内路径
指定路径挂载 docker -v 主机路径:容器内路径

**4.3,读取权限问题**

ro权限，只能通过宿主机进行写入
docker -v 挂载方式:ro
rw权限，可读可写权限
docker -v 挂载方式:rw

### 5,DockerFile

**5.1,DockerFile本质就是构建自己的镜像**

**5.2,构建步骤**

1. 使用构建命令写DockerFile文件
2. docker build镜像
3. docker run镜像
4. 发布镜像
    - hubdocker
    - 阿里云
    - ....

**5.3,常用构建命令**

构建命令都是大写不能写错

FROM - 构建的开始
MAINTAINER - 姓名+邮箱代表谁写的
RUN - 构建时需要运行的命令
ADD - 需要添加的内容
WORDDIR - 工作目录
VOLUME - 挂载目录
EXPOSE - 暴露端口配置
CMD - 启动容器需要运行的命令
ENTORPINT - 启动容器需要运行的命令
COPY - 将文件拷贝到镜像中
ENV - 设置环境变量

**5.4,docker build**

docker build -f 执行文件 -t 名称:版本信息 .

**5.5,docker history**

可以查看自己或者其他镜像的构建过程

**5.6,CMD和ENTRYPOINT区别**

©版权归属作者“HEJIE.XYZ / 何杰”，转载请注明。谢谢~
