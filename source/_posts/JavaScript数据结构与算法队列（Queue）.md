---
title: JavaScript数据结构与算法队列（Queue）
catalog: true
tags:
  - javascript
  - 数据结构
url: 184.html
id: 184
categories:
  - 数据结构
date: 2019-03-06 17:53:17
subtitle:
header-img:
---

#### 1\. 什么是队列？

队列是遵循FIFO(First In First Out，先进先出，也称为先来先服务)原则的一组有序的项。 队列在尾部添加新元素，并从顶部移除元素。最新添加的元素必须排在队列的末尾。在计算机科学中，一个常见的例子就是打印队列。比如说我们需要打印五份文档。我们会打 开每个文档，然后点击打印按钮。每个文档都会被发送至打印队列。第一个发送到打印队列的文 档会首先被打印，以此类推，直到打印完所有文档。

#### 2\. 代码实现一个队列Queue

    class Queue {
    
      constructor() {
        this.items = [];
      }
      // 向队列添加元素
      push(element) {
        this.items.push(element);
      }
      // 从队列移除元素
      shift() {
        return this.items.shift();
      }
      // 查看队列头元素
      peek() {
        return this.items[0];
      }
      // 检查队列是否为空
      isEmpty() {
        return this.items.length === 0;
      }
      // 打印队列
      print() {
        return this.items.toString();
      }
    
    }
    
    

#### 3\. 什么是优先队列？

队列大量应用在计算机科学以及我们的生活中，我们在之前话题中实现的默认队列也有一些 修改版本。 其中一个修改版就是优先队列。元素的添加和移除是基于优先级的。一个现实的例子就是机 场登机的顺序。头等舱和商务舱乘客的优先级要高于经济舱乘客。在有些国家，老年人和孕妇(或 带小孩的妇女)登机时也享有高于其他乘客的优先级。 另一个现实中的例子是医院的(急诊科)候诊室。医生会优先处理病情比较严重的患者。通 常，护士会鉴别分类，根据患者病情的严重程度放号。 实现一个优先队列，有两种选项:设置优先级，然后在正确的位置添加元素;或者用入列操 作添加元素，然后按照优先级移除它们。在这个示例中，我们将会在正确的位置添加元素，因此 可以对它们使用默认的出列操作。

    function PriorityQueue() {
      let items = [];
      function QueueElement (element, priority){ // {1}
        this.element = element;
        this.priority = priority;
      }
      this.enqueue = function(element, priority){
        let queueElement = new QueueElement(element, priority);
        let added = false;
        for (let i=0; i<items.length; i++){
          if (queueElement.priority < items[i].priority){ // {2}
            items.splice(i,0,queueElement); // {3}
            added = true;
            break; // {4}
    } }
        if (!added){
          items.push(queueElement); //{5}
    } };
      this.print = function(){
        for (let i=0; i<items.length; i++){
          console.log(`${items[i].element} -
          ${items[i].priority}`);
        }
    };
    //其他方法和默认的Queue实现相同 }
    

#### 4\. 什么是循环队列？

还有另一个修改版的队列实现，就是循环队列。循环队列的一个例子就是击鼓传花游戏(Hot Potato)。在这个游戏中，孩子们围成一个圆圈，把花尽快地传递给旁边的人。某一时刻传花停止， 这个时候花在谁手里，谁就退出圆圈结束游戏。重复这个过程，直到只剩一个孩子(胜者)。

> 循环队列——击鼓传花

    function hotPotato(nameList, num) {
      var queue = new Queue();
      var eliminated = "";
      for (let i = 0; i < nameList.length; i++) {
        queue.push(nameList[i]);
      }
      while (queue.size() > 0) {
        for (let i = 0; i < num; i++) {
          queue.push(queue.shift())
        }
        eliminated = queue.shift();
        console.log(`eliminated: ${eliminated}`);
      }
      return eliminated
    }
    var nameList = ['java', 'cpp', 'c#', 'golang', 'php', 'basic', 'javascript', 'python'];
    hotPotato(nameList, 3);
    
    // eliminated: golang
    // eliminated: python
    // eliminated: php
    // eliminated: cpp
    // eliminated: java
    // eliminated: c#
    // eliminated: javascript
    // eliminated: basic
    

#### 5\. JavaScript 任务队列

当我们在浏览器中打开新标签时，就会创建一个任务队列。这是因为每个标签都是单线程处 理所有的任务，它被称为事件循环。浏览器要负责多个任务，如渲染HTML，执行JavaScript代码， 处理用户交互(用户输入、鼠标点击等)，执行和处理异步请求。