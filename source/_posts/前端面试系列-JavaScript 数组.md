---
title: 前端面试系列-JavaScript 数组
catalog: true
tags:
  - ES6
  - Frontend Interview
  - javascript
url: 156.html
id: 156
categories:
  - 前端面试
date: 2019-02-26 12:45:15
subtitle:
header-img:
---

#### 一. 如何对一个多维数组进行扁平化

首先需要对数组进行降维，有很多种解决方案 假设我们现在有一个高维数组

    const arr = [1,2,[2,323,434,[2323,[2322,5646],23],255]];
    

> 1.  利用递归思想来解决

    let tempArr = [];
    function flattenArrFirst(array){
    array.map(item=>{
        if(Array.isArray(item)){
            tempArr.concat(flattenArrFirst(item));
        }else{
            tempArr.push(item);
        }
    });
    return tempArr;
    }
    
    console.log(flattenArrFirst(arr));
    // 打印结果:
    // [ 1, 2, 2, 323, 434, 2323, 2322, 5646, 23, 255 ]
    

> 2.  利用toString()方法和split方法

    function flattenArrSecond(array){
        let arrStr = array.toString().split(',');
    
        return arrStr.map(item=>Number(item));
    }
    console.log(flattenArrSecond(arr));
    // 打印结果:
    // [ 1, 2, 2, 323, 434, 2323, 2322, 5646, 23, 255 ]
    

> 3.  第三种 利用ES6语法

    function flattenArrThird(array){
        while(array.some(item => Array.isArray(item))){
          array = [].concat(...array);
        }
        return array;
    }
    console.log(flattenArrThird(arr));
    // 打印结果:
    // [ 1, 2, 2, 323, 434, 2323, 2322, 5646, 23, 255 ]
    

> 4.  利用数组reduce方法（高端操作，哈哈 ^_^~）

    function flattenFour(array){
       return array.reduce((result,item)=>{
           return result.concat(Array.isArray(item) ? flattenFour(item) : item)
       },[])
    }
    console.log(flattenFour(arr));
    // 打印结果:
    // [ 1, 2, 2, 323, 434, 2323, 2322, 5646, 23, 255 ]
    

> 5.  利用join方法和split 方法

    function flattenArrFive(array){
        return arr.join(",").split(",").map(item => Number(item))
    }
    
    console.log(flattenArrFive(arr));
    // 打印结果:
    // [ 1, 2, 2, 323, 434, 2323, 2322, 5646, 23, 255 ]
    

#### 二. 如何对一个数组去重

假如现在有数组:

    const arr = [2,243,44,36,2645,22,2,53,2323,6454,42,32];
    

> 1.第一种利用循环遍历

    function uniqueFirst(arr){
        let tempArr = [];
        for (let i = 0; i < arr.length; i++) {
          if(tempArr.indexOf(arr[i]) == -1){
              tempArr.push(arr[i]);
          }
        }
        return tempArr;
    }
    console.log(uniqueFirst(arr));
    // 打印结果：
    // [ 2, 243, 44, 36, 2645, 22, 53, 2323, 6454, 42, 32 ]
    

> 2.第二种排序相邻去重法

    function uniqueSecond(arr){
        arr.sort();
        let tempArr = [arr[0]];
        for(let i = 1; i < arr.length; i++){
           if(tempArr[tempArr.length-1] !== arr[i]){
               tempArr.push(arr[i])
           }
        }
        return tempArr;
    }
    console.log(uniqueSecond(arr));
    // 打印结果：
    // [ 2, 22, 2323, 243, 2645, 32, 36, 42, 44, 53, 6454 ]
    

> 3.第三种利用ES6 Set

    var arr = [2, 2, 2, 243, 44, 36, 2645, 22, 2, 53, 2323, 6454, 42, 32, 2, 2, 2];
    function uniqueThird(arr){
        return Array.from(new Set(arr));
    }
    console.log(uniqueThird(arr));
    // 打印结果：
    // [ 2, 243, 44, 36, 2645, 22, 53, 2323, 6454, 42, 32 ]
    

> 4.第四种利用reduce

    function uniqueFour(array) {
        return array.reduce((a,b)=>{
            a.indexOf(b) == -1 ? false : a.push(b);
            return a
        },[])
    }
    console.log(uniqueFour(arr));
    // 打印结果：
    //[ 2, 22, 2323, 243, 2645, 32, 36, 42, 44, 53, 6454 ]
    

> 5.第五种利用filter和includes方法

    function uniqueFive(array){
        let tempArr = [];
       tempArr = array.filter(item=>{
          return tempArr.includes(item)? false : tempArr.push(item)
       });
       return tempArr;
    }
    console.log(uniqueFive(arr));
    // 打印结果：
    [ 2, 243, 44, 36, 2645, 22, 53, 2323, 6454, 42, 32 ]
    

> 6.第六种 对象键值对法

    function uniqueSix(array){
        let obj = {};
        let tempArr = [];
        for(let i =0;i<array.length;i++){
            if(!obj[array[i]]){
               tempArr.push(array[i]);
               obj[arr[i]] = arr[i];
            }
        }
        return tempArr;
    }
    console.log(uniqueSix(arr));
    // 打印结果：
    [ 2, 243, 44, 36, 2645, 22, 53, 2323, 6454, 42, 32 ]
    

> 7.  面试官追问题：如何对一个对象数组按照规定的字段去重

    var usersArr = [
        { name: "name1", age: "1" },
        { name: "name2", age: "11" },
        { name: "name7", age: "11" },
        { name: "name3", age: "12" },
        { name: "name4", age: "13" },
        { name: "name2", age: "1" },
        { name: "name6", age: "12" }
    ]
    
    function uniqueByFields(array,fields){
       let hash = {}
       return array.reduce((arr,item)=>{
           hash[item[fields]] ? "" : hash[item[fields]] = true && arr.push(item);
           return arr
       },[])
    }
    
    console.log(uniqueByFields(usersArr, "age"));
    // 打印结果：
    [ { name: 'name1', age: '1' },
      { name: 'name2', age: '11' },
      { name: 'name3', age: '12' },
      { name: 'name4', age: '13' } ]
    

> 8.  面试时关联题目：如何对对象数组按照特定字段进行排序

    function sortByFileds(arr,fields){
        return arr.sort((a,b)=>{
            return a[fields] - b[fields];
        });
    }
    console.log(sortByFileds(usersArr, "age"));
    // 输出结果：
    [ { name: 'name1', age: '1' },
      { name: 'name2', age: '1' },
      { name: 'name2', age: '11' },
      { name: 'name7', age: '11' },
      { name: 'name3', age: '12' },
      { name: 'name6', age: '12' },
      { name: 'name4', age: '13' } ]