---
title: JavaScript数据结构与算法-字典和哈希表
catalog: true
tags:
  - javascript
  - 数据结构
url: 199.html
id: 199
categories:
  - 数据结构
date: 2019-03-10 15:56:49
subtitle:
header-img:
---

#### 1\. 什么是字典？

集合表示一组互不相同的元素(不重复的元素)。在字典中，存储的是\[键，值\] 对，其中键名是用来查询特定元素的。字典和集合很相似，集合以\[值，值\]的形式存储元素，字 典则是以\[键，值\]的形式来存储元素。字典也称作映射。

#### 2\. 手动实现Dirctionary

    // 字典 Dirctionary
    
    class Dirctionary {
        constructor(){
            this.items = new Map();
        }
        set(key,value){
            this.items.set(key,value);
        }
        get(key){
            return this.items.get(key);
        }
        remove(key){
            this.items.delete(key);
        }
        has(key){
            return this.items.has(key);
        }
        size(){
            return this.items.size();
        }
        clear(){
            this.items = new Map();
        }
        values(){
            const arr = []
            for(let key of this.items.keys()){
              arr.push(this.items.get(key));
            }
            return arr;
        }
    
    }
    
    

#### 3\. 什么是散列表

散列算法的作用是尽可能快地在数据结构中找到一个值。在之前的章节中，你已经知道如果 要在数据结构中获得一个值(使用get方法)，需要遍历整个数据结构来找到它。如果使用散列 函数，就知道值的具体位置，因此能够快速检索到该值。散列函数的作用是给定一个键值，然后 返回值在表中的地址。

#### 4\. 手动实现一个散列表

    // 散列表
    
    var loseloseHashCode = function (key) {
        var hash = 0;                          
        for (var i = 0; i < key.length; i++) { 
            hash += key.charCodeAt(i);         
        }
        return hash % 37; 
    };
    function HashTable() {
        var table = [];
    
        this.put = function (key, value) {
            var position = loseloseHashCode(key);
            console.log(position + ' - ' + key);
            table[position] = value; 
        };
        this.get = function (key) {
            return table[loseloseHashCode(key)];
        };
        this.remove = function (key) {
            table[loseloseHashCode(key)] = undefined;
        };
    }
    var hash = new HashTable();
    hash.put('Gandalf', 'gandalf@email.com');
    hash.put('John', 'johnsnow@email.com');
    hash.put('Tyrion', 'tyrion@email.com');
    // 19 - Gandalf
    // 29 - John
    // 16 - Tyrion