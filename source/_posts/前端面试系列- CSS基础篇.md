---
title: 前端面试系列- CSS基础篇
catalog: true
tags:
  - CSS3
  - Frontend Interview
  - html
url: 153.html
id: 153
categories:
  - 前端面试
date: 2019-02-26 12:41:20
subtitle:
header-img:
---

![banner](http://pic.97uimg.com/back_pic/20/15/11/09/7a16651588b11b8555782f9f7751bcfd.jpg)

> 一. 有哪几种游览器内核 1\. Trident内核：IE,MaxThon,TT,The Word,360,搜狗浏览器等。\[又称为MSHTML\] 2. Gecko内核：Netscape6及以上版本，FF,MozillaSuite/SeaMonkey等； 3. Presto内核：Opera7及以上。\[Opera内核原为：Presto，现为：Blink\] 4. Webkit内核：Safari,Chrome等。\[Chrome的:Blink(Webkit的分支)\] 二. 如何清除浮动，解决浮动坍塌。 1. 父级div定义height。 2. 结尾处加空div标签clear：both。 3. 父级div定义伪类：after和zoom。 4. 父级div定义overflow：hidden。 5. 父级div定义overflow：auto。 6. 父级div也浮动，需要定义宽度。 7.父级div定义display：table。 8. 结尾处加br标签clear:both

最好的一种方式：伪类清除浮动

    .clearfix:after {
      visibility: hidden;
      display: block;
      font-size: 0;
      content: " ";
      clear: both;
      height: 0;
    }
    .clearfix{
        zoom:1
    }