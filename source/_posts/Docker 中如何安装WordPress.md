---
title: Docker 中如何安装WordPress
catalog: true
tags:
  - Docker
  - WordPress
url: 39.html
id: 39
categories:
  - Docker
date: 2019-02-18 18:12:44
subtitle:
header-img:
---

### Docker 中如何安装WordPress

![docker](https://gss3.bdstatic.com/7Po3dSag_xI4khGkpoWK1HF6hhy/baike/w%3D268%3Bg%3D0/sign=05f46e0580d4b31cf03c93bdbfed4042/2cf5e0fe9925bc31137974de55df8db1cb13704b.jpg)

> ##### 1\. 从dockerHub 中拉取mysql镜像

       docker pull mysql:5.6
    

当然你也可选用最新的mysql版本

    docker pull mysql
    

![](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=2829320801,1933304096&fm=58&bpow=1024&bpoh=1024)

> ##### 2\. 从dockerHub 中拉取 wordpress镜像

    docker pull wordpress:latest
    

> ##### 3\. 创建mysql容器 并且做一些设置

    docker run --name wordpress-mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.6
    
    

> ##### 4\. 创建WordPress 容器并且做一些基础配置

    docker run --name blog-wordpress --link wordpress-mysql:mysql -p 8080:80 -d wordpress
    

> ##### 5\. 访问localhost:8080 端口进行配置wordpress站点