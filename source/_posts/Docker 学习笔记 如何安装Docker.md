---
title: Docker 学习笔记 如何安装Docker
catalog: true
tags:
  - Docker
url: 114.html
id: 114
categories:
  - Docker
date: 2019-02-21 11:41:50
subtitle:
header-img:
---

![docker](https://gss3.bdstatic.com/7Po3dSag_xI4khGkpoWK1HF6hhy/baike/w%3D268%3Bg%3D0/sign=05f46e0580d4b31cf03c93bdbfed4042/2cf5e0fe9925bc31137974de55df8db1cb13704b.jpg)

> #### 1.安装环境 CentOS7

      sudo yum install docker
    

> #### 2.启动Docker

       sudo yum systemctl start docker
    

> #### 3.搜索镜像

       docker search ubuntu
    

> #### 4.拉取镜像

    docker pull ubuntu:16.04
    

> #### 5.docker 权限问题
> 
> 运行如下命令

     sudo groupadd docker #创建docker用户组
     sudo gpasswd -a ${USER} docker #将当前用户添加该组内
     sudo service docker restart # 重启docker服务，退出ssh,重新连接即可