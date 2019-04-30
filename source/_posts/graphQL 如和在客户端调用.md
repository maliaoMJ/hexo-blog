---
title: graphQL 如和在客户端调用
catalog: true
tags:
  - graphql
  - nodejs
url: 92.html
id: 92
categories:
  - GraphQL
date: 2019-02-20 17:45:16
subtitle:
header-img:
---

![](http://116.85.35.63/wp-content/uploads/2019/02/12.png)

> 源码地址：[github地址](https://github.com/maliaoMJ/graphql-api-server)

#### 1\. 先展示目录结构

![](http://116.85.35.63/wp-content/uploads/2019/02/13.png)

#### 2\. 如何搭建

> 1.  在上一篇文章的基础上，在 `index.js` 添加Express静态文件托管并且指明静态文件夹目录

    app.use(express.static('public'));
    

> 2.  创建public目录并且在此文件中创建`index.html` 文件

    <!DOCTYPE html>
    <html lang="en">
    
    <head>
        <meta charset="UTF-8">
        <title>Learn GraphQL Page</title>
        <style type="text/css">
        #btn {
            width: 100px;
            height: 41px;
            background: #2196F3;
            border-radius: 23px;
            color: #fff;
            text-align: center;
            line-height: 41px;
            cursor: pointer;
            box-shadow: 4px 3px 19px rgba(0, 0, 0, 0.5);
        }
        </style>
    </head>
    
    <body>
        <div id="btn">GET DATA</div>
    </body>
    </html>
    

此时你可以访问`localhost:3000/index.html` 可以看见你创建的index.html 文件，如下图：

![](http://116.85.35.63/wp-content/uploads/2019/02/14.png)

> 3.  我们要请求GraphQL数据 `(GraphQL 只支持GET和POST请求)`，在index.html的js中，添加如下代码：

    let btn = document.getElementById('btn');
    btn.onclick = function() {
        const userId = 10001;
        const query = `query AllUser($id:Int!) {
        userInfo(id: $id) {
            name
            age
            gender
        }
      }`;
        fetch('/graphql', {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "Accept": "application/json"
                },
                body: JSON.stringify({
                    query,
                    variables: { id: userId }
                })
            }).then(res => res.json())
            .then(resData => {
                console.log(resData.data);
            })
    
    }
    

1.  这里的query 请求的查询语句
2.  这里的variables 是变量
3.  这里的$id:Int!中的`!` 表示该参数不能为空,具体细节看下图:

![](http://116.85.35.63/wp-content/uploads/2019/02/15.png)

> 4.  点击按钮，请求数据，如图所示

![](http://116.85.35.63/wp-content/uploads/2019/02/16.png)

#### 3\. 最后附源代码：

> 1.  index.js 代码

    const express = require('express');
    const { buildSchema } = require('graphql');
    const graphqlHTTP = require('express-graphql');
    const originData = [{
            id: 10000,
            name: 'superMario',
            gender: 'man',
            age: 23
        },
        {
            id: 10001,
            name: 'Jack',
            gender: 'man',
            age: 23
        },
        {
            id: 10002,
            name: 'Tom',
            gender: 'man',
            age: 23
        },
        {
            id: 10003,
            name: 'Alice',
            gender: 'woman',
            age: 23
        }
    ]
    
    const app = express();
    // Express 托管静态文件
    app.use(express.static('public'));
    
    const schema = buildSchema(`
        type User {
            name: String
            age: Int
            id: Int
            gender: String
        }
      type Query {
        hello: String
        users: [User!]
        randomUser: User
        userInfo(id:Int!):User
      }
    `)
    
    const rootValue = {
        hello: () => 'hello world',
        users: () => {
            return originData
        },
        randomUser: () => {
            let random = Number.parseInt(Math.random() * 3);
            return originData[random];
        },
        userInfo: ({id}) => {
            const tempData = originData.filter(item => item.id == id);
            return tempData[0]
        }
    }
    
    app.use('/graphql', graphqlHTTP({
        schema,
        rootValue,
        graphiql: true
    }));
    
    app.listen(3000, () => {
        console.log("Now open localhost:3000!");
    });
    
    

> 2.  index.html

    <!DOCTYPE html>
    <html lang="en">
    
    <head>
        <meta charset="UTF-8">
        <title>Learn GraphQL Page</title>
        <style type="text/css">
        #btn {
            width: 100px;
            height: 41px;
            background: #2196F3;
            border-radius: 23px;
            color: #fff;
            text-align: center;
            line-height: 41px;
            cursor: pointer;
            box-shadow: 4px 3px 19px rgba(0, 0, 0, 0.5);
        }
        </style>
    </head>
    
    <body>
        <div id="btn">GET DATA</div>
    </body>
    <script type="text/javascript">
    let btn = document.getElementById('btn');
    btn.onclick = function() {
        const userId = 10001;
        const query = `query AllUser($id:Int!) {
        userInfo(id: $id) {
            name
            age
            gender
        }
      }`;
        fetch('/graphql', {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "Accept": "application/json"
                },
                body: JSON.stringify({
                    query,
                    variables: { id: userId }
                })
            }).then(res => res.json())
            .then(resData => {
                console.log(resData.data);
            })
    
    }
    </script>
    
    </html>
    

> 3.  执行命令,打开游览器localhost:3000/index.html

      nodemon index.js