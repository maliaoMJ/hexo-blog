---
title: centOS中利用Docker安装gitlab
catalog: true
tags:
  - Docker
  - git
  - gitlab
url: 111.html
id: 111
categories:
  - Docker
date: 2019-02-21 11:37:05
subtitle:
header-img:
---

![](http://images0.cnblogs.com/news/66372/201507/052157452643027.png)

#### 首先安装虚拟机或者直接在你的云主机上操作

#### 修改centos 默认配置 查看IP

> centos 配置最好高于：4G 2核，否则运行不起来

#### 查看centos的IP地址

     ip addr 
    

#### 无法查询IP的解决办法

> 进入`ect/sysconfig/network-scripts`文件夹,修改对应的网卡配置文件，如图所示

![](http://116.85.35.63/wp-content/uploads/2019/02/17.png)

![](http://116.85.35.63/wp-content/uploads/2019/02/18.png)

      service network start
    

> 然后查看IP地址

![](http://116.85.35.63/wp-content/uploads/2019/02/19.png)

#### 安装docker 并启动

> 1.  `yum -y install docker`
> 2.  `systemctl start docker`
> 3.  `docker login #输入你的用户名和密码`
> 4.  `docker pull gitlab/gitlab-ce:latest #可能需要一些时间`
> 
> 最后创建并启动容器

    docker run -d \
    --hostname 192.168.50.27 \
    -p 443:443 \
    -p 80:80 \
    -p 9090:9090 \
    --name gitlab \
    --restart always \
    -v /home/gitlab/config:/etc/gitlab \
    -v /home/gitlab/log:/var/log/gitlab \
    -v /home/gitlab/opt:/var/opt/gitlab \
    --privileged=true gitlab/gitlab-ce:latest
    

#### 需要等一会，打开游览器输入虚拟机或者云服务器的公网IP

> 如图 1\. 重置密码

![](http://116.85.35.63/wp-content/uploads/2019/02/20.png)

> 2.  登录和注册

![](http://116.85.35.63/wp-content/uploads/2019/02/21.png)

> 3.  登录后主界面

![](http://116.85.35.63/wp-content/uploads/2019/02/22.png)

#### 配置gitlab

> 查看容器id `shell docekr ps -a` 进入容器命令行

      docker exec -it containerID  /bin/bash // containerID 代表该gitlab的ID
    

> 参照官网修改gitlab.rb 文件修改gitlab配置(由于容器数据卷挂载地方不同，通常是在/etc/gitlab/ 文件夹下)