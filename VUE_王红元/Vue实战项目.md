

要注意脚手架的版本

### 1、划分目录结构

![image-20201222102847460](Images\image-20201222102847460.png)

### 2、normalize.css文件引入

为什么引入：

​	Normalize.css只是一个很小的css文件，但它在磨人的HTML元素样式上提供了跨浏览器的高度一致性。相比于传统的CSS reset,Normalize.css是一种现代的、为HTML5准备的优质替代方案。总之，Normalize.css是一种CSS reset的替代方案。

#### 1、新建normalize.css

可以在github搜索，找到源码

```css
/*! normalize.css v8.0.1 | MIT License | github.com/necolas/normalize.css */

/* Document
   ========================================================================== */

/**
 * 1. Correct the line height in all browsers.
 * 2. Prevent adjustments of font size after orientation changes in iOS.
 */

html {
    line-height: 1.15; /* 1 */
    -webkit-text-size-adjust: 100%; /* 2 */
}

/* Sections
   ========================================================================== */

/**
 * Remove the margin in all browsers.
 */

body {
    margin: 0;
}

/**
 * Render the `main` element consistently in IE.
 */

main {
    display: block;
}

/**
 * Correct the font size and margin on `h1` elements within `section` and
 * `article` contexts in Chrome, Firefox, and Safari.
 */

h1 {
    font-size: 2em;
    margin: 0.67em 0;
}

/* Grouping content
   ========================================================================== */

/**
 * 1. Add the correct box sizing in Firefox.
 * 2. Show the overflow in Edge and IE.
 */

hr {
    box-sizing: content-box; /* 1 */
    height: 0; /* 1 */
    overflow: visible; /* 2 */
}

/**
 * 1. Correct the inheritance and scaling of font size in all browsers.
 * 2. Correct the odd `em` font sizing in all browsers.
 */

pre {
    font-family: monospace, monospace; /* 1 */
    font-size: 1em; /* 2 */
}

/* Text-level semantics
   ========================================================================== */

/**
 * Remove the gray background on active links in IE 10.
 */

a {
    background-color: transparent;
}

/**
 * 1. Remove the bottom border in Chrome 57-
 * 2. Add the correct text decoration in Chrome, Edge, IE, Opera, and Safari.
 */

abbr[title] {
    border-bottom: none; /* 1 */
    text-decoration: underline; /* 2 */
    text-decoration: underline dotted; /* 2 */
}

/**
 * Add the correct font weight in Chrome, Edge, and Safari.
 */

b,
strong {
    font-weight: bolder;
}

/**
 * 1. Correct the inheritance and scaling of font size in all browsers.
 * 2. Correct the odd `em` font sizing in all browsers.
 */

code,
kbd,
samp {
    font-family: monospace, monospace; /* 1 */
    font-size: 1em; /* 2 */
}

/**
 * Add the correct font size in all browsers.
 */

small {
    font-size: 80%;
}

/**
 * Prevent `sub` and `sup` elements from affecting the line height in
 * all browsers.
 */

sub,
sup {
    font-size: 75%;
    line-height: 0;
    position: relative;
    vertical-align: baseline;
}

sub {
    bottom: -0.25em;
}

sup {
    top: -0.5em;
}

/* Embedded content
   ========================================================================== */

/**
 * Remove the border on images inside links in IE 10.
 */

img {
    border-style: none;
}

/* Forms
   ========================================================================== */

/**
 * 1. Change the font styles in all browsers.
 * 2. Remove the margin in Firefox and Safari.
 */

button,
input,
optgroup,
select,
textarea {
    font-family: inherit; /* 1 */
    font-size: 100%; /* 1 */
    line-height: 1.15; /* 1 */
    margin: 0; /* 2 */
}

/**
 * Show the overflow in IE.
 * 1. Show the overflow in Edge.
 */

button,
input { /* 1 */
    overflow: visible;
}

/**
 * Remove the inheritance of text transform in Edge, Firefox, and IE.
 * 1. Remove the inheritance of text transform in Firefox.
 */

button,
select { /* 1 */
    text-transform: none;
}

/**
 * Correct the inability to style clickable types in iOS and Safari.
 */

button,
[type="button"],
[type="reset"],
[type="submit"] {
    -webkit-appearance: button;
}

/**
 * Remove the inner border and padding in Firefox.
 */

button::-moz-focus-inner,
[type="button"]::-moz-focus-inner,
[type="reset"]::-moz-focus-inner,
[type="submit"]::-moz-focus-inner {
    border-style: none;
    padding: 0;
}

/**
 * Restore the focus styles unset by the previous rule.
 */

button:-moz-focusring,
[type="button"]:-moz-focusring,
[type="reset"]:-moz-focusring,
[type="submit"]:-moz-focusring {
    outline: 1px dotted ButtonText;
}

/**
 * Correct the padding in Firefox.
 */

fieldset {
    padding: 0.35em 0.75em 0.625em;
}

/**
 * 1. Correct the text wrapping in Edge and IE.
 * 2. Correct the color inheritance from `fieldset` elements in IE.
 * 3. Remove the padding so developers are not caught out when they zero out
 *    `fieldset` elements in all browsers.
 */

legend {
    box-sizing: border-box; /* 1 */
    color: inherit; /* 2 */
    display: table; /* 1 */
    max-width: 100%; /* 1 */
    padding: 0; /* 3 */
    white-space: normal; /* 1 */
}

/**
 * Add the correct vertical alignment in Chrome, Firefox, and Opera.
 */

progress {
    vertical-align: baseline;
}

/**
 * Remove the default vertical scrollbar in IE 10+.
 */

textarea {
    overflow: auto;
}

/**
 * 1. Add the correct box sizing in IE 10.
 * 2. Remove the padding in IE 10.
 */

[type="checkbox"],
[type="radio"] {
    box-sizing: border-box; /* 1 */
    padding: 0; /* 2 */
}

/**
 * Correct the cursor style of increment and decrement buttons in Chrome.
 */

[type="number"]::-webkit-inner-spin-button,
[type="number"]::-webkit-outer-spin-button {
    height: auto;
}

/**
 * 1. Correct the odd appearance in Chrome and Safari.
 * 2. Correct the outline style in Safari.
 */

[type="search"] {
    -webkit-appearance: textfield; /* 1 */
    outline-offset: -2px; /* 2 */
}

/**
 * Remove the inner padding in Chrome and Safari on macOS.
 */

[type="search"]::-webkit-search-decoration {
    -webkit-appearance: none;
}

/**
 * 1. Correct the inability to style clickable types in iOS and Safari.
 * 2. Change font properties to `inherit` in Safari.
 */

::-webkit-file-upload-button {
    -webkit-appearance: button; /* 1 */
    font: inherit; /* 2 */
}

/* Interactive
   ========================================================================== */

/*
 * Add the correct display in Edge, IE 10+, and Firefox.
 */

details {
    display: block;
}

/*
 * Add the correct display in all browsers.
 */

summary {
    display: list-item;
}

/* Misc
   ========================================================================== */

/**
 * Add the correct display in IE 10+.
 */

template {
    display: none;
}

/**
 * Add the correct display in IE 10.
 */

[hidden] {
    display: none;
}
```



#### 2、新建base.css

代码搜索王红元老师的GitHub找到的，王红元老师的github

```java
https://github.com/coderwhy/supermall/blob/master/src/assets/css/base.css
```



```css
@import "./normalize.css";

/*:root -> 获取根元素html*/
:root {
    --color-text: #666;
    --color-high-text: #ff5777;
    --color-tint: #ff8198;
    --color-background: #fff;
    --font-size: 14px;
    --line-height: 1.5;
}

*,
*::before,
*::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: "Helvetica Neue",Helvetica,"PingFang SC","Hiragino Sans GB","Microsoft YaHei","微软雅黑",Arial,sans-serif;
    user-select: none; /* 禁止用户鼠标在页面上选中文字/图片等 */
    -webkit-tap-highlight-color: transparent; /* webkit是苹果浏览器引擎，tap点击，highlight背景高亮，color颜色，颜色用数值调节 */
    background: var(--color-background);
    color: var(--color-text);
    /* rem vw/vh */
    width: 100vw;
}

a {
    color: var(--color-text);
    text-decoration: none;
}


.clear-fix::after {
    clear: both;
    content: '';
    display: block;
    width: 0;
    height: 0;
    visibility: hidden;
}

.clear-fix {
    zoom: 1;
}

.left {
    float: left;
}

.right {
    float: right;
}

```

