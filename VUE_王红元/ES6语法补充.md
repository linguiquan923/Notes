### ESl语法：let和var的区别

let在if和for中有自己的作用域，而var没有

```html
const button = document.getElementsByTagName('button')
  for(var i = 0 ; i < button.length ; i++){
    button[i].addEventListener('click',function () {
      console.log('第 ' + i + '个按钮')
    })
  }
```

![image-20201116144028112](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201116144028112.png)

```html
 const button = document.getElementsByTagName('button')
  for(let i = 0 ; i < button.length ; i++){
    button[i].addEventListener('click',function () {
      console.log('第 ' + i + '个按钮')
    })
  }
```

![image-20201116144050909](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201116144050909.png)

### ES6语法：const的使用和注意点

1、很多语言中已经存在，主要作用是把某个变量修饰为常量，在js中使用const修饰之后是不可以再次修改的。

```htaml
const a = 20;
a = 30 ; // 会报错
```

2、指向的对象是不可以修改的，但是可以修改对象的内部属性值

```html
  const obj = {
    name: 'cat',
    age: 20
  }
  console.log(obj)
  
  obj.age = 100;
  obj.name = 'dog'
  console.log(obj)
```



![image-20201116144852484](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20201116144852484.png)

### ES6：对象字面量的增强写法

属性增强

```html
const name = 'cat';
  const age = 20;
  // //ES5的写法
  // const obj = {
  //   name: 'cat',
  //   age: 20
  // }
  //ES6的写法
  const obj={
    name,
    age
  }
  console.log(obj)
```

函数增强

```html
const obj = {
run: function(){

}
}
const obj = {
run(){

}
}
```

