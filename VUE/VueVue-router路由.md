# Vue:Vue-router路由

#### 1、现在当前的目录下执行

```java
npm install vue-router --save-dev
```

下载好需要的包

#### 2、使用vue-router

```js
import VueRouter from 'vue-router'

//显示的使用vue-router
Vue.use(VueRouter)
```

#### 3、先在component里面创建好页面，创建一个router文件夹，下面创建index.js路由转发。在里面配置

```js
import Vue from 'vue'
import VueRouter from "vue-router";

import Content from '../components/Content'
import Main from '../components/Main'
import lgq from '../components/lgq'

//安装路由
Vue.use(VueRouter);

//配置导出路由
export default new VueRouter({
  routes:[
    {
      path:'/content',
      component:Content,
  },
    {
      path: '/main',
      component: Main
    },
    {
      path:'/lgq',
      component:lgq
    }
  ]
});

```

#### 4、在app.vue里面把链接加进入，

```js
 <router-link to="/main">首页</router-link>
    <router-link to="/content">内容页</router-link>
    <router-link to="/lgq">林贵全</router-link>
    <router-view></router-view>
```

​	还有在main.js里面把路由使用给导入进去

```js
import Vue from 'vue'
import App from './App'
import VueRouter from "vue-router";

import router from './router'

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  //使用路由
  router,
  components: { App },
  template: '<App/>'
})
```

![image-20201104172039160](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201104172039160.png)