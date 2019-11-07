---
title: 阅读Vue Composition API 了解Vue3.x的设计思想（下篇）
catalog: true
tags:
  - JavaScript
  - TypeScript
  - Vue
categories:
  - Vue
date: 2019-10-19 18:09:42
subtitle:
header-img: "https://img.zcool.cn/community/0144ed58660585a8012060c810d41f.jpg@2o.jpg"
---

# API Reference 

> 要看源码之前，首先要学会TypeScript（前十篇文档有`Typescript`学习指南）,因为vue3.x 版本中+90%的代码是使用的`Typescript`，[Vue3.x 源码地址](https://github.com/vuejs/vue-next)。

## ReadOnly
  接收一个对象（reactive or plain）或ref并返回一个对原始对象的只读（and reactive）代理。

    ```javascript
        const original = reactive({ count: 0 })

        const copy = readonly(original)

        watch(() => {
        // works for reactivity tracking
        console.log(copy.count)
        })

        // mutating original will trigger watchers relying on the copy
        original.count++

        // mutating the copy will fail and result in a warning
        copy.count++ // warning!
    ```
    
## ReadOnly

### 未完待续.......
> 各种API源码解析