然后记得在App.vue里面去注册它

```vue
<style>
  @import "assets/css/base.css";
</style>
```





### 3、配置别名vue.config.js

在**supermall目录下**新建**vue.config.js**

```js
module.exports = {
  configureWebpack:{
    resolve:{
      alias:{
        //默认配置了@，src
        'assets': '@/assets',
        'common': '@/common',
        'components': '@/components',
        'network': '@/network',
        'views': '@/views',
      }
    }
  }
}
```

### 4、.editorconfig文件

vue-cli2才有，这里从旧代码复制过来

这个文件很重要，可以配置统一的缩进符号等，如果身为一个组长，那么就必须配置好这个文件

### 5、tabbar的引入和项目模块的划分

从旧代码把tabbat拷贝过来



#### 5.1、引入img图片



#### 5.2、导入路由

下载路由

```java
npm install vue-router
```

编辑router/index.js文件，即路由文件

在main.js注册路由

```java
import Vue from 'vue'
import App from './App.vue'
import router from "./router";

Vue.config.productionTip = false

new Vue({
  render: function (h) { return h(App) },
  router
}).$mount('#app')

```

在App.vue使用TabBar

```vue
<template>
  <div id="app">
    <router-view/>
    <main-tar-bar/>
  </div>
</template>

<script>

  import MainTarBar from "./components/content/mainTaBbar/MainTarBar";

export default {
  name: 'app',
  components: {
    MainTarBar
  }
}
</script>
```

### 6、首页导航栏的封装和使用

#### 6.1、创建一个新的NavBar放在，因为是每个版本都需要用到的



#### 6.2、NavBar编辑设置插槽和css

在里设置插槽要使用具名插槽，那么每次替换元素的时候才可以准确的选择到

#### 6.3、在Home里面使用

这里推荐背景颜色还有字体颜色在Home里面设置，因为每一个NavBar的颜色样式都是不一样的

### 7、请求首页多个数据

添加封装的network，安装axios

```java
npm install axios --save
```

#### 添加一个home.js封装Home的网络请求，专门面对Home的网络请求，home.js是面对request的。Home->home->request

#### 1、home.js

```js
import {request} from "./request";

export function getHomeMultidata() {
  return request({
    url: '/home/multidata'
  })
}

export function getHomeGoods(type,page) {
  return request({
    url: '/home/data',
    params:{
      type,
      page
    }
  })
}

```

#### 2、request.js封装好的网络请求

```js
import axios from 'axios'

export function request(config) {
     //1.创建axios的实例
      const instance = axios.create({
        baseURL: 'http://123.207.32.32:8000',
        timeout: 5000
      })
      //2、axios拦截器的使用
      //2.1请求拦截
      instance.interceptors.request.use((config) => {
        return config
      },(err) => {
        // console.log(err);
      })
      //2.1响应拦截
      instance.interceptors.response.use((res)=>{
        return res.data
      },(err)=>{
        console.log(err);
      })
      //3.发送真正的网络请求
      return instance(config)
}

```



在Home.vue调用网络请求获取数据

在组件创建的时候采用created去创建网络请求，在data里面初始化两个参数去接收返回的数据

```vue
import {getHomeMultidata} from "network/home";

export default {
    name: "Home",
    components:{
      NavBar
    },
    data(){
      return {

        // results: null
        banners: [],
        recommends: []

      }
    },
    created() {
      //1、请求多个数据，加括号调用函数，
      getHomeMultidata().then((res)=>{
        // console.log(res);
        // this.results = res
        this.banners = res.data.banner.list
        this.recommends = res.data.recommend.list
      })
    },
```

### 8、轮播图的展示

#### 1、先封装成组件再调用，直接复制包装好的组件Swiper

![image-20201201154833076](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201201154833076.png)

#### 2、在Home.vue里面导入Swiper，并且使用

先导入Swiper和SwiperItem，然后在template里面用for循环取出返回数据的链接和图片，将他们放入到对应的a标签和图片中

```vue
<template>
  <div id="home" class="wrapper">
    <NavBar class="home-nav"><div slot="center">购物车</div></NavBar>
    <swiper>
      <swiper-item v-for="item in banners">
        <a :href="item.link">
          <img :src="item.image"/>
        </a>
      </swiper-item>
    </swiper>
  </div>
</template>

<script>

import NavBar from "components/common/navbar/NavBar";
import {getHomeMultidata} from "network/home";
import {Swiper,SwiperItem} from 'components/common/swiper'

export default {
    name: "Home",
    components:{
      NavBar,
      Swiper,
      SwiperItem
    },
    data(){
      return {

        // results: null
        banners: [],
        recommends: []

      }
    },
    created() {
      //1、请求多个数据，加括号调用函数，
      getHomeMultidata().then((res)=>{
        // console.log(res);
        // this.results = res
        this.banners = res.data.banner.list
        this.recommends = res.data.recommend.list
      })
    },
  }
</script>
```

#### 3、把Swiper部分抽取出来重新封装

在home文件夹下面新建childcomps

```vue
<template>
  <div id="home" class="wrapper">
    <swiper>
      <swiper-item v-for="item in banners">
        <a :href="item.link">
          <img :src="item.image"/>
        </a>
      </swiper-item>
    </swiper>
  </div>
</template>

<script>
  import {Swiper,SwiperItem} from 'components/common/swiper'
  export default {
    name: "HomeSwiper",
    props:{
      banners:{
        type: Array,
        default(){
          return[]
        }
      }
    },
    components:{
      Swiper,
      SwiperItem
    }
  }
</script>

<style scoped>

</style>

```

#### 4、在Home使用该组件

要在Home.vue位置把数据传入到组件当中

```vue
<home-swiper :banners="banners"/>
```

### 9、首页开发-推荐信息展示

#### 1、封装组件

调整css，然后在template里面去调整数据展示位置，要在Home.vue传入数据，在组件里面接收数据

```vue
<template>
  <div class="recommend">
    <div v-for="item in recommends" class="recommend-item">
      <a :href="item.link">
        <img :src="item.image"/>
        <div>{{item.title}}</div>
      </a>
    </div>
  </div>
</template>

<script>
  export default {
    name: "RecommendView",
    props:{
      recommends:{
        type: Array,
        default(){
          return[]
        }
      }
    }
  }
</script>

<style scoped>
  .recommend{
    display: flex;
    text-align: center;
    font-size: 12px;

    padding: 10px 0 20px;
    border-bottom: 10px solid #eee;
  }
  .recommend-item{
    flex: 1;
  }
  .recommend-item img{
    height: 70px;
    width: 70px;
    margin-bottom: 10px;
  }
</style>

```

### 10、本周流行开发

#### 1、封装组件

```vue
<template>
  <div class="feature">
      <a href="https://act.mogujie.com/zzlx67">
        <img src="~assets/img/home/recommend_bg.jpg"/>
      </a>
  </div>
</template>

<script>
  export default {
    name: "featureView"
  }
</script>

<style scoped>
  .feature img{
    width: 100%;
  }
</style>

```

#### 2、在Home.vue使用

```vue
import FeatureView from "./childComps/FeatureView";

components:{
FeatureView
}
```

### 11、TabControl的封装

​	我们不需要每个都采取插槽的设置，因为每一个所需要的内容都是不一样的，只有纯文字的话我们只需要传入对应的文字就可以了

​	先建立一个独立的封装组件，在里面获取需要的titles，在Home中获取并且使用它，并且传入对应的数据，利用display: flex去排列它，使用for循环去遍历，使用:class="{active: index === currentIndex}"去实现点击变色的功能。

​	我们还需要实现滑动到顶部会停止，因为只需要在Home里面实现这种功能，所以我们在Home的界面给它添加css。

```vue
<template>
  <div class="tab-control">
    <div v-for="(item,index) in titles"
         class="tab-control-item"
         :class="{active: index === currentIndex}"
          @click="itemClick(index)">
      <span>{{item}}</span>
    </div>
  </div>
</template>

<script>
  export default {
    name: "TabControl",
    props:{
      titles:{
        type:Array,
        default(){
          return[]
        }
      }
    },
    data(){
      return{
        currentIndex: 0
      }
    },
    methods:{
      itemClick(index){
        // console.log(index);
        this.currentIndex = index
      }
    }
  }
</script>

<style scoped>
  .tab-control{
    display: flex;
    text-align: center;
    height: 44px;
    line-height: 44px;
    font-size: 15px;
  }
  .tab-control-item{
    flex: 1;

  }
  .tab-control-item span{
    padding: 5px;
  }

  .active{
    color: var(--color-high-text);
  }
  .active span{
    border-bottom: 3px solid var(--color-high-text);
  }


</style>

```

