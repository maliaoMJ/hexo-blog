---
title: 前端面试系列-JavaScript中的this指向问题
catalog: true
tags:
  - Frontend Interview
  - html
  - javascript
url: 150.html
id: 150
categories:
  - 前端面试
date: 2019-02-26 12:39:33
subtitle:
header-img:
---

谁调用它他就指向谁，箭头函数没有this

> 1.  new 的过程中发生了什么

1.  首先创建一个新的对象
2.  链接到原型
3.  绑定this
4.  返回新的对象

代码实现：

    function create() {
      let obj = {};
      let Constructor = [].shift.call(arguments);
      obj.__proto__ = Constructor.prototype
      let result = Constructor.apply(obj, arguments);
      return typeof result === 'object' ? result : obj
    }
    

> 2.  怎么实现bind, call, apply 手动实现一下
> 3.  如果不传入参数默认是window
> 4.  改变this的指向，让新的对象执行该函数
> 5.  先给新的对象添加一个该函数，然后执行，执行后删除

代码模拟实现Call：

    Function.prototype.myCall = function (context) {
      var context = context || window;
      context.fn = this;
      let args = [...arguments].slice(1);
      let result = context.fn(...args);
      delete context.fn;
      return result;
    }
    

代码模拟实现Apply：

    Function.prototype.myApply = function (context) {
      var context = context || window;
      context.fn = this;
      var result;
      if (arguments[1]) {
        result = context.fn(...arguments[1]);
      } else {
        result = context.fn();
      }
      delete context.fn
      return result;
    }
    

代码模拟实现Bind：

    Function.prototype.myBind = function (context) {
      if (typeof this != 'object') {
        throw new TypeError('type error');
      }
      var self = this;
      var args = [...arguments].slice(1);
      return function Fn() {
        if (typeof this instanceof Fn) {
          return new self(...args, ...arguments);
        } else {
          return self.apply(context, args.concat(...arguments));
        }
      }
    }
    
    

> 3.instanceof 原理是什么？手动实现一下

    function instanceOf(left, right) {
      let prototype = right.prototype;
      let left = left.__proto__
      while (true) {
        if (left == null) return false
        if (left == prototype) return true
        left = left.__proto__
      }
    }