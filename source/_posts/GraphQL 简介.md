---
title: GraphQL 简介
catalog: true
tags:
  - graphql
url: 12.html
id: 12
categories:
  - GraphQL
date: 2019-02-18 16:12:40
subtitle:
header-img:
---

### GraphQL 简介

![](http://static.oschina.net/uploads/space/2018/0723/081132_6JEI_2720166.png)

#### 1\. 什么是GraphQL ?

1.  GraphQL是一种新的API标准，它提供了一种更高效、强大和灵活的数据提供方式。它是由Facebook 2015年开发和开源，目前由来自世界各地的大公司和个人维护。是REST API的替代品。
    
2.  GraphQL本质上是一种基于api的查询语言，现在大多数应用程序都需要从服务器中获取数据，这些数据存储可能存储在数据库中，API的职责是提供与应用程序需求相匹配的存储数据的接口。
    
3.  GraphQL 既是一种用于 API 的查询语言也是一个满足你数据查询的运行时。 GraphQL 对你的 API 中的数据提供了一套易于理解的完整描述，使得客户端能够准确地获得它需要的数据，而且没有任何冗余，也让 API 更容易地随着时间推移而演进，还能用于构建强大的开发者工具。
    

#### 2\. GraphQL 有什么特点？

1.  请求的数据，不多不少 \> 例如： User 中有name,age,gender,department等信息字段，我们可以只取得需要的字段。
2.  获取多个资源，只用一个请求。
3.  描述所有可能的类型系统。便于维护，根据需求平滑演进，添加或者隐藏字段。

#### 3\. GraphQL与Restful API 对比

1.  restful: representational State Transfer 表属性状态转移。本质就是用定义uri,通过API接口来取得资源。通用系统架构，不受语言限制。
2.  restful一个接口只能返回一个资源，graphql 一次可以获取多个资源。
3.  restful用不同的url来区分资源，graphql用类型区分资源。

#### 4\. 构建一个GraphQL简单的服务端（Nodejs Express）

1.  定义查询的Schema和类型

       const Schema = buildSchema(
       `
        type Query {
            hello: String
        }
       `);
    

2.  定义查询对应的处理器

    const rootValue = {
        hello: ()=>{
            return "hello graphql!"
        }
    }
    
    

3.  结合Schema和对应的处理器

    app.use('/graphql', graphqlHTTP({
        schema,
        rootValue,
        graphiql: true // 打开graphql语言查询调试图形界面
    }))
    

4.  打开游览器，输入localhost:3000/graphql,在graphiql 图形界面中输入查询语句

      nodemon app.js
    

    query {
        hello
    }
    

> 结果如图 [![](https://i.loli.net/2019/02/18/5c6a69c78a41b.png)](https://i.loli.net/2019/02/18/5c6a69c78a41b.png) 源码文件 app.js

    // npm install express exprss-graphql graphql --save
    const express = require('express');
    const graphqlHTTP = require('express-graphql');
    const { buildSchema } = require('graphql');
    const app = express();
    const schema = buildSchema(`
        type Query {
            hello: String
        }
       `);
    const rootValue = {hello: () => "hello graphql"};
    app.use('/graphql', graphqlHTTP({
        schema,
        rootValue,
        graphiql: true
    }));
    app.listen(3000, ()=>{
        console.log("Now browser to localhost:3000/graphql!");
    })