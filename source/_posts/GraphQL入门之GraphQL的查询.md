---
title: GraphQL入门之GraphQL的查询
catalog: true
tags:
  - graphql
url: 69.html
id: 69
categories:
  - GraphQL
date: 2019-02-19 13:18:01
subtitle:
header-img:
---

#### 

GraphQL入门之GraphQL的查询

![](http://img0.imgtn.bdimg.com/it/u=3309702283,977143577&fm=26&gp=0.jpg)

#### 先附上源代码文件

> app.js

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
    

> 然后运行

     nodemon app.js
    

> 打开游览器访问 localhost:3000/graphql

#### 一. 字段（Fields）

> 1.  简单而言，GraphQL 是关于请求对象上的特定字段。我们以一个非常简单的查询以及其结果为例：

![](http://116.85.35.63/wp-content/uploads/2019/02/1.png)

> 查询语句如下：

    query LearnGraphQLFields{
     hello
    }
    

> 在前一例子中，我们请求返回了一个字符串类型（String），但是字段也能指代对象类型（Object）。这个时候，你可以对这个对象的字段进行次级选择（sub-selection）。GraphQL 查询能够遍历相关对象及其字段，使得客户端可以一次请求查询大量相关数据，而不像传统 REST 架构中那样需要多次往返查询。

![](http://116.85.35.63/wp-content/uploads/2019/02/2.png)

> 查询语句如下：

    query LearnGraphQLFieldsObject {
      randomUser {
        name
        age
        gender
        id
      }
    }
    

#### 二. 参数（Arguments）

> 即使我们能做的仅仅是遍历对象及其字段，GraphQL就已经是一个非常有用的数据查询语言了。但是当你加入给字段传递参数的能力时，事情会变得更加有趣。

![](http://116.85.35.63/wp-content/uploads/2019/02/8.png)

> 查询语句如下：

    query LearnGraphQLArguments {
      userInfo(id: 10001) {
        name
        age
        gender
        id
      }
    }
    
    

#### 三. 别名（Aliases）

> 你可能已经发现，即便结果中的字段与查询中的字段能够匹配，但是因为他们并不包含参数，你就没法通过不同参数来查询相同字段。这便是为何你需要别名 ——这可以让你重命名结果中的字段为任意你想到的名字。

![](http://116.85.35.63/wp-content/uploads/2019/02/5.png)

> 查询语句如下：

    query LearnGraphQLArguments {
      userItemDetail: userInfo(id: 10001) {
        name
        age
        gender
        id
      }
    }
    

#### 四. 片段（Fragments）

> 假设我们的 app 有比较复杂的页面。你立马就能想到对应的查询会变得复杂，因为我们需要将一些字段重复至少一次。两方各一次以作比较。这就是为何 GraphQL 包含了称作片段的可复用单元。片段使你能够组织一组字段，然后在需要它们的的地方引入。下面例子展示了如何使用片段解决上述场景：

![](http://116.85.35.63/wp-content/uploads/2019/02/6.png)

> 查询语句如下：

    query LearnGraphQLArguments {
      userItemDetail:userInfo(id: 10001) {
        ...useItem
      }
    }
    
    fragment useItem on User {
      name
      age
      id
      gender
    }
    
    

#### 五. 变量（Variables）

> 目前为止，我们将参数写在了查询字符串内。但是在很多应用中，字段的参数可能是动态的。将这些动态参数直接传进查询字符串并不是好主意，因为这样我们的客户端就得动态地在运行时操作这些查询字符串了，再把它序列化成 GraphQL 专用的格式。其实，GraphQL拥有一级方法将动态值提取到查询之外，然后作为分离的字典传进去。这些动态值即称为变量。使用变量之前，我们得做三件事：

1.  使用 $variableName 替代查询中的静态值。
2.  声明 $variableName 为查询接受的变量之一。
3.  将 variableName: value 通过传输专用（通常是 JSON）的分离的变量字典中。

![](http://116.85.35.63/wp-content/uploads/2019/02/9.png)

> 查询语句如下：

    query getUser($id:Int=10001){
      userDetailInfo:userInfo(id: $id){
        name,
        age,
        gender
      }
    }
    

#### 六. 操作名称（Operation name）

> 这之前，我们都使用了简写句法，省略了 query 关键字和查询名称，但是生产中使用这些可以使我们代码减少歧义。操作类型可以是 query、mutation 或 subscription，描述你打算做什么类型的操作。操作类型是必需的，除非你使用查询简写语法，在这种情况下，你无法为操作提供名称或变量定义。操作名称是你的操作的有意义和明确的名称。它仅在有多个操作的文档中是必需的，但我们鼓励使用它，因为它对于调试和服务器端日志记录非常有用。 当在你的网络日志或是 GraphQL 服务器中出现问题时，通过名称来从你的代码库中找到一个查询比尝试去破译内容更加容易。 就把它想成你喜欢的程序语言中的函数名。例如，在 JavaScript 中，我们只用匿名函数就可以工作，但是当我们给了函数名之后，就更加容易追踪、调试我们的代码，并在其被调用的时候做日志。同理，GraphQL 的查询和变更名称，以及片段名称，都可以成为服务端侧用来识别不同 GraphQL 请求的有效调试工具。

#### 七. 指令（Directives）

> 我们上面讨论的变量使得我们可以避免手动字符串插值构建动态查询。传递变量给参数解决了一大堆这样的问题，但是我们可能也需要一个方式使用变量动态地改变我们查询的结构。譬如我们假设有个 UI 组件，其有概括视图和详情视图，后者比前者拥有更多的字段。我们来构建一个这种组件的查询：

![](http://116.85.35.63/wp-content/uploads/2019/02/10.png)

> 查询语句如下：

    query getUser($id:Int=10001,$showId:Boolean=false){
      userDetailInfo:userInfo(id: $id){
        name,
        age,
        gender
      }
      userList: users{
        name @include(if:$showId)
        age
        gender
      }
    }
    

> #### 更多学习请参考 [GraphQL官网](http://graphql.cn/)