```vue
<tab-control :titles="['流行','新款','精选']" class="tab-control"/>
//滑动到距离顶部停止，这是暂时使用的方法
.tab-control{
    position: sticky;
    top: 44px;
  }
```

### 12、保存商品的数据结构

​	因为要同时保存流行，精选等数据信息，我们需要同时请求所有的数据。但是随着用户的点击，比如说用户下拉到流行的第五页，这时候点击精选的时候应该显示精选当时的页数。我们选择一下的数据结构去存储。默认的page为0

```java
goods:{
          'pop':{page: 0 , list:[]},
          'news':{page: 0 , list:[]},
          'sell':{page: 0 , list:[]},
        }
```

# ##################2020.12.01#####################



### 13、我们开发的时候created里面只放一些逻辑流程，methods里面放具体操作

```vue
created() {
      //1、请求多个数据，加括号调用函数，
      this.getHomeMultidata()
      //2、获取商品的所有信息
      this.getHomeGoods()
    },
    methods:{
      getHomeMultidata(){
        getHomeMultidata().then((res)=>{
          // console.log(res);
          // this.results = res
          this.banners = res.data.banner.list
          this.recommends = res.data.recommend.list
        })
      },
      getHomeGoods(type){
        getHomeGoods(type,1).then((res)=>{
        console.log(res);
      })
      }
    }
```

### 14、利用解析的方法把网络请求数据数组里面的数据一一添加到data里面的数组

```vue
getHomeGoods(type){
        const page = this.goods[type].page + 1
        getHomeGoods(type,page).then((res)=>{
        // console.log(res);
        this.goods[type].list.push(...res.data.list)
      })
      }
```



### *、从这里开始拿不到接口数据，只展示逻辑代码

### 15、首页开发-首页商品的展示

#### 1、首先我们要创建一个GoodsList的组件，这个组件是关于首页商品展示的部分

在里面接收从Home传来的数据，利用一个数组去接收，使用for循环去遍历每一个对象，把遍历出来的对象放在GoodsListItem里面去展示数据

```vue
<!-- <goods-list :goods="goods['pop'].list"/> --> //Home传入到GoodList获取pop的数据
```

##### GoodsList

```vue
<template>
  <div class="goods">
      <h2>{{goods}}</h2>
      <!-- <goods-list-item v-for="item in goods" :goods-item="item"></goods-list-item> -->
  </div>
</template>

<script>
import GoodsListItem from './GoodsListItem.vue'

export default {
    name: "GoodsList",
    components:{
        GoodsListItem
    },
    props:{
        goods:{
            type: Array,
            default(){
                return []
            }
        }
    }
}
</script>

<style>
</style>
```

##### GoodsListItem

使用goodsItem去接收数据

```vue
<template>
  <div>我是goodsListItem</div>
</template>

<script>
export default {
    name: 'GoodListItem',
    props:{
        goodsItem:{
            type: Object,
            default(){
                return{}
            }
        }
    }
}
</script>

<style>

</style>
```

实现的效果

![image-20201202113944685](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201202113944685.png)

### 16、tabControl点击切换商品

#### 1、点击按钮，子组件传数据到父组件，Home组件获取所点击的信息，根据返回的数据决定要传递给子组件什么数据渲染

tabControl.vue

在 这里实现传递下标出去，然后告诉Home自己点击的是哪一个主题，传出tabClick的事件产生

```vue
methods:{
      itemClick(index){
        // console.log(index);
        this.currentIndex = index
        this.$emit('tabClick',index)
      }
    }
```

在Home监听传出的tabClick事件，在methods中实现方法

```vue
 <tab-control :titles="['流行','新款','精选']" 
                  class="tab-control"
                  @tabClick="tabClick"
                  />
```

在method中，现在data定义一个变量currentType，因为我们的传入值是不是固定的，通过switch的语句去决定要传入的值是什么。

```
<goods-list :goods="goods[currentType].list"/>
```

```vue
methods:{
      /**
       * 事件监听的方法
       */
      tabClick(index){
        // console.log(index);
        switch(index){
          case 0:
            this.currentType = 'pop';
            break;
          case 1:
            this.currentType = 'new';
            break;
          case 2:
            this.currentType = 'sell'
            break;
        }
      },
```

用计算属性去代替

```vue
computed:{
      showGoods(){
        return this.goods[this.currentType].list
      }
    }
```

### 17、Better-Scroll的安装和使用

解决滑动卡顿的问题

#### 1、安装框架

```java
npm install better-scroll --save
```

#### 2、使用import引入

#### 3、使用

```vue
data(){
        return{
            scroll: null
        }
    },
    mounted() {
        console.log(document.querySelector('.wrapper'))
        this.scroll = new BScroll(document.querySelector('.wrapper'),{
        })
    },
```

#### 4、注意要实现设定好wrapper的高度

![image-20201202172136334](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201202172136334.png)

#### 5、Better-Scroll的基本使用

##### 1、检测滚动probeType

```vue
mounted() {
        console.log(document.querySelector('.wrapper'))
        //默认情况下不侦测，probe侦测的意思
        //0,1都是不侦测
        //2：手指滑动的时候侦测
        //3: 只要滑动都侦测
        this.scroll = new BScroll(document.querySelector('.wrapper'),{
            probeType: 3
        })
        this.scroll.on('scroll',(position)=>{
            console.log(position)
        })
    },
```

![image-20201202173726717](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201202173726717.png)

##### 2、click

项目补充

##### 3、pullUpLoad上拉加载更多

要先设置pullUpLoad: true，

监听pullingUp事件，上拉一次之后是不可以再次使用了，得使用**this.scroll.finishPullUp()**告诉组件已经上拉完毕，可以再次请求上拉

```vue
mounted() {
        console.log(document.querySelector('.wrapper'))
        //默认情况下不侦测，probe侦测的意思
        //0,1都是不侦测
        //2：手指滑动的时候侦测
        //3: 只要滑动都侦测
        this.scroll = new BScroll(document.querySelector('.wrapper'),{
            probeType: 3,
            pullUpLoad: true
        })
        this.scroll.on('scroll',(position)=>{
            // console.log(position)
        })
        this.scroll.on('pullingUp', ()=> {
            console.log('上拉加载中')
            //发送网络请求，请求更多数据

            //等待数据请求完成，而且将更新的数据展示出来之后
            setTimeout(() => {
                this.scroll.finishPullUp()
            }, 2000);
        })
    },
```



# ##################2020.12.03###################

### 18、Better-Scroll的封装和使用

#### 1、封装scroll

```vue
<template>
  <div class="wrapper" ref="wrapper">
      <div class="content">
          <slot></slot>
      </div>
  </div>
</template>

<script>
    import BScroll from 'better-scroll'
export default {
    name: "Scroll",
    data(){
        return{
            scroll: null
        }
    },
    mounted(){
        // console.log(this.$refs.wrapper);
        this.scroll = new BScroll(this.$refs.wrapper,{
            probeType:2,
            pullUpLoad: true
        })
        this.scroll.on('scroll',(position)=>{
            // console.log(position);
        })
        this.scroll.on('pullingUp',()=>{
            console.log('上拉加载中')
            setTimeout(() => {
                this.scroll.finishPullUp()
            }, 2000);
            
        })
    }
}
</script>

<style>

</style>
```

#### 2、在home.vue中使用

```vue
<scroll class="content">
</scroll>
```

#### 3、给scroll设置高度

home要设置好定位，然后content才会跟着生效，属性scoped是作用域的意思，因为有这个，我们定义的css是不会跑到别的组件去，只会在本组件生效

```vue
<style scoped>
  #home {
    padding-top: 22px;
    /*height: 100px;*/
    position: relative;
  }
  .content{
    /* height: 400px; */
    /* height: calc(100% - 93px); */
    /* overflow: hidden; */
    position: absolute;
    top: 22px;
    bottom: 49px;
    right: 0;
    left: 0;
  }

</style>

```

### 19、BackTop的封装和使用

#### 封装BackTop

