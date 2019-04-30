---
title: JavaScript数据结构与算法-链表（LinkedList）
catalog: true
tags:
  - javascript
  - 数据结构
url: 191.html
id: 191
categories:
  - 数据结构
date: 2019-03-07 11:52:47
subtitle:
header-img:
---

#### 1.什么是链表？

要存储多个元素，数组(或列表)可能是最常用的数据结构。正如本书之前提到过的，每种 语言都实现了数组。这种数据结构非常方便，提供了一个便利的\[\]语法来访问它的元素。然而， 这种数据结构有一个缺点:(在大多数语言中)数组的大小是固定的，从数组的起点或中间插入 或移除项的成本很高，因为需要移动元素(尽管我们已经学过的JavaScript的array类方法可以帮 我们做这些事，但背后的情况同样是这样)。 链表存储有序的元素集合，但不同于数组，链表中的元素在内存中并不是连续放置的。每个 元素由一个存储元素本身的节点和一个指向下一个元素的引用(也称指针或链接)组成。下图展 示了一个链表的结构:

![](http://116.85.35.63/wp-content/uploads/2019/03/11111111111.png)

#### 2\. 实现一个链表结构（单向链表）

    // JavaScript 数据结构值列表
    function LinkedList() {
      let Node = function (element) {
        this.element = element;
        this.next = null;
      }
      let length = 0;
      let head = null;
      // 添加元素
      this.append = function (element) {
        let node = new Node(element);
        let current;
        if (head == null) {
          head = node
        } else {
          current = head;
          while (current.next) {
            current = current.next
          }
          current.next = node
        }
        length++;
      }
      // 插入元素
      this.insert = function (position, element) {
        if (position > -1 && position <= length) {
          let node = new Node(element);
          let current = head;
          let pervious;
          let index = 0;
          if (position == 0) {
            node.next = current;
            head = node;
          } else {
            while (index++ < position) {
              pervious = current;
              current = current.next;
            }
            node.next = current;
            pervious.next = node;
            length++;
            return true;
          }
        } else {
          return false;
        }
      }
      // 从列表中移除一项
      this.getHead = function () {
        return head;
      }
      // 从列表移除指定的一项
      this.removeAt = function (position) {
        if (position > -1 && position < length) {
          let current = head;
          let previous;
          let index = 0;
          // 边界检查合法的情况下
          if (position == 0) {
            head = current.next;
          } else {
            while (index++ < position) {
              previous = current;
              current = current.next;
            }
            previous.next = current.next;
          }
          length--;
          return current.element;
        } else {
          return false;
        }
      }
      // 返回元素在列表中的索引
      this.indexOf = function (element) {
        let current = head;
        index = 0;
        while (current) {
          if (current.element === element) {
            return index;
          }
          index++;
          current = current.next();
        }
        return -1;
      }
      // 判断列表是否为空
      this.isEmpty = function () {
        return length === 0;
      }
      // 获取列表的长度
      this.size = function () {
        return length;
      }
      // 由于列表项使用了Node类，就需要重写继承自JavaScript对象默认的toString方法，让其只输出元素的值。
      this.toString = function () {
        let current = head,
          string = '';
        while (current) {
          string += current.element + (current.next ? 'n' : '');
          current = current.next;
        }
        return string;
      }
      this.remove = function (element) {
        let index = this.indexOf(element);
        return this.removeAt(index);
      }
    }
    
    let list = new LinkedList();
    list.append(15);
    list.append(14);
    list.append(13);
    list.append(13);
    
    console.log(list);
    console.log(list.toString()); // 15n14n13n13
    
    

#### 3 .双向链表

链表有多种不同的类型，这一节介绍双向链表。双向链表和普通链表的区别在于，在链表中， 一个节点只有链向下一个节点的链接，而在双向链表中，链接是双向的:一个链向下一个元素， 另一个链向前一个元素，如下图所示:

![](http://116.85.35.63/wp-content/uploads/2019/03/222222222.png)

#### 4.循环链表

`循环链表`可以像链表一样只有单向引用，也可以像双向链表一样有双向引用。循环链表和链 表之间唯一的区别在于，最后一个元素指向下一个元素的指针(tail.next)不是引用null， 而是指向第一个元素(head)，如下图所示。

![](http://116.85.35.63/wp-content/uploads/2019/03/3333333.png)

#### 5\. 双向循坏列表

双向循环链表有指向head元素的tail.next，和指向tail元素的head.prev

![](http://116.85.35.63/wp-content/uploads/2019/03/444444.png)