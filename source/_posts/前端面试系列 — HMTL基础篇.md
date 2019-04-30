---
title: 前端面试系列 — HMTL基础篇
catalog: true
url: 133.html
id: 133
categories:
  - HTML5
date: 2019-02-26 12:25:17
subtitle:
header-img:
tags:
---

![banner image](http://pic.90sjimg.com/back_pic/qk/back_origin_pic/00/01/52/d2060da501854ef147f75e63d4412978.jpg)

> ##### 1\. HTML中首行 有什么作用? doctype 又分哪几类?其中这些每一类由对应有什么作用？请简单描述一下。

1.  DOCTYPE 是 document type(文档类型的）的简写。是为了告诉浏览器需要通过哪一种规范（文档类型定义，DTD）解析文档（比如HTML或XHTML规范）
2.  DTD（document type definition，文档类型定义）是一系列的语法规则，用来定义 XML 或 (X)HTML 的文件类型。浏览器会使用它来判断文档类型， 决定使用何种协议来解析，以及切换浏览器模式。
3.  DTD总体分为三类，HTML4.0 HTML5, XHML。其中HTML4.0 分为三种模式：严格模式(strict)，过渡模式（Transitional）, Frameset模式。区别主要在于是不是允许展示性和弃用的元素，是否允许框架集。
4.  如何不写DOCTYPE，游览器可会以怪异模式来渲染文档，最好还是写DOCTYPE，这样游览器会以w3c 标准模式来渲染文档。

> ##### 2\. HTML 与 XML 有什么区别

主要区别： XML区分大小写。 XML标签必须闭合，单元素需要以/作为闭合结尾，嵌套不能出错。 XML属性必须放在引号中。 XML属性必须有属性值，不能省略。 XML中空格不会被自动删除。

> ##### 3.说说html中有哪些块级元素，那些内联元素，以及块级元素与内联元素的区别。

1.  块级元素 address, blockquoto, center, dir, div, dl,fieldset, form, h1-h6,hr,ol,ul,li,p,pre,table,th,tbody,td,tfoot,thead,tr...
2.  内联元素 a, abbr, acronym, b, big, br, cite, code, dfn, em, font, i, img, input, kdb,label,q,slect,small,span,strike,strong,sub,sup,textarea,tt,u...
3.  判断行内元素和块级元素的快捷方法就是判断是否能并列。

> ##### 4.说说HTML5新增了那些元素，以及如何更好的语义化。

1.  header--显示头部信息，article---定义独立内容，常用于用户评语或博客条目，section---节，例如文章的章节、页眉、页脚等，该标记中还新增一个cite属性，引用资源的URL，nav---导航链接，在一个html中可以有多个nav， hgroup---适用于多标题文档，对标题进行组合，可以理解hgroup相当一个容器，里面包含有正副标题， footer---眉脚，页面底部，footer元素内使用address元素， aside定义所处的内容之外的内容，但与附近内容有关（注释、引用、提示）， figure 定义媒介内容， dialog定义对话框或窗口，带有open属性，跟用户互动， mark元素标记，相当于荧光笔在纸上标记文字，time元素定义时间（24小时制）、日期，meter元素定义度量衡，process元素定义运行中的进度。
2.  其他新增元素：video、audio、source、embed、canvas、bdi、command、datalist、details、keygen、output、rp、rt、ruby、track、summary等

> #### 5.HTML5新增了那些API?
> 
> 1.  Media API
> 2.  Text Track API
> 3.  Application Cache API
> 4.  User Interaction
> 5.  Data Transfer API
> 6.  Command API
> 7.  Constraint Validation API
> 8.  History API
> 9.  Canvas API
> 10.  Geolocation API等等