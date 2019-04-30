---
title: JavaScript数据结构与算法-集合
catalog: true
tags:
  - javascript
  - 数据结构
url: 195.html
id: 195
categories:
  - 数据结构
date: 2019-03-07 14:49:23
subtitle:
header-img:
---

#### 1\. 什么是集合 ？

集合是由一组无序且唯一(即不能重复)的项组成的。这个数据结构使用了与有限集合相同 的数学概念，但应用在计算机科学的数据结构中。

#### 2\. 代码实现集合

    // 数据结构 之 集合 我们用ES6来实现
    
    class Collect {
      constructor() {
        this.set = new Set();
      }
      add(element) {
        this.set.add(element);
      }
      delete(element) {
        this.set.delete(element);
      }
      has(element) {
        return this.set.has(element);
      }
      clear() {
        this.set = new Set();
      }
      size() {
        return this.set.size();
      }
      values() {
        return [...this.set]
      }
    }
    
    let setA = new Collect();
    let setB = new Collect();
    
    setA.add(2);
    setA.add(3);
    setA.add(4);
    setA.add(4);
    setA.add(5);
    console.log(setA.values());
    console.log(setA.has(3));
    
    

#### 3\. 集合操作 交集

交集的数学概念是集合A和集合B的交集

     intersection(otherSet) {
        let intersectionSet = new Set();
        for (let value of this.set.values()) {
          if (otherSet.has(value)) {
            intersectionSet.add(value)
          }
        }
        return intersectionSet;
      }
    

#### 4\. 集合操作 并集

并集的数学概念是集合A和集合B的并集

      union(otherSet) {
        return new Set([...this.set].concat([...otherSet.set]));
      }
    

#### 5\. 集合操作 差集

差集的数学概念是集合A和集合B的差集

     difference(otherSet) {
        let differSet = new Set();
        for (let value of this.set.values()) {
          if (!otherSet.has(value)) {
            differSet.add(value);
          }
        }
        return differSet;
      }
    

#### 6\. 集合操作 子集

子集的数学概念是集合A是集合B的子集

     subset(childSet) {
        for (let value of childSet.set) {
          if (!this.set.has(value)) {
            return false
          }
        }
        return true
      }
    

#### PS: 完整代码以及结果

    // 数据结构 之 集合 我们用ES6来实现
    
    class Collect {
      constructor() {
        this.set = new Set();
      }
      add(element) {
        this.set.add(element);
      }
      delete(element) {
        this.set.delete(element);
      }
      has(element) {
        return this.set.has(element);
      }
      clear() {
        this.set = new Set();
      }
      size() {
        return this.set.size();
      }
      value() {
        return [...this.set]
      }
      // 交集
      intersection(otherSet) {
        let intersectionSet = new Set();
        for (let value of this.set.values()) {
          if (otherSet.has(value)) {
            intersectionSet.add(value)
          }
        }
        return intersectionSet;
      }
      // 并集
      union(otherSet) {
        return new Set([...this.set].concat([...otherSet.set]));
      }
      // 差集
      difference(otherSet) {
        let differSet = new Set();
        for (let value of this.set.values()) {
          if (!otherSet.has(value)) {
            differSet.add(value);
          }
        }
        return differSet;
      }
      // 子集
      subset(childSet) {
        for (let value of childSet.set) {
          if (!this.set.has(value)) {
            return false
          }
        }
        return true
      }
    }
    
    let setA = new Collect();
    let setB = new Collect();
    
    setA.add(2);
    setA.add(3);
    setA.add(4);
    setA.add(4);
    setA.add(5);
    setB.add(4);
    setB.add(4);
    setB.add(5);
    console.log(setA.value());
    console.log(setB.value());
    console.log(setA.has(3));
    console.log(setA.union(setB));
    console.log(setA.intersection(setB));
    console.log(setA.difference(setB));
    console.log(setA.subset(setB));
    // [ 2, 3, 4, 5 ]
    // [ 4, 5 ]
    // true
    // Set { 2, 3, 4, 5 }
    // Set { 4, 5 }
    // Set { 2, 3 }
    // true