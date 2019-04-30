---
title: 如何在Vue SPA中集成GraphQL
catalog: true
tags:
  - graphql
  - nodejs
  - vuejs
url: 101.html
id: 101
categories:
  - GraphQL
date: 2019-02-20 18:50:50
subtitle:
header-img:
---

![](https://camo.githubusercontent.com/e78e52aa36ff76ef5e142bfeced3b5f657b3fc26/68747470733a2f2f63646e2d696d616765732d312e6d656469756d2e636f6d2f6d61782f3830302f312a483941414e6f6f664c716a53313058643554775259772e706e67)

> 1.  为了快速搭建，首先我们安装vue-cli@3.x

     sudo npm install -g @vue/cli
     yarn global add @vue/cli
    

> 2.  我们快速搭建一个Vue-SPA项目

      mkdir learn_vue_with_graphql
      cd learn_vue_with_graphql
      vue create vue-ssr-graphql
      # 按照你自己的需求选择插件
      # 安装完毕后，进入vue-ssr-graphql目录
      cd vue-ssr-graphql
    

> 3.  安装关于在vue中使用graphQL的一些包

    npm install --save vue-apollo graphql apollo-client apollo-link apollo-link-http apollo-cache-inmemory graphql-tag
    

或者

    yarn add vue-apollo graphql apollo-client apollo-link apollo-link-http apollo-cache-inmemory graphql-tag
    

> 4.  在项目`src` 目录下创建`apolloClient.js`文件

    import { ApolloClient } from 'apollo-client';
    import { HttpLink } from 'apollo-link-http';
    import { InMemoryCache } from 'apollo-cache-inmemory';
    
    const httpLink = new HttpLink({
      uri: 'http://localhost:3000/graphql'
    });
    
    const cache = new InMemoryCache();
    // 导出 ApolloClient graphQL客户端
    export default new ApolloClient({
      link: httpLink,
      cache
    });
    

> 5.  修改`main.js`安装插件到Vue和Apollo provider（Provider 保存了可以在接下来被所有子组件使用的 Apollo 客户端实例。）

    import Vue from 'vue'
    import App from './App.vue'
    import router from './router'
    import store from './store'
    import apolloClient from './apolloClient'
    import './registerServiceWorker'
    import VueApollo from 'vue-apollo';
    
    Vue.config.productionTip = false
    
    const apolloProvider = new VueApollo({
      defaultClient: apolloClient
    })
    Vue.use(VueApollo);
    new Vue({
      router,
      store,
      apolloProvider,
      render: h => h(App)
    }).$mount('#app')
    
    

> 6.在组件中使用（需要配合graphQL服务端）

服务端源码地址：[github](https://github.com/maliaoMJ/graphql-api-server)

1.  首先打开`views`目录下的`Home.vue` 文件

在vue文件的JS部分做如下修改

    <script>
    // @ is an alias to /src
    import gql from "graphql-tag";
    export default {
      name: "home",
      computed: {
        loading() {
          return this.$apollo.loading;
        }
      },
      data() {
        return {
          hello: ""
        };
      },
      mounted() {
        console.log(this.$apollo.loading, this.$apollo.data);
      },
      apollo: {
        hello: {
          query: gql`
            query getUserInfo($id: Int!) {
              hello
              userInfo(id: $id) {
                name
                age
                gender
                id
              }
            }
          `,
          variables: {
            id: 10001
          }
        }
      }
    };
    </script>
    
    

2.  运行项目

运行如下命令

    yarn run serve
    

然后打开游览器访问`localhost:8080`可以看到如下结果

![](http://116.85.35.63/wp-content/uploads/2019/02/16-1.png)

##### 最后附上源代码文件地址 [github](https://github.com/maliaoMJ/vue-graphQL-template)