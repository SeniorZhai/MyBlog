title: Docker入门
date: 2017-02-20 16:43:01
categories: Docker
tags: [入门]
---

<!--more-->
#镜像
##查看镜像信息
`docker images`命令可以列出本地主机上已有的镜像
- PEPOSITORY 仓库
- TAG 标签信息
- ID 唯一ID
- CREATED 创建时间
- VIRTUAL SIZE镜像大下
`docker inspect`命令可以获取该奖项的详细信息
返回一个JSON格式的消息，如果只要其中一项内容时，可以使用`-f`指定参数。
例如镜像的`Architecture`信息
```
docker inspect -f {{".Architecture"}} ID
```
##搜寻镜像
使用`docker search`命令可以搜索远端仓库共享的镜像，默认搜索Docker Hub
##删除镜像
使用`docker rmi`命令可以删除镜像，命令后可以使标签或ID
##查看所有容器
`docker ps -a`命令可以看到本机上存在的所有容器
##创建镜像
###基于已有的镜像的容器创建
docker commit 命令 主要选项包括
- `-a` `--author=""` 作者信息
- `-m` `--message=""` 提交信息
- `-p` `--pause==true` 提交时暂停容器运行
```
docker commit -m "message" -a "author" id name
```
###基于本地模板导入
从一个操作西永模板文件导入一个镜像，推介使用[OPENVZ](https://openvz.org/Download/template/precreated)下载模板
```
cat ubuntu-14.04-x86_64-minimal.tar.gz | docler import - ubuntu:14.04
```
###存出和载入镜像
####存入镜像
使用`docker save`命令
```
docker save -o ubuntu_14.tar unbuntu:14.04
```
###载入镜像
```
docker load --input unbuntu_14.tar
```
###上传镜像
doker push命令可以上传镜像到仓库，默认上传到DockerHub上
```
docker tag test:latest user/test:latest
docker push user/test:latest
```

#容器
容器是镜像的一个运行实例，带有额外可写的文件层
##创建容器
`docker create`新建一个停止状态的容器，可以使用`docker start`命令启动它
##新建并启动容器
`docker run`

##使用特权
--privileged

##使用Dokerfile创建镜像
Docker可以使用文本文件Dockerfile来描述镜像从而创建镜像
###基本结构
Dockerfile由命令语句组成，支持`#`开头的注释
一般而言，Dockerfile分为四部分：基础信息、维护者信息、镜像操作指令和容器启动时执行指令
```Dockerfile
# 第一行必须是指定基础镜像
FROM ubuntu
# 维护者信息
MAINTAINER user user@email.com
# 镜像指令
RUN echo "test message" >> /etc/test.txt
RUN apt-get update && apt-get install -y nginx
# 容器启动时执行指令
CMD /usr/sbin/nginx
```
##指令
###1.FROM
FROM <image>:<tag>
指定基础镜像
###2.MAINTAINER
MAINTAINER <name> 指定维护者信息
###3.RUN
RUN <command>

> RUN echo "root:Docker!" | chpasswd
