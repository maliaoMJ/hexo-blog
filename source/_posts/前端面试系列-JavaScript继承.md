---
title: 前端面试系列-JavaScript继承
catalog: true
tags:
  - javascript
url: 178.html
id: 178
categories:
  - JavaScript
date: 2019-03-05 23:12:36
subtitle:
header-img:
---

![](http://www.hnaccp.com/uploads/allimg/170928/0U3021263-5.jpg)

### Javascript 继承

#### 1\. 通过构造函数实现继承

    function Parent(){
        this.parent = 'parent'
        this.getWho = function(){
            return 'getWho:'+this.parent
        }
    }
    Parent.prototype.sayWho = function(){
       console.log(`prototype from ${this.parent}`);
        return `prototype from ${this.parent}`
    }
    
    function Child(){
        Parent.call(this);
        this.child = "child"
    }
    
    let children = new Child();
    
    console.log(children.child);// child
    console.log(children.getWho());// getWho:parent
    console.log(children.parent); // parent
    console.log(children.sayWho()); // TypeError: children.sayWho is not a function
    
    

这个主要是借用 call 来改变 this 的指向，通过 call 调用 Parent ，此时 Parent 中的 this 是指 Child。有个缺点，从打印结果看出 Child 并没有 say 方法，所以这种只能继承父类的实例属性和方法，不能继承原型属性/方法。

#### 2\. 借助原型链实现继承

    function Parent1(){
        this.parent = 'parent'
        this.list = [1,2,3]
        this.lookList=function(){
            return this.list
        }
    }
    Parent1.prototype.getList=function(){
        return this.list
    }
    function Child1(){
        this.child = 'child'
    }
    Child1.prototype = new Parent1();
    
    let s1 = new Child1();
    let s2 = new Child1();
    console.log(s1.list); // [ 1, 2, 3 ]
    s1.list.push(4)
    console.log(s2.list); // [ 1, 2, 3, 4 ]
    console.log(s1.lookList()); // [ 1, 2, 3, 4 ]
    console.log(s1.getList()); // [ 1, 2, 3, 4 ]
    

通过一讲的，我们知道要共享莫些属性，需要 对象.proto = 父亲对象的.prototype,但实际上我们是不能直接 操作proto，这时我们可以借用 new 来做，所以 Child1.prototype = new Parent1(); <=> Child1.prototype.proto = Parent1.prototype; 这样我们借助 new 这个语法糖，就可以实现原型链继承。但这里有个总是，如打印结果，我们给 s1.list 新增一个值 ，s2 也跟着改了。所以这个是原型链继承的缺点，原因是 s1.pro 和 s2.pro指向同一个地址即 父类的 prototype

#### 3\. 组合式继承

    function Parent2(){
        this.parent = 'parent'
        this.list = [1,2,3]
    }
    Parent2.prototype.getList = function(){
        return this.list
    }
    function Child2(){
        Parent2.call(this)
        this.child = 'child';
    }
    Child2.prototype = new Parent2();
    
    let s3 = new Child2();
    let s4 = new Child2();
    
    console.log(s3.getList());// [ 1, 2, 3 ]
    console.log(s4.list);//[ 1, 2, 3]
    s3.list.push(4);
    console.log(s3.list); // [ 1, 2, 3, 4 ]
    

将 1 和 2 两种方式组合起来，就可以解决 1 和 2 存在问题，这种方式为组合继承。这种方式有点缺点就是我实例一个对象的时， 父类 new 了两次，一次是 let s3 = new Child2()对应 Child2.prototype = new Parent2()还要 new 一次。

#### 4\. 寄生组合继承

    function Parent3() {
        this.parent = 'parent3'
        this.list = [1, 2, 3]
    }
    Parent3.prototype.getList = function () {
        return this.list
    }
    function Child3() {
        Parent3.call(this)
        this.child = 'child';
    }
    Child3.prototype =Object.create(Parent3.prototype); //重点
    Child3.prototype.constructor = Child3;// 重点
    let s5 = new Child3();
    let s6 = new Child3();
    
    console.log(s5.getList());// [ 1, 2, 3 ]
    s5.list.push(4);
    console.log(s5.list); // [ 1, 2, 3, 4 ]
    console.log(s6.list);//[ 1, 2, 3]
    

这里主要使用Object.create()，它的作用是将对象继承到proto属性上。这时 Child3.prototype 还是没有自己的 constructor,它要找的话还是向自己的原型对象上找最后还是找到 Parent3.prototype, constructor 还是 Parent3 ,所以要给 Child3.prototype 写自己的 constructor