---
title: Uni-App 基于Vue技术栈方向跨端技术实践
catalog: true
tags:
  - Vue
  - Weex
  - 微信小程序
categories:
  - Vue
date: 2019-04-30 6:37:05
subtitle:
header-img: "https://p4.ssl.cdn.btime.com/t0125a8daac3dbdb9f2.jpg?size=1200x616"
---

### 1.什么是Uni-App?
uni-app 是一个使用 Vue.js 开发跨平台应用的前端框架，开发者编写一套代码，可编译到iOS、Android、H5、小程序等多个平台。一套代码编到7个平台，最全面的跨平台。
如下图：
---
![跨端图](http://img4.imgtn.bdimg.com/it/u=869023636,1150895082&fm=26&gp=0.jpg)
![功能架构图](https://img-cdn-qiniu.dcloud.net.cn/uniapp/doc/uni0124.png)

uni-app实现了一套代码，同时运行到多个平台，一套代码，同时可以运行到iOS模拟器、Android模拟器、H5、微信开发者工具、支付宝小程序Studio、百度开发者工具、字节跳动开发者工具中，如下图所示

![多端图](http://img.cdn.aliyun.dcloud.net.cn/guide/uniapp/run1x7.png)

> PS: 学习一门技术最好的方式，就是看官网： https://uniapp.dcloud.io/

### 2. 语法原理以及注意事项

uni-app 在发布到H5时支持所有vue的语法；发布到App和小程序时，由于平台限制，无法实现全部vue语法，但uni-app仍是是对vue语法支持度最高的跨端框架。
> PS: 过于啰嗦和简单，官网文档在此： https://uniapp.dcloud.io/use

### 3. 技术实践
> 1.先看一下项目的目录结构图


![目录结构图](https://s2.ax1x.com/2019/04/30/EG8srn.png)


1. src 目录 app.vue 文件相当于原生小程序的app.js，负责初始化全局globalData和一些生命周期函数等功能。
2. src 目录 main.js 全局的入口文件，跟用vue-cli 脚手架生成的差不多
3. src 目录 config 这个是我自定义的目录，处理全局的配置，比如请求的URL，毕竟设计多种环境，比如开发，测试，beta和正式上线。
4. src 目录 utils 这个是我自定义的目录,用来封装全局统一用的工具库，例如：校验手机号，车辆号等等
5. src 目录 manifest.json 这个文件是uni-app跨端框架提供的各个端配置文件
6. src 目录 pages.json 相当于原生小程序的app.json 文件，用来配置各个页面的信息
7. src 目录 pages目录，这个目录是核心目录，不能更改，相当于原生的pages目录
8. src 目录 static目录， 这个目录经过我的实践发现最好不要改其他名字，uni-app给我们做了跨端情况下静态资源打包处理。所以的静态资源再不用CDN的情况下，都可以放在此目录下。
9. src uni.scss 全局的scss 文件，uni-app对适配问题已经做了处理，rpx or px --> unx,具体看官网
10. dist 目录 该目录是编译过后的各端文件，主要分为两类，dev(开发)和buid(production),各个端文件根据文件名来区分，这里开发参考官网和package.json scripts项下的命令

> PS: 目录因人而异，自己怎么开心，就怎么写 ^_^~

### 4. 实践结果
![实践结果图](https://s2.ax1x.com/2019/04/30/EGJN9S.png)
1. 目前由于百度小程序和头条小程序暂为对个人开发者开放，所以只展示 uni-app 在微信小程序和支付宝小程序的跨端实现，如上图所示（左：微信 右：支付宝）支付宝登录授权机制很像微信的授权机制。
2. 采坑： 样式在跨端的时候，有些样式不生效，因为平台不同，有些小程序组件的默认样式也不同。
3. 解决方案：1.我们自己可以内置一套全局默认的样式 2. 采用第三方跨端UI框架，比如uni-app自己实现的一套UI框架。

> PS: 商业项目没有参考示例代码，更多介绍请参见官网： https://uniapp.dcloud.io/