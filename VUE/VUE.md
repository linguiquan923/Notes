# VUE

Soc

HTML + CSS + JS ;



#### 1、下载插件

#### 2、在script导入vue

```
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
```

#### 3、使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<div id="app">
    {{message}}
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            message:"hello vue"
        }
    })
</script>
</body>
</html>
```



## vue的语法

#### if

```html
<div id="app">
    <h1 v-if="type === 'A'">A</h1>
    <h1 v-else-if="type === 'B'">B</h1>
    <h1 v-else>Other</h1>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            type: 'A'
        }
    })
</script>
```

#### for

```html
<div id="app">
   <li v-for="item in items">
       {{item.message}}
   </li>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            items: [
                {message: "你好啊"},
                {message: "我很好"}
            ]
        }
    })
</script>
```

#### 事件绑定

```html
<div id="app">
  <button v-on:click="sayHi">click me</button>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            message: "你好啊"
        },
        methods:{//方法必须绑定在Vue的Methods的对象当中
            sayHi: function (event) {
                alert(this.message)
            }
        }
    })
</script>
```

#### 视图双向绑定

```html
<div id="app">
    请输入文本<input type="text" v-model="message">{{message}}

    单选框：<input type="radio" name="sex" value="男" v-model="sex"/>男
    单选框：<input type="radio" name="sex" value="女" v-model="sex"/>女
    <p>
        您选中了{{sex}}
    </p>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            sex: "",
            message: "123"
        }
    })
</script>
```



#### 下拉框

```html
<div id="app">
   <select v-model="message">
       <option value="" disabled>--请选择--</option>
       <option>A</option>
       <option>B</option>
       <option>C</option>
       <option>D</option>
   </select>
    <p>您选中了： {{message}}</p>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    var vm = new Vue({
        el:"#app",
        data:{
            message: ""
        }
    })
</script>
```

#### 组件

```
<div id="app">
<!--    v-bind="item"传递到下面的props-->
  <qinjiang v-for="item in items" v-bind:item="item"></qinjiang>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script>
    Vue.component("qinjiang",{
    	<!-- props可以从上面绑定的传值-->
        props: ['item'],
        template: ' <li>{{item}}</li>'
    })
    var vm = new Vue({
        el:"#app",
        data:{
            items: ["java","Linux","前端"]
        }
    })
</script>
```

## Axios异步通信

#### 1、引入js

```html

<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

#### 2、使用

```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--解决闪烁问题-->
    <style>
        [v-clock]{
            display:none;
        }
    </style>
</head>
<body>
<div id="vue" v-clock>
    <div>{{info.address}}</div>
    <!--v-bind 是绑定的意思，将这个绑定在uve，就可以访问uve的数据了-->
    <a v-bind:href="info.url">访问</a>
</div>


<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script>
    var vm = new Vue({
        el:"#vue",
        data:{

        },
        //data:属性
        data(){
          return{
              //请求的返回参数合适，必须和json格式一样
              info:{
                  name:null,
                  address:{
                      street:null,
                      city:null,
                      country:null
                  },
                  url:null,
              }
          }
        },
        mounted(){//钩子函数，从json文件里面取值
            axios.get('../data.json').then(response=>(this.info=response.data));
        }
    })
</script>
</body>
</html>
```

## 计算属性

```js
         methods: {
                    currentTime1:function () {
                        return Date.now();

                    }
                },
        computed:{//计算属性
            currentTime2:function () {
                return Date.now();

            }
        }
```

计算属性方法不能和methods重名，重名的话只会调用methods里面的方法。computed里面的方法只是一个属性。是通过内存调用的，可以想象成一个缓存

## 插槽的使用

1、首先我们应该在一个组件里面定义插槽，容纳后分别定义插槽的内容，并且给插槽起名字，在模板里面加入插槽的名字。

```js
<script>
    Vue.component("todo",{
        template:
            '<div>' +
                '<slot name="todo-title"></slot>' +
                '<ul>' +
                    '<slot name="todo-items"></slot>' +
                '</ul>' +
            '</div>'
    });

    Vue.component("todo-title",{
        props:['title'],
        template:
        '<div>{{title}}</div>'
    });
    Vue.component('todo-items',{
        props:['item1'],
        template:
        '<li>{{item1}}</li>'
    });

    var vm = new Vue({
        el:"#vue",
        data:{
            title:'中国的省份',
            items:["福建","安徽","湖南"]
        },

    })
</script>
```

```html
在html页面中，我们应该把插槽放进去，并且绑定数据和插槽名称，
:item1="item"是v-bind:item1="item"的缩写，其中item1是传到插槽的数据，而item是绑定的数据
```

```
<todo>
    <todo-title slot="todo-title" :title="title"></todo-title>
    <todo-items slot="todo-items" v-for="item in items" :item1="item"></todo-items>
</todo>
```

## 自定义事件

1. 首先在slot插槽里面定义好所需要的组件
2. 在组件里面定义好函数，通过v-on:或者@click去绑定好组件里面的方法，组件里面的不能直接调用vue的事件
3. 在Vue里面定义好函数，通过:remove1="removeItem(index)"去帮方法绑定到remove身上
4. 在组件当中，要获取传递下来的值index，通过props获取，还有在组件里面的方法添加

```html
	this.@emit("绑定的方法名字",传入的值)
```

​	这样子就可以使用自定义的事件



```html
<!DOCTYPE html>
<html lang="en" xmlns:v-bind="http://www.w3.org/1999/xhtml">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<div id="vue" >
    <todo>
        <todo-title slot="todo-title" :title="title"></todo-title>
        <todo-items slot="todo-items" v-for="(item,index) in items" :item1="item" :index="index" v-on:remove="removeItem(index)"></todo-items>
    </todo>
</div>


<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>

<script>
    Vue.component("todo",{
        template:
            '<div>' +
                '<slot name="todo-title"></slot>' +
                '<ul>' +
                    '<slot name="todo-items"></slot>' +
                '</ul>' +
            '</div>'
    });

    Vue.component("todo-title",{
        props:['title'],
        template:
        '<div>{{title}}</div>'
    });
    Vue.component('todo-items',{
        props:['item1','index'],
        template:
        '<li>{{item1}}<button @click="remove">删除</button></li>',
        methods:{
            remove:function (index) {
                this.$emit("remove",index)
            }
        }
    });

    var vm = new Vue({
        el:"#vue",
        data:{
            title:'中国的省份',
            items:["福建","安徽","湖南"]
        },
        methods:{
            removeItem:function (index) {
                /*splice函数从输入输入的index坐标开始删除，删除index后面跟着的位数*/
                console.log("省份" + this.items[index] + "删除成功")
                this.items.splice(index,1)

            }
        }
    })
</script>
</body>
</html>
```

