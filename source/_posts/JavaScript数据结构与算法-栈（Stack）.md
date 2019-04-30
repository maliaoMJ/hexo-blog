---
title: JavaScript数据结构与算法-栈（Stack）
catalog: true
tags:
  - javascript
  - 数据结构
url: 181.html
id: 181
categories:
  - 数据结构
date: 2019-03-06 17:51:10
subtitle:
header-img:
---

### JavaScript 数据结构-栈

#### 1\. 什么是栈？

栈是一种遵从后进先出(LIFO)原则的有序集合。新添加的或待删除的元素都保存在栈的 同一端，称作栈顶，另一端就叫栈底。在栈里，新元素都靠近栈顶，旧元素都接近栈底。栈也被用在编程语言的编译器和内存中保存变量、方法调用等。 #

#### 2\. 用JavaScript实现一个简单的栈

    class Stack {
      constructor() {
        this.items = []
      }
      // 进栈
      push(item) {
        this.items.push(item);
      }
      // 出栈
      pop() {
        return this.items.pop();
      }
      // 返回栈顶的元素
      peek() {
        return this.items[this.items.length - 1]
      }
      // 判断栈是否为空
      isEmpty() {
        return this.items.length == 0;
      }
      // 移除栈里的所有元素
      clear() {
        this.items = []
      }
      // 返回栈的长度
      size() {
        return this.items.length;
      },
      print() {
          return this.items.toString();
      }
    }
    
    let stack1 = new Stack();
    console.log(stack1);  // Stack { items: [] }
    
    

#### 3\. 用栈解决问题

栈的实际应用非常广泛。在回溯问题中，它可以存储访问过的任务或路径、撤销的操作(后 面的章节讨论图和回溯问题时，我们会学习如何应用这个例子)。Java和C#用栈来存储变量和方 法调用，特别是处理递归算法时，有可能抛出一个栈溢出异常。 既然我们已经了解了Stack类的用法，不妨用它来解决一些计算机科学问题。本节，我们将 学习使用栈的三个最著名的算法示例。首先是十进制转二进制问题，以及任意进制转换的算法; 然后是平衡圆括号问题;最后，我们会学习如何用栈解决汉诺塔问题。

> 1.  十进制转二进制

    function divideBy2(decNumber){
        let stack = new Stack();
        let binaryString = "";
        let rem = "";
        while(decNumber > 0){
            rem = Math.floor(decNumber % 2);
            stack.push(rem);
            decNumber = Math.floor(decNumber / 2);
        }
        while(!stack.isEmpty()){
            binaryString += stack.pop().toString() 
        }
        return binaryString;
    }
    console.log(divideBy2(5)); // 101
    

> 2.  十进制转任意进制

    function baseConverter(decNumber, base) {
      let stack = new Stack();
      let binaryString = '';
      let rem = '';
      const DIGITS = '0123456789ABCDEF';
      while (decNumber > 0) {
        rem = Math.floor(decNumber % base);
        stack.push(rem);
        decNumber = Math.floor(decNumber / 2);
    
      }
      while (!stack.isEmpty()) {
        binaryString += DIGITS[stack.pop()]
      }
      return binaryString;
    }
    console.log(baseConverter(455, 15)); //137EDB825