```vue
<template>
  <div class="back-top">
      <img src="~assets/img/common/top.png"/>
  </div>
</template>

<script>
export default {
    name: 'BackTop',
}
</script>

<style>
    .back-top{
        position: fixed;
        right: 8px;
        bottom: 55px;
    }
    .back-top img{
        width: 43px;
        height: 43px;
    }
</style>
```

修改scroll，可以直接调用scroll的方法实现回到顶部

#### 调用

在Home中，是没有办法直接监听子组件的点击事件，需要添加native属性

```vue
<back-top @click.native = "backClick"/>
```



```vue
backClick(){
        // console.log("backClick");
        this.$refs.scroll.scrollTo(0,0,1000)
      },

```

可以在Home直接实现this.$refs.scroll..scroll.scrollTo(0,0,1000)调用scroll的scroll属性，修改属性，但是我们为了体现封装思想，在scroll里面把这个方法实现，我们只需要在Home调用这个方法即可，而不是在Home里面对scroll的元素进行操作

```vue
 methods:{
        scrollTo(x,y,time = 500){
            this.scroll.scrollTo(x,y,time)
        }
    },
```

#### 显示和隐藏

##### 1、实现实时监听位置

我们包装scroll的时候是不能把是否监听直接赋值常量的，应该赋值变量，让使用它的人决定是否要实现监听，在Home传入probeType的值，使用props接收它

```
this.scroll = new BScroll(this.$refs.wrapper,{
            probeType: this.probeType,     
```

##### 2、把检测到的位置信息发送出去

```vue
this.scroll.on('scroll',(position)=>{
            // console.log(position);
            this.$emit('scroll',position)
        })
```

##### 3、在Home获取到位置信息，使用v-show来确定是否显示

```vue
 <back-top @click.native = "backClick" v-show="isShowBackTop"/>
//判断y的位置来决定是否显示
contentScroll(position){
        // console.log(position);
        if(-position.y > 1000){
          this.isShowBackTop = true
        }else if(-position.y <= 1000){
          this.isShowBackTop = false
        }
      },
```

### 20、完成上拉加载更多

#### 1、把上上拉加载更多的值赋予变量，将上拉事件发送出去

```vue
 this.scroll = new BScroll(this.$refs.wrapper,{
            probeType: this.probeType,
            pullUpLoad: this.pullUpLoad
        })

this.scroll.on('pullingUp',()=>{
            this.$emit('pullingUp')
        })
```

#### 2、Home监听上拉事件

```vue
<scroll class="content" 
            ref="scroll" 
            :probe-type="0" 
            :pull-up-load="isPullUpLoad" 
            @scroll="contentScroll"
            @pullingUp="moreLoad">
```

#### 3、方法实现加载更多

```vue
 moreLoad(){
        // console.log('Loading.......');
        this.getHomeGoods(this.currentType)
        //上拉请求完之后要继续上拉的话要调用finishPullUp()，在getHomeGoods实现
      },

//修改后的getHomeGoods
 getHomeGoods(type){
        const page = this.goods[type].page + 1
        getHomeGoods(type,page).then((res)=>{
        // console.log(res);
        this.goods[type].list.push(...res.data.list)
        this.goods[type].page += 1

        //把finishPullUp的方法在scroll封装
        this.$$refs.scroll.finishPullUp()
        
      })
      }
```

#### 4、体现的是封装的思想，所以我们把方法在scroll里面实现封装

```vue
finishPullUp(){
            //每次上拉加载完后要调用这个方法，才能继续下一次上拉
            this.scroll.finishPullUp()
        }
```

### 21、解决首页Better-Scroll滚动区域的问题

#### 1、产生原因：

因为图片加载是异步的，vue刚开始会根据读取的内容获取高度，但是图片加载异步，就导致实际上获取的高度和实际上所需要的高度不匹配

#### 2、如何解决

vue使用refresh去解决这个问题

监听每一张图片的加载，加载完成就调用scroll的一个refresh，使其完成刷新加载，高度得到更新

##### 1、新的问题，GoodsListItem和Home相隔太远

但是这又有一个新的问题，GoodsListItem和Home相隔太远

