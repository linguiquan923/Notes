# Vue快速实战开发

#### vue和elementUI(应该以管理员的权限去运行)

#### 1、创建项目。初始化一个项目，并且下载好vue-router和element-ui

```java
vue init webpack hello1-vue
cd hello1-vue
   //导入路由
npm install vue-router --save-dev
    //导入element-ui
npm i element-ui -S
npm install
    //导入css编辑器
cnpm install sass-loader node-sass --save-dev
npm run dev
```

#### 2、删除包里面不需要的东西，然后创建router包和view包，创建好路由管理和视图

路由管理

```js
import Vue from 'vue'
import Router from 'vue-router'
import Main from "../views/Main";
import Login from "../views/Login";

Vue.use(Router);

export default new Router({
  routes:[
    {
      path:'/main',
      component:Main
  },{
      path: '/login',
      component: Login
    }]
});

```

视图只需要在element上面复制进去那些代码就可以

#### 3、在main.js和app.vue需要做出一定的修改

需要导入element-ui和router，还有相对应的index.css

把路由router添加进去，添加render: h=>h(App)，

打开终端，运行npm run dev

```
import router from './router'
import Element from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'

Vue.use(Element)
Vue.use(router)

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  render: h=>h(App)
})
```

#### 4、路由的嵌套

```js
routes:[
    {
      path:'/main',
      component:Main,
      children:[{
        path:'/user/userprofile',
        component: UserProfile
      },{
        path:'/user/userlist',
        component: UserList
      }]
  },{
      path: '/login',
      component: Login
    }]
```

#### 5、重定向和参数传递

在router-link修改参数，这个name:'UserProfile'是要在index.js （路由转发核心文件）中提前定义好路由的名字

```js
<router-link :to="{name:'UserProfile',params:{id: 1}}">个人信息</router-link>
```

然后在index.js收到信息，并且修改传递路径

```js
path:'/user/userprofile/:id',
        name:'UserProfile',
        component: UserProfile
```

最后在被传递到的页面使用$route.params.id取值使用，**要注意的是要在标签里面使用，否则会报错**

```js
<div>
    <h1>用户信息</h1>
    {{$route.params.id}}</div>
```

也可以使用props，在index.js里面添加props:true

```js
children:[{
        path:'/user/userprofile/:id',
        name:'UserProfile',
        component: UserProfile,
        props:true
      }
```

在相应的页面接收 ,props:['id']接收，{{id}}直接使用

```java 
<template>
  <div>
    <h1>用户信息</h1>
<!--    {{$route.params.id}}-->
    {{id}}
  </div>
</template>
<script>
    export default {
        props:['id'],
        name: "UserProfile"
    }
</script>
<style scoped>
</style>

```

##### 重定向

只需要添加一个路由，然后通过rediretct去执行即可

```js
 {
      path:'/goHome',
      redirect:'/main'
    }
```

#### 6、404页面

只需要新建一个NotFound的页面，然后建立一个路由 就可以

```js
 {
      path:'*',
      component:NotFound
    }
```

#### 7、钩子函数(跟过滤器很像)

next()跳入下一个页面

next('/path')改变路由的方向，跳向指定的方向

next(false)返回原本的页面

next((vm)=>{})仅在beforeRouteEnter中可以用，vm是组件实例。

只需要在对应的页面中加入代码就可以实现：

```js
<script>
    export default {
        props:['id'],
        name: "UserProfile",
        beforeRouteEnter:(to,from,next)=>{
            console.log("进图路由之前");
             next((vm) => {

                vm.getData()
                // this.getData()
            });
        },
        beforeRouteLeave:(to,from,next)=>{
            console.log("进入路由之后");
            next()
        }
    }
</script>
```

#### 8、在钩子函数中使用异步请求

1. ##### 	安装Axios

   1. ```js
      cnpm install axios -s
      npm install --save vue-axios
      ```

2. ##### 在main.js中引入

   1. ```
      import Axios from 'axios'
      import VueAxios from 'vue-axios'
      
      Vue.use(Axios,VueAxios)
      ```