![img](file:///C:\Users\Administrator\Documents\Tencent Files\1446070006\Image\C2C\E3CED5291A2BF5CD255B24ED1CC9BFBC.png)

##### 2、使用事件Bus总线去解决

GoodsListItem现在图片位置监听是否加载完成

```vue
<img src=""    @load="imageLoad"/>
```

GoodsListItem：在对应的方法实现

```vue
methods:{
	imageLoad(){
	this.$bus.$emit('itemImageLoad')	
	}
}
```

Home：在Home中，当组件创建的时候就要监听

```vue
created(){
this.$bus.$on('itemImageLoad',()=>{
	console.log('---')
})
}
```

要在main.js中注册bus

```js
Vue.prototype.$bus = new Vue()
```

Scroll：在Home监听中调用scroll组件的refresh，我们要先把refresh封装成方法，满足封装的思想

请求数据是在created里面请求的，scroll是在mouted里面产生的，所以此时可能存在一种情况报错，我们请求好数据发送过来的时候，scroll还没有创建完成，这个时候会报错，所以我们使用 this.scroll && this.scroll.refresh() ，这个时候就会先判断this.scroll是否有值，有值的话那么就会执行下一步语句

```vue
refresh(){
           this.scroll && this.scroll.refresh()
        }
```

Home：在Home中调用，要在mouted函数里面调用这个监听，因为在created里面调用这个监听的话，scroll也可能会不存在，所以要在mouted里面监听

```vue
mouted(){
this.$bus.$on('itemImageLoad',()=>{
	console.log('---')
	this.$scroll.scroll.refresh()
})
}
```

###### 总线的使用

```java
Vue.prototype.$bus = new Vue()
this.$bus.$emit('itemImageLoad')	
this.$bus.$on('itemImageLoad',()=>{
	console.log('---')
})
```

##### 3、refresh调用太频繁的问题

##### 4、使用防抖动函数debounce解决



```vue
methods:{
	 //防抖动函数
        debounce(func,delay){
            let timer = null
            return function(...args){
                if(timer) clearTimeout(timer)
                timer = setTimeout(()=>{
                    func.apply(this,args)
                },delay)
            }
        }
}
```

​	这里把所要执行的函数当作一个func传入到防抖动函数中，还有传入的时间delay，当我们第一次使用这个函数的时候，创建一个timer，设置定时器，第二次访问的时候，会判断是否存在定时器，如果存在，清空这个定时器，再新建一个，这样子就可以让使用频繁的refresh减少调用次数，例如，第一次调用防抖函数，防抖函数定时500毫秒，但是第二次调用函数只需要100毫秒，那么第一个执行到100毫秒的定时器就会被清除，再新建一个500毫秒的定时器，以此类推

##### 5、使用防抖动函数的时候建议包装起来

在common新建一个utils.js文件

```js
export function debounce(func,delay){
    let timer = null
    return function(...args){
        if(timer) clearTimeout(timer)
        timer = setTimeout(()=>{
            func.apply(...args)
        },delay)
    }
}
```

要使用的时候直接导入就可以使用

```vue
import {debounce} from '' 
```

### 22、上拉加载更多功能

#### 1、监听scroll滚动到底部

```vue
//3、监听滚动到底部
        if(this.scroll.pullUpLoad){
            //先判断是否存在pullUpLoad，存在的话再进行监听
        this.scroll.on('pullingUp',()=>{
            this.$emit('pullingUp')
            
        })
        }
```

### 23、tabControl实现吸顶效果，到一定距离停住

#### 1、获取tabControl的offsetTop

要在mouted里面完成，因为tabControl要先加载完成才能够获取（后面改在methods里面）

```vue
mounted(){
      //1、获取tabControl的offsetTop
      //所有的组件都有一个属性$el，用来获取组件中的元素
      console.log(this.$refs.tabControl.$el)
    }
```

#### 2、这个时候的offsetTop是不正确的

因为图片还未加载完成，没有把图片的高度计算进去，我们要监听图片的加载是否完成

HomeSwiper：用@load去监听，再methods中实现监听发出事件，为了改善发出事件的次数，设置一个发出事件状态，先设置为false，等发出事件之后就不会再次调用

```vue
swiperimageload(){
        if(!this.isLoad){
        this.$emit('swiperimageload')
        this.isLoad = true
        }
      }
```



Home中捕捉事件，这时候再去获取offsetTop的值才是正确的

Home.vue

```vue
swiperimageload(){
        // console.log('1111');
        //1、获取tabControl的offsetTop
      //所有的组件都有一个属性$el，用来获取组件中的元素
      // console.log(this.$refs.tabControl.$el.offsetTop)
      this.tabOffsetTop = this.$refs.tabControl.$el.offsetTop
      },
```

#### 3、实现TabControl

在nav-bar下面增加一个TabControl，设置好z-index和position，并且使用v-show隐藏起来

![image-20201203160500363](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201203160500363.png)

判断TabCiontrol的offsetTop是否和监听到的position.y达到某个条件，那么使用v-show显示出来

Home.vue

```vue
contentScroll(position){
        //1、判断是否显示返回顶部的按钮
        // console.log(position);
        if(-position.y > 1000){
          this.isShowBackTop = true
        }else if(-position.y <= 1000){
          this.isShowBackTop = false
        }
      // console.log(this.$refs.tabControl.$el.offsetTop);
        //2、判断是否显示TabControl
        this.isTabFixed = (-position.y) > this.$refs.tabControl.$el.offsetTop - 22
      },
```

两个tabContrl都实现点击保持一直，分别赋予ref，在监听点击事件中给this.$refs.tabControl.currentType赋值

```vue
tabClick(index){
        // console.log(index);
        switch(index){
          case 0:
            this.currentType = 'pop';
            break;
          case 1:
            this.currentType = 'new';
            break;
          case 2:
            this.currentType = 'sell'
            break;
        }
        this.$refs.tabControl.currentIndex = index
        this.$refs.tabControl1.currentIndex = index
      },
```

### 24、让Home保持原来的状态

#### 1、是Home不要被销毁

添加一个keep-alive

App.vue

```vue
<keep-alive>
      <router-view/>
</keep-alive>
```



#### 2、保持位置不变

出去的时候保持位置信息，回来的时候读取位置信息，在Vue里面有两个属性可以解决

Home.vue

```vue
 //进入
    activated(){
        // console.log('进入');
		//进去的时候读取保存的位置
        this.$refs.scroll.scrollTop(0,this.saveY,0)
        this.$refs.scroll.refresh()
    },
    //离开
    deactivated(){
        // console.log('离开');
        this.saveY = this.$refs.scroll.getScrollY()

    }
```

Scroll.vue:封装的方法

```vue
//返回y
        getScrollY(){
            return this.scrollY ? this.scroll.y : 0 
        }
```

### 25、跳转到详情页并且携带id

我们这里没有数据，所以换成点击Recommend的图片

#### 1、给图片加上监听事件itemClick

GoodsListItem（Recommend）

```vue
<img src="" @click="itemClick"/>
```

#### 2、新建details的组件，并且添加路由

路由

```js
{
    path: '/detail',
    component: Detail
  },
```

Home:路由跳转

```vue
this.$router.push({
          path:'/detail',
          query:{
            iid:111
          }
        })
```

Detail： 接收数据

```vue
 this.iid = this.$route.query.iid
```

#### 3、创建导航栏

##### 1、导航栏实现

使用封装的导航栏，直接在插槽插入数据，实现for循环获取数据，通过css样式修改排列，通过index的下标决定点击哪一个按钮

DetailItem.vue

```vue
<div slot="center" class="nav-bar">
              <div v-for="(item,index) in titles" 
                    :key="item" 
                    class="nav-bar-item"
                    @click="titleClick(index)"
                    :class="{active: currentIndex == index}">
                      {{item}}
            </div>
          </div>
```

```vue
<style scoped>
    .nav-bar{
        display: flex;
        font-size: 12px;
    }
    .nav-bar-item{
        flex: 1;
    }
    .active{
        color: red;
    }
</style>
```

##### 2、导航栏返回按钮实现

在左边插槽插入一个图片，给图片设置点击事件即可

##### 3、请求数据

要封装网络请求

network/detail.js

```js
import {request} from './request'
export function getDetail(iid){
    return request({
        url: '/detail',
        params:{
            iid
        }
    })
}
```

使用这个网络请求

Detail.vue

```vue
import 
在created中使用这个网络请求
 getDetail(this.iid).then((res)=>{
            console.log(' i am the Detail')
            console.log(res);
        })
```

使用一个数组去接受数据，在created的函数里面去调用数据

Detail.vue

```vue
data(){
	topImages:[]
        },

created(){
        //1、获取iid
        this.iid = this.$route.query.iid
        
        // console.log(this.iid);
        //2、请求iid的数据
        getDetail(this.iid).then((res)=>{
            // console.log(' i am the Detail')
            // console.log(res);
            //获取顶部轮播图的图片
            this.topImages = res.result.itemInfo.topImages
        })
    }
}
```

在DetailItem中获取从Detail传来的数据，并且渲染出来

DetailItem.vue

```vue
<template>
  <div class="detailswiper">
    <!-- {{topImages}} -->
    
    <swiper class="detailswiper1">
        <swiper-item v-for="item in topImages" :key="item">
            <img :src="item"/>
        </swiper-item>
    </swiper>
    </div>

</template>

<script>

import {Swiper,SwiperItem} from 'components/common/swiper'

export default {
    name:'detailswiper',
    components:{
        Swiper,
        SwiperItem
    },
    props:{
        topImages:{
            type:Array,
            default(){
                return[]
            }
        }
    }
}
</script>

<style scoped>
.detailswiper1{
    height: 250px;
    overflow: hidden;
}
</style>>
```

![image-20201204093603182](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201204093603182.png)

这个时候点击跳转到详情界面是无法变动数据的，因为我们之前使用了keep-alive的空间，我们要排除Detai这个空间

```vue
<keep-alive exclude="Detail">
      <router-view/>
    </keep-alive>
```

### 26、商家信息展示

#### 1、封装一个network/detail.js接受数据

```js
export class Shop{
    constructor(shopInfo){
        this.logo = shopInfo.shopLogo;
        this.name = shopInfo.name;
        this.fans = shopInfo.cFans;
        this.sells = shopInfo.cSell;
        this.score = shopInfo.score;
        this.goodsCount = shopInfo.cGoods;
    }
}
```

#### 2、在Detail.vue导入(import)Shop，创建一个对象去存储它

```vue
created(){
        //1、获取iid
        this.iid = this.$route.query.iid
        // console.log(this.iid);
        //2、请求iid的数据
        getDetail(this.iid).then((res)=>{
            // console.log(' i am the Detail')
            // console.log(res);
            //1、获取顶部轮播图的图片
            const data = res.result
            this.topImages = data.itemInfo.topImages

            //3、创建店铺信息的对象
            this.shop = new Shop(data.shopInfo)
        })
    }
```



# ##################2020.12.03####################

#### 3、店铺信息的解析和展示

在Detail.vue获取到店铺的数据之后，把数据传送给DetailShopInfo.vue里面，再让DetailShopInfo.vue去渲染

#### 4、商品信息的基本展示

基本思路

因为从服务器传来的数据是杂乱无章的，所以我们需要对数据进行包装

##### 1、在network/detail.js里面封装一个Goods

##### 2、在Detail.vue里面引用这个Goods，在data()中创建一个变量把数据保存

##### 3、传给要展示的页面

##### 4、加入滚动的Better-Scroll

引入Better-Scroll

Detail.vue

```vue
//公共组件
import Scroll from 'components/common/scroll/Scroll'

在components中注册使用
在template中使用即可
要注意样式
<style scoped>
    .detail{
        position: relative;
        height: 100vh;
    }  
    .detail-nav-bar{
        position: relative;
        background-color: white;
        z-index: 9;
    } 
    .swiper{
        position: relative;
        height: calc(100% - 44px);
        z-index: 8;
        background-color: white;
    }
</style>
```

##### 5、详情页-商品数据展示

### 27、首页跳转到详情页

#### 1、问题

​	在详情页里面推荐商品的图片加载出来后会调用@load加载监听，这个监听会调用事件总线，我们在这里实际上是不需要监听事件总线的，我们可以采取解决办法,

在详情页里面和Home里面我们各自需要监听事件，我们可以采取判断路由的不同路径去决定去调用事件总线发出不同的事件

```vue
//判断路由是否有/home来决定是否调用事件总线
if(this.$route.path.indexOf('/home')){
	this.$bus.$emit('homeItemImageLoad');
}else if(this.$route.path.indexOf('/detail')){
	this.$bus.$emit('detailItemImageLoad')
}
```

​	第二种方法：可以采取当离开Home界面的时候取消掉事件总线的监听



### 28、混入(mixin)

实现在Vue生命周期中的一些代码复用

在common中建立一个mixin.js

```js
export const itemListenerMixin = {
    mounted(){
        console.log("我是混入的内容");
    }
}
```

在Home.vue中引用它

```vue
<script>
import {itemListenerMixin} from 'common/mixin'
export default {	
 ...
    mixins:[itemListenerMixin],
 ... 
}
</script>
```

![image-20201204110342809](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201204110342809.png)

### 29、点击标题跳到对应的内容

#### 1、联动效果

##### 1、点击标题滚动到对应位置

​	使用this.$emit把标题点击的下标传出给Detail.vue，在Detail中创建一个对象themeTopYs，来存储详情页对应标题的标签的offsetTop，要根据详情页标题的顺序去存储值，然后每次点击的时候，就使用this.$refs.scroll.scrollTo()来滑动scroll里面的内容

​	要在created的属性里面调用this.$nextTick(()=>{})函数，这个函数会在vue的元素请求完数据加载完成之后回调一次，那么就可以在这去获取各个组件的offsetTop了。如果直接获取各个组件的offsetTop的话可能会出现组件还没有加载完成，导致空值的现象。

​	但是在这里调用的话我们的图片依旧是没有加载出来的，依旧会出现错误。一般来说offsetTop不对的时候，都是因为图片的问题

​	mounted属性的话是在组件加载完成之后调用的，并不一定是网络数据请求之后调用的，所以就算在mounted里面获取各个组件的offsetTop信息的话也是可能存储在错误的

```vue
methods:{
        //详情页点击主题滑动到对应的位置
        titleClick(index){
            //测试成功
            // console.log(index);
			//在这里按照数组里面的y值去滑倒到指定的位置。
            this.$refs.swiper.scrollTo(0,-this.themeTopYs[index],300)

        }
    },

this.$nextTick(()=>{
             //获取各个标题的offsetTop，把数据传入到数组当中
        this.themeTopYs = []
        this.themeTopYs.push(0)
        this.themeTopYs.push(this.$refs.params.offsetTop)
        this.themeTopYs.push(this.$refs.comment.offsetTop)
        this.themeTopYs.push(this.$refs.recommend.offsetTop)
        })
```

![image-20201204123436771](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201204123436771.png)

在这里获取的offsetTop的值是不对的，因为我们的图片还没有加载完成

关于图片的问题我们需要监听一下函数是否加载完成

```vue
<img src="" @load="imageLoad()"/>
methods:{
	imageLoad(){
	//在图片加载监听函数里面获取offsetTop
	//但是在这里获取offsetTop的话会执行多次，所以我们要采用防抖函数
	//在这里调用获取offsetTop的函数
	this.themeOffsetTop()
}
}
```

防抖函数的使用

```vue
//先创建一个themeOffsetTop对象去存储这个防抖函数
created(){
	//4、创建对象存储防抖函数
            //利用防抖函数获取offsetTop
            this.themeOffsetTop = debounce(()=>{
                this.themeTopYs = []
                this.themeTopYs.push(0)
                this.themeTopYs.push(this.$refs.params.offsetTop)
                this.themeTopYs.push(this.$refs.comment.offsetTop)
                this.themeTopYs.push(this.$refs.recommend.offsetTop)
            })
}
```

![image-20201204145310488](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201204145310488.png)

##### 2、滚动内容显示相应标题

要先监听滚动的位置，我们设置的滚动监听默认是0，要实现监听就得传入2或者3 **:probe-type="3"**

```vue
 <scroll class="swiper" 
                ref="swiper"
                :probe-type="3"
                @scroll="contentScroll">
```

在methods中，我们判断y的滑动位置，如果在

![image-20201204152245846](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201204152245846.png)

对应的范围之内，就把detailnavbar里面的currentIndex替换成对应的i，封装一个方法在DetailNav里面用来设置index的值

```vue
methods:{
contentScroll(position){
            // console.log(position)
            //获取y值
            const y = -position.y
            //根据范围内容去判断
            // console.log(this.themeTopYs);
            const length = this.themeTopYs.length
            // console.log(length);
            for(let i = 0 ; i < length ; i++){
                if(
                    ( i < length - 1 && this.themeTopYs[i] < y && this.themeTopYs[i+1] > y)
                ||
                ( i === length-1 && this.themeTopYs[i] < y))
                {
                    // console.log(y);
                    this.$refs.detailnavbar.setDetailNavIndex(i)
                }   
            }
        }
```

##### 3、对复杂判断条件的分析和优化

往this.themeTopYs的尾部多加一个值，这个值为数字的最大值         

​       this.themeTopYs.push(Number.MAX_VALUE)

```vue
//4、创建对象存储防抖函数
            //利用防抖函数获取offsetTop
            this.themeOffsetTop = debounce(()=>{
                console.log('防抖函数使用第二次');
                this.themeTopYs = []
                this.themeTopYs.push(0)
                this.themeTopYs.push(this.$refs.params.offsetTop)
                this.themeTopYs.push(this.$refs.comment.offsetTop)
                this.themeTopYs.push(this.$refs.recommend.offsetTop)
                this.themeTopYs.push(Number.MAX_VALUE)
            },50)
```

优化后的循环

由于最后一个数据是我们加入的，所以我们不需要去遍历最后一层数据，所以就只需要满足这个条件就可以

```vue
for(let i = 0 ; i < length -1 ; i++){
                if( y > this.themeTopYs[i] && y < this.themeTopYs[i+1]){
                    this.$refs.detailnavbar.setDetailNavIndex(i)
                }
            }
```



#### 2、底部工具栏，点击加入购物车

##### 1、封装一个组件导入到Detail.vue中就可以

##### 2、加入购物车，放到store里面

子组件点击响应，Detail中获取响应事件

```vue
//检测添加到购物车响应事件
        addToCar(){
            // console.log('检测添加到购物车响应事件');
            //1、在这里获取商品信息，只获取购物车需要的信息
            //假数据
            const product = {}
            product.image = this.topImages[0];
            product.title = 'clothes'
            product.desc = 'girl clothes'
            product.price = '78';
            product.iid = '111';
        }
```

##### 3、引入Vuex

下载Vuex

```java
npm install vuex --save
```

创建store/index.js

```js
import Vue from 'vue'
import Vuex, { Store } from 'vuex'

//1、安装插件
Vue.use(Vuex)

//2、创建Store对象
const store = new Store({
    state:{

    },
    mutations:{

    }
})

//3、Vue实例上
export default store
```

在main.js里面注册store

```js
import Vue from 'vue'
import App from './App.vue'
import router from "./router";
import store from './store'

Vue.config.productionTip = false

Vue.prototype.$bus = new Vue()

new Vue({
  render: function (h) { return h(App) },
  router,
  store
}).$mount('#app')

```

##### 4、代码修改优化

我们对vuex的数据进行操作的时候一般要通过mutations来操作，mutations是用来定义方法的

index.js我们现在mutations里面定义一个添加购物车的方法，和添加一个变量来存储商品

```js
import Vue from 'vue'
import Vuex, { Store } from 'vuex'

//1、安装插件
Vue.use(Vuex)

//2、创建Store对象
const store = new Store({
    state:{
        carList:[]
    },
    mutations:{
        addCarList(state,payload){
            state.carList.push(payload)
        }
    }
})

//3、Vue实例上
export default store
```

在Detail.vue中把商品添加到购物车当中，要使用commit的方法来使用

```vue
//检测添加到购物车响应事件
        addToCar(){
            // console.log('检测添加到购物车响应事件');
            //1、在这里获取商品信息，只获取购物车需要的信息
            //假数据
            const product = {}
            product.image = this.topImages[0];
            product.title = 'clothes'
            product.desc = 'girl clothes'
            product.price = '78';
            product.iid = '111';
            //2、将商品添加到购物车，依赖Vuex，需要使用commit
            this.$store.commit('addCarList',product)
        }
```

判断购物车是否已经有相同的物件

```js
mutations:{
        //判断是否是购物车有的商品，
        addCarList(state,payload){
            // let oldProduct = null
            // for(let item of state.carList){
            //     if(item.iid === payload.iid){
            //         oldProduct = item
            //     }
            // }
            //1、查找之前的数组是否有该商品，用find方法从carList中循环出一个item，判断item.iid是否等于payload.iid如果相等返回为真，即oldProdect为true
            
            let oldProduct = context.state.carList.find(item => item.iid === payload.iid)

            if(oldProduct){
                //现存商品数量加一
                oldProduct.count += 1
            }else{
                //新商品
                payload.count += 1
                state.carList.push(payload)
            }

            
        }
    }
```

##### 5、对Vuex的代码重构

一般来说我们的mutations里面只进行单一的操作，而不是复杂的操作。在上面的代码中我们的mutaions中的方法有多步的操作，所有我们的代码要进行重构

actions中不仅可以放异步操作的代码，也可以放调用mutaions的代码

store/index.js

```js
//一般来说mutations的代码只做单一的操作
    mutations:{
        //相同的商品数量加一
        addCounter(state,payload){
            payload.count++
        },
        //添加新商品进购物车
        addToCar(state,payload){
            state.carList.push(payload)
        }
    },
    actions:{
        //判断是否是购物车有的商品，
        addCarList(context,payload){

            //1、查找之前的数组是否有该商品，用find方法从carList中循环出一个item，判断item.iid是否等于payload.iid如果相等返回为真，即oldProdect为true
            let oldProduct = context.state.carList.find((item) => {
                item.iid === payload.iid
            })

            //2、判断oldProdect
            if(oldProduct){
                //现存商品数量加一
                context.commit('addCounter',oldProduct)
                
            }else{
                //新商品
                context.commit('addToCar',payload)
            }
        }
    },
```

在index.js代码中，代码过于臃肿，我们对代码进行一个抽取，在相同的目录下创建mutations.js和actions.js，而且我们要把mutations中的方法名称定义为一个常量，新建成mutaions-type.js，方便用来调用mutaions方法时的使用

mutations.js

```js

import {ADD_COUNTER,ADD_TO_CART} from './mutaions-type'

export default{
    //相同的商品数量加一
    [ADD_COUNTER](state,payload){
        payload.count++
    },
    //添加新商品进购物车
    [ADD_TO_CART](state,payload){
        state.carList.push(payload)
    }
}
```

actions.js

```js
import {ADD_COUNTER,ADD_TO_CART} from './mutaions-type'

export default{
    //判断是否是购物车有的商品，
    addCarList(context,payload){

        //1、查找之前的数组是否有该商品，用find方法从carList中循环出一个item，判断item.iid是否等于payload.iid如果相等返回为真，即oldProdect为true
        let oldProduct = context.state.carList.find((item) => {
            item.iid === payload.iid
        })

        //2、判断oldProdect
        if(oldProduct){
            //现存商品数量加一
            context.commit(ADD_COUNTER,oldProduct)
            
        }else{
            //新商品
            context.commit(ADD_TO_CART,payload)
        }

        
    }
}
```

mutations-type.js，官方推荐使用这样子的方法

```js
export const ADD_COUNTER = 'add_counter'
export const ADD_TO_CART = 'add_to_cart'
```

在store/index.js中使用import导入

```js
import mutations from './mutations'
import actions from './actions'
```





#### 3、回到顶部

##### 实现Detail的backTop，返回顶部

我们把需要公共用到的代码抽取出来放到mixin中，混入封装

```js
import BackTop from 'components/content/backTop/BackTop.vue';

export const itemListenerMixin = {
    mounted(){
        // console.log("我是混入的内容");
    }
}

export const backTopMixin = {
    components: {
        BackTop,
    },
    data(){
        return {
            isShowBackTop: false,
        }   
    },
    methods:{
        backTop(){
            this.$refs.swiper.scrollTo(0,0,300)
        }
    }
}

```

在Detail.vue中引入这个mixin，在template使用**<back-top v-show="isShowBackTop" @click.native="backTop"/>**

```vue
import {itemListenerMixin,backTopMixin} from 'common/mixin'
export default  {
	mixin:[itemListenerMixin,backTopMixin]
}
```

# ###################2020.12.04####################

### 30、购物车-导航栏的实现

#### 1、引入组件NavBar就可以

#### 2、关于购物车顶部数量的问题

##### 1、利用计算属性去获取vuex中所有商品的数量

```vue
computed:{
      total(){
        let total = 0
        let products = null
        products = this.$store.state.carList
        if(products){
          for(let item of products){
            total += item.count
          }
          return total
        }
      }
    },
```

##### 2、可以把vuex中的getters映射到局部的计算属性

写好getters

```js
export default{
    carListLength(state){
        let total = 0
        let products = null
        products = state.carList
        if(products){
          for(let item of products){
            total += item.count
          }
        }
        console.log(total);
        return total
    }
}
```

在Cart.vue引入mapGetters，要记住是从**vuex**中引入的，在computed中引入getters中的属性

```vue
{{length}}
import {mapGetters} from 'vuex'
computed:{
      //把getters中映射到局部计算属性有两种方法。1、数组语法，2、对象语法
      // ...mapGetters(['carListLength'])
      ...mapGetters({
        length: 'carListLength'
      })
    },
```

### 31、购物车商品的展示

#### 1、新建一个CartList组件来当作展示商品的父组件

使用scroll的注意点，要注意父组件要确定好高度，然后里面的content可以滑动，浏览器有的时候滑动是有问题的。

浏览器没办法滚动的原因是刚加载进去的时候我们的scroll高度为0，然后添加数据进去，vue默认高度还是为0，我们应该在进入Cart.vue界面的时候对scroll进行一次刷新，在CartList.vue界面刷新

```vue
activated(){
        //由于刚加载进去cart页面scroll默认高度为0，所以我们每次进入的时候刷新一下scroll就可以获取到准确高度了
        this.$refs.scroll.refresh()
    }
```



#### 2、新建CartListItem来展示每个小商品

在cartlistitem里面只需要把每一个商品的样式弄好即可

只需要把每个小商品的数据展示出来就可以

关于checkButton的使用，在components/content/里面新建一个组件，确定好背景图片

```vue
<template>
  <div class="check-button">
      <img src="~assets/img/cart/dagou.svg"/>
  </div>
</template>

<script>
export default {
    name:'checkbutton'
}
</script>

<style scoped>
    .check-button img{
        width: 20px;
        height: 16px;
        z-index: 2;
        border-radius: 10px;
    }
</style>
```

然后在使用的组件里面调用就可以，关于颜色展示使用 :class="{ischeck:isCheck}" 就可以，绑定响应事件

# ###################202.12.06#####################

#### 3、item选中和不选中的切换

这个状态应该在模型里面决定，在mutations.js里面给我们的模型新添加一个isCheck的属性值，默认为true

```js

import {ADD_COUNTER,ADD_TO_CART} from './mutaions-type'

export default{
    //相同的商品数量加一
    [ADD_COUNTER](state,payload){
        payload.count++
    },
    //添加新商品进购物车
    [ADD_TO_CART](state,payload){
        //这是新添加的属性值
        payload.isCheck = true
        state.carList.push(payload)
    }
}
```

在CartListItem.vue里面的checkbButton的显示应该由product.isCheck决定

```vue
<check-button class="check-button" @click.native="cartListItemClick" :class="{ischeck:product.isCheck}"/>

methods:{
cartListItemClick(){
            
            this.product.isCheck = !this.product.isCheck
            console.log('cartListItemClick');
        }
}
```

### 32、底部工具的封装和应用

#### 1、在childComps新建一个CartBottomBar.vue

里面设置好组件的高度，添加到Cart.vue里面，得在scroll的高度里面减去这个高度，然这个组件独立出来，否则会覆盖住scroll里面的东西

在CartBottomBar里面增加checkButton的选择框

#### 2、计算总价格

结果保留两位小数，在计算属性里面添加一个totalPrice，计算store里面所有被选中的商品价格

```vue
computed:{
       totalPrice(){
            return "￥" + this.$store.state.carList.filter(item => {
                return item.isCheck
            }).reduce((preValue,item) =>{
                return preValue + item.count * item.price
            },0).toFixed(2)
    }
```

#### 3、去计算按钮

计算属性获取总数，关于布局我们可以分别给checkButton和total一个宽度，使用display:flex，让中间的flex：1，可以实现布局

```vue
total(){
            return this.$store.state.carList.filter(item => {
                return item.isCheck
            }).reduce((preValue,item)=>{
                return preValue + item.count
            },0)
        }
```

![image-20201207102727785](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201207102727785.png)

#### 4、全选按钮

```vue
<check-button class="check-button1" :class="{checkbutton11: selectAll}"></check-button>
```

计算属性computed

```vue
selectAll(){
            //判断是否有没有被选中，如果存在没有选中的，那么就返回false
            //长度为0代表全选，返回真
            // return this.$store.state.carList.filter((item)=>{
            //     //如果有没有被选中的，就返回
            //     return !item.isCheck
            // }).length == 0
			//如果存在没选中的和列表为空返回false，
            return (this.$store.state.carList.length != 0 && !this.$store.state.carList.find(item => !item.isCheck))
        }
```

完成全选点击按钮

```vue
methods:{
	checkClick(){
            // console.log('checkItemClick');
            //如果已被选中，那么就所有的取消，否则反之
            //取消商品所有按钮
            if(this.selectAll){
                // for(let i = 0 ; i < this.$store.state.carList.length ; i++){
                //     // console.log(this.$store.state.carList[i]);
                //     this.$store.state.carList[i].isCheck = false
                // }
                // for(let item of this.$store.state.carList){
                //     item.isCheck = false
                // }
                this.$store.state.carList.forEach(element => {
                    element.isCheck = false
                });
            }else{
                // for(let i = 0 ; i < this.$store.state.carList.length ; i++){
                //     // console.log(this.$store.state.carList[i]);
                //     this.$store.state.carList[i].isCheck = true
                // }
                // for(let item of this.$store.state.carList){
                //     item.isCheck = true
                // }
                this.$store.state.carList.forEach(element => {
                    element.isCheck = true
                });
            }
        }
}
```

### 33、关于VuexPromise和mapActions的补充

#### 1、从vuex返回promise来确定是否添加购物车成功

修改store/getters.js代码

```js
import {ADD_COUNTER,ADD_TO_CART} from './mutaions-type'

export default{
    //判断是否是购物车有的商品，
    addCarList(context,payload){
        return new Promise((resolve,reject)=>{
            //1、查找之前的数组是否有该商品，用find方法从carList中循环出一个item，判断item.iid是否等于payload.iid如果相等返回为真，即oldProdect为true
            let oldProduct = context.state.carList.find(item => item.iid === payload.iid)

            // console.log(oldProduct);

            //2、判断oldProdect
            // if(oldProduct){
            //     //现存商品数量加一
            //     console.log('oldProduct');
            //     context.commit(ADD_COUNTER,oldProduct)
                
            // }else{
            //     //新商品
            //     console.log('newProduct');
            //     context.commit(ADD_TO_CART,payload)
            // }


            //为了测试购物车多条数据，改了这的添加
            context.commit(ADD_TO_CART,payload)
            resolve('添加商品成功')
        })
    }
}
```

在detail.vue就可以使用Promise的then了

```vue
//2、将商品添加到购物车，依赖Vuex
            this.$store.dispatch('addCarList',product).then((res)=>{
                console.log(res);
            })
```

#### 2、mapActions的使用

和mapGetters的用法差不多一样，先导入，后methods中注册，是vuex中actions和methods的映射

```vue
import {mapActions} from 'vuex'

methods:{
	//数组语法
	//...mapActions(['addCart'])
	//对象语法
	...mapActions{
	add: 'addCart'
	}
}
```

### 34、Toast的封装

#### 1、普通的封装

自己封装一个组件使用，不建议，太麻烦

#### 2、插件方式的封装

##### 1、先创建好一个组件，在components/common

```vue
<template>
  <div class="toast" v-show="isShow" >
      <div>{{message}}</div>
  </div>
</template>

<script>
export default {
    name:'toast',
    props:{

    },
    data(){
        return{
            message:'',
            isShow:false,
        }
    },
    methods:{
        show(message='默认为自',duration=2000){
            console.log('我是Toast的方法');
            this.message = message
            this.isShow = true
            setTimeout(() => {
                console.log('我是timeout');
                this.message = ''
                this.isShow = false
            }, duration);
        },
      
    }
}
</script>

<style scoped>
.toast{
    position: fixed;
    z-index: 999;
    top: 50%;
    left: 50%;
    background-color:rgba(0, 0, 0, 0.75);
    transform: translate(-50%,-50%);
    padding: 8px 10px;
    border-radius: 5px;
    color: white;
}
</style>
```

##### 2、在文件目录下创建一个index.js用来把这个组件当作插件挂载进去

```js
import Toast from './Toast'

const obj = {}
//obj在加载的时候会默认传过来一个Vue
obj.install = function(Vue){

    //1、创建组件构造器
    const toastContrustor = Vue.extend(Toast)

    //2、new的方式，根据组件构造器，可以创建出来一个组件对象
    const toast = new toastContrustor()

    //3、将组件对象，手动挂载到某一个元素上
    toast.$mount(document.createElement('div'))

    //4、toast.$el对应的就是div
    document.body.appendChild(toast.$el)

    Vue.prototype.$toast = toast;
}

export default obj
```

##### 3、在main.js中去把这个插件导入进去

```js
import toast from 'components/common/toast'
//安装toast插件
Vue.use(toast)
```

##### 4、在要使用的地方

```vue
this.$toast.show(message,duration) //message和duration可传可不传
```

### 35、解决移动端的300ms

fastclick

#### 1、下载

```java
npm install fastclick --save
```

#### 2、引入，使用

main.js，在手机上测试过，效果明显

```js
import FastClick from 'fastclick'

//使用fastclick解决移动端300ms延迟
FastClick.attach(document.body)
```

### 36、图片懒加载

什么是图片懒加载：图片需要展示的时候再加载

#### 1、下载

```java
npm install vue-lazyload --save
```

#### 2、引入、使用

```js
import LazyLoad from 'vue-lazyload'

//使用懒加载插件
Vue.use(LazyLoad)
```

在需要用懒加载的地方

```vue
<img :src="item" @load="imgLoad"/>
改成
<img v-lazy="item" @load="imgLoad"/>
```

使用占位图片，图片还没有加载出来的时候使用

main.js

```vue
//使用懒加载插件
Vue.use(LazyLoad,{
	//占位图片的位置
  loading: require('./assets/img/common/placceholer.png')
})
```

### 37、px2vw-css单位转换插件

#### 1、安装

```java
npm install postcss-px-viewport --save-dev
```

#### 2、在最外面的目录下新创建postcss.config.js

```js
module.exports = {
    plugins: {
        autoprefixer: {},
        "postcss-px-to-viewport":{
            //设计稿一般来说使用iphone6的
            viewportWidth: 375, //视窗的高度，对应的是我们设计稿的高度
            viewportHeight: 667,  //视窗的宽度，对应的是我们设计稿的宽度
            unitPrecision: 5,   //指定'px' 转换为视窗单位值的最小位数（很多时候无法整除）
            viewportUnit: 'vw',  //指定需要转换成的视窗单位，建议使用vw
            selectorBlackList: ['ignore','tab-bar','tab-bat-item'],//指定不需要被转换的类
            minPixelValue: 1, //小于或者等于1px的不转换为视窗单位
            mediaQuery: false, //允许媒体查询中转换 'px',
            exclude: [/TabBar/] //匹配到有关于xxxTabBarxx的组件都不需要转换，必须正则匹配
        },
    }
}
```

#### 3、效果，会等比缩放

![image-20201207165506726](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201207165506726.png)

# ###################2020.12.07####################



### 38、项目部署

#### 1、nginx项目在window上的部署

##### 1、项目打包

会生成一个dist的包

```java
npm run build
```

##### 2、安装nginx

下载nginx，在window上解压出来，直接就可以使用，访问

```java
localhost
```

可以看到nginx的页面

##### 3、配置nginx

###### 1、把dist文件夹下面的东西复制到nginx的html下面，重新访问localhost就可以了

![image-20201208092200749](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201208092200749.png)

###### 2、修改配置文件

在conf/nginx.conf修改，修改后要重启

```java
nginx -s reload
```



root代表的是访问的目录，index代表是访问的主页

![image-20201208093001704](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201208093001704.png)

![image-20201208092913351](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201208092913351.png)



#### 2、在Linux下面部署

略

### 39、Vue的响应式原理

![image-20201208101824829](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201208101824829.png)





#### 1、更新的原理

Vue对每一个值进行监听，如果发生了改变，就通知每一个用过值的对象，之间采取的是发布订阅者模式。

![image-20201208101837850](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201208101837850.png)

