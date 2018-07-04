---
layout: post
title: "VUE实战笔记1"
date: 2018-1-16
tags: [vue]
comments: true
---
### VUE核心功能

vue的核心功能是数据双向绑定

el用于指定一个页面中已经存在的DOM元素来挂载Vue实例，可以是html节点，也可以是CSS选择器。

通过VUE实例的data选项，可以声明应用内需要双向绑定的数据。建议所有会用到的数据都预先在data中声明，这样不至于将数据散落在业务逻辑中，难以维护。

 VUE常见生命周期钩子
    created：实例创建完成之后调用，已经完成了数据的观测，但是尚未挂载，即$el不能用，需要初始化处理一些数据时比较有用
    mounted: el挂载到实例之后调用，一般第一个业务逻辑会在这里开始
    beforeDestroy：实例销毁之前调用，主要是解绑一些实用addEventListener监听的事件等

钩子函数中的this指向的是调用它的Vue实例.

```javascript
var app = new Vue({
    el:app,
    data(){
        return{
            a:1
        }
    },
    mounted:function(){
        console.log(this.$el) <div id="app"></div>
    }
})
```

```javascript
 <span v-pre>{ { something } }</span>
```

### 过滤器

在 { { } } 插值的尾部添加一个管道符 | 可对数据进行过滤，经常用于格式化文本，比如字母大小写、货币千位使用逗号分离等。过滤的规则是自定义的。

过滤器可以串联，也可以接受参数

```javascript
{{ message | filterA | filterB }}

{{ message | filterA('arg1','arg2') }}
```

例如对时间进行格式化处理：

```javascript
 <div id = '#app'>
     {{ date | formatDate }}
 </div>

 <script>
     在小于10之前补0

     var padDate = function(value){
         return value < 10 ? '0' + value : value;
     }
     var app = new Vue({
         el:"#app",
         data:{
             date:new Date()
         },
         filters:{
             formatDate:function(value){
                 var date = new Date(value);
                 var year =date.getFullYear();
                 var month =padDate(date.getMonth()+1);
                 var day =padDate(date.getDate());
                 var hours =padDate(date.getHours());
                 var minutes =padDate(date.getMinutes());
                 var second =padDate(date.getSeconds());
                 return year + '-' + month + '-' + day + ' ' + hours + ':' + minutes + ':' + seconds;
             }
         },
         mounted(){
             var _this = this;声明一个变量只想VUE实例的this，保证作用域一致
             this.timer = setInterval(function(){
                 _this.date = new Date()
             },1000);
         },
         beforeDestroy:function(){
             if(this.timer){
                 clearInteral(this.timer)
             }
         }
     })
 </script>

```

### 指令

指令的作用就是当其表达式的值改变时，相应的将某些行为应用到DOM上

数据驱动是Vue.js的核心理念，所以不是万不得已的情况下，不要主动去操作DOM，只需维护好数据,DOM的事Vue自己本身会处理好。

#### v-bind

v-bind的基本用途是动态更新HTML元素上的属性，当数据变化时，就会重新渲染

#### v-on

v-on:用来绑定事件监听器，事件的方法都是以函数的形式写在Vue实例的methods属性中，方法内的this也是指向Vue实例。

Vue将methods里的方法也代理了，所以可以像访问Vue数据那样来调用方法

```javascript
 <div id = "#app">
     <p v-if = "show">hello world </p>
     <button v-on:click = "handleClose">
         点击隐藏
     </button>
 </div>

 <script>
     var app = new Vue({
         el:"#app",
         data:{
             show:true
         },
         methods:{
             handleClose(){
                 this.close(); //可以像访问Vue数据那样来调用方法
             },
             close(){
                 this.show = false;
             }
         }
     })
 </script>

```

### 语法糖

语法糖是指在不影响功能的情况下，添加某种方法实现同样的效果，从而方便程序开发。

比如v-bind简写为一个冒号:,v-on简写为一个@

### computed计算属性

模板内的表达式常用于简单的运算，但是当其过长或逻辑复杂时，会变得难以维护，所以需要计算属性来解决此问题。所有的计算属性都是以函数的形式写在Vue实例中computed选项内，最终返回的是计算后的结果。

在一个计算属性里可以完成各种复杂的逻辑，包括运算、函数调用等，只需要最后返回一个结果就可以了。如：通过计算展示购物车中的两个包裹的物品总价

```javascript
 <div id = 'app'>商品总价为：{{ prices }}</div>

 <script>
     var app = new Vue({
         el:"#app",
         data:{
             package1:[
                 {
                     name:'iphone7',
                     price:7000,
                     count:2
                 },
                  {
                     name:'iphone8',
                     price:5888,
                     count:1
                 }
             ],
             package2:[
                 {
                     name:'apple',
                     price:3,
                     count:3
                 },
                 {
                     name:'banana',
                     price:1.5,
                     count:4
                 }
             ]
         },
         computed:{
             price(){
                 var prices = 0;
                 for(var i = 0; i < this.package1.length; i++){
                     price += this.package1[i].price * this.package1[i].count
                 }
                 for(var i = 0; i < this.package2.length; i++){
                     price += this.package2[i].price * this.package2[i].count
                 }
                 return prices;
             }
         }
     })
 </script>
```

每一个计算属性都包含一个getter与setter，上述例子只是计算属性的默认用法，用了getter来获取。

setter函数：当手动修改计算属性的值就像修改一个普通数据一样，会触发setter函数，执行一些自定义的操作。

```javascript
 <div id = "app">姓名：{{ fullName }}</div>

 <script>
     var app = new Vue({
         el:"#app",
         data:{
             firstName:"Jack",
             lastName:"Green"
         },
         computed:{
             fullName:{
                 get(){
                     return this.firstName + ' ' + this.lastName;
                 },
                 set(newValue){
                     var names = newValue.split(' ');
                     this.firstName = names[o];
                     this.lastName = names[names.length - 1];
                 }
             }
         }
     })

      // use setter

     app.fullName = 'John Doe';
 </script>

```

计算属性的两个小技巧：

1 计算属性可以依赖其他计算属性

2 计算属性不仅可以依赖当前Vue实例的数据，还可以依赖其他实例的数据

computed与methods不同点：有些功能使用methods定义函数，也可以实现该功能，那为什么还要使用计算属性呢？原因是因为计算书型是基于依赖缓存的，即：当一个计算属性所依赖的数据发生变化时，才会重新取值。而methods不同，只要重新渲染就会被调用，函数就会被执行。

总结：当遍历大数组和做大量计算时，应当使用计算属性，除非是你不希望得到缓存。 

### 绑定class的几种方式

#### 对象语法

给class设置一个对象，可以动态的切换class

```javascript
 <div id="app">
     <div class="demo"   v-bind : class="{'active':isActive,'error':isError}"></div>
 </div>
 <script>
     var app = new Vue({
         el:"#app",
         data:{
             isActive: true,
             isError: false
         }
     })

 </script>
```

#### 数组语法

当需要应用多个class时，可以使用数组语法。

```javascript
 <div id="app">
     <div class="demo"   v-bind : class="{isActive,isError}"></div>
 </div>
 <script>
     var app = new Vue({
         el:"#app",
         data:{
             isActive: 'active',
             isError: 'error'
         }
     })

 </script>

```

在写复用组件时，如果表达式较长或者逻辑较为复杂，应优先使用计算属性动态设置类名。

#### 绑定内联样式

当使用内联样式时，css属性需要使用驼峰命名或者短横分隔命名。

```javascript
 <div id="app">
     <div class="demo"   v-bind : style="{'color':color,'fontSize':size+'px'}"></div>
 </div>
 <script>
     var app = new Vue({
         el:"#app",
         data:{
             color: 'green',
             fontSize: 16
         }
     })

 </script>

```

但是一般不会使用内联样式，都是写在data中或者computed里。

```javascript
 <div id="app">
     <div class="demo"   v-bind : style="styles"></div>
 </div>
 <script>
     var app = new Vue({
         el:"#app",
         data:{
            styles:{
                color: "blue",
                fontSize: 14+"px"
            }
         }
     })

 </script>

```

注意：使用:style时，vue.js会自动给特殊的css属性名称增加前缀，如transform。


### vue基本指令

#### v-cloak

v-cloak与css结合可以解决当网速慢、vue.js未加载完成、屏幕出现闪动的问题。

使用方法：

```javascript
<div id="app" v-cloak>{{ message }}</div>
<script>
    var app = new Vue({
        el:'#app',
        data:{
            message:" hello world"
        }
    })
</script>
<style>
[v-cloak]{
    display:none;
}
</style>
```

v-cloak是一个解决初始化慢导致页面闪动的最佳实践，但是在有webpack与vue-router的大项目中，项目是通过路由挂载不同组件时，则不需要v-cloak。

#### v-once

将元素或者组件只渲染一次，渲染过后，包括子节点，都不再随着数据的改变而改变，将被视为静态内容，此指令在业务中很少使用，但是当你需要进一步优化性能时，可能会用到v-once。

### 条件渲染指令

#### v-if、v-else-if、v-else

这三个表达式的顺序不能改变：v-if后跟v-else-if(可省略)后跟v-else,表达式为真时，当前组件以及所有子节点会被渲染，为假时则会被移除。

#### v-show

改变的是元素的display属性。v-show不能在\<template>上使用。

v-if更适合条件不经常改变的场景，v-show适用于频繁切换条件。

### 列表渲染指令 v-for

#### 遍历数组

v-for需要结合in来使用，并且支持一个可选参数作为当前项的索引。

```javascript
<div id = 'app'>
    <ul>
        <li v-for="(book,index) in books ">{{index}}---{{book}}</li>
    </ul>

</div>

```

#### 遍历对象

遍历对象属性的时候，支持两个可选参数：键名和索引

```javascript
<div id = 'app'>
    <ul>
        <li v-for="(value,key,index) in users ">{{index}}---{{key}}:{{value}}</li>
    </ul>
</div>

```

### 数组更新

#### 改变原数组的方法

- push()
- pop()
- shift()
- unshift()
- splice()
- sort()
- reverse()

#### 不改变原数组

- filter()
- concat()
- slice()

与JavaScript数组方法不同的是，vue中使用arr[2]=1,或者arr.length=1时并不会触发视图的更新，如果需要实现同样的效果时，可以使用vue的set方法，或者是$set。

#### 过滤与排序

不需要改变原数组，借助计算属性返回过滤或者排序后的数组。

### 方法与事件

#### 点击事件

@click的表达式可以是直接的JavaScript语句，也可以是在VUE实例中methods选项中的一个函数名。

在@click调用的函数名后可加可不加()，使用$event可以访问原生DOM事件。

```javascript

<div id = "#app">
     <p v-if = "show">hello world </p>
     <button v-on:click = "handleClose">
         点击隐藏
     </button>
 </div>

 <script>
     var app = new Vue({
         el:"#app",
         data:{
             show:true
         },
         methods:{
             handleClose(){
                 this.close(); //可以像访问Vue数据那样来调用方法
             },
             close(){
                 this.show = false;
             }
         }
     })
 </script>

```

#### 修饰符

在事件绑定的后面用圆点加一个后缀名来使用修饰符。

```javascript
//阻止事件冒泡
<a @ckick.stop = "show"></a>

//提交事件不再重载页面
<form @submit.prevent = "show"></form>

//添加事件侦听器时使用事件捕获模式
<div @click.capture = "handle"></div>

//自身触发(不在子元素触发)
<div @click.self = "handle"></div>

//事件只触发一次
<div @click.once = "handle"></div>

//监听键盘事件
<div @keyup.13 = "submit"></div>

```

### 表单与v-model

Vue的核心功能就是实现数据绑定，vue提供平v-model用于在表单中进行双向数据绑定。

如希望实时更新，可以使用@input事件来代替v-model。

```javascript

<div id="app">
    <input type="text" v-model="message">
    <input type="text" @input = "handleInput">
    <p>{{message}}</p>
    <p>{{message1}}</p>
</div>
 <script>
     var app = new Vue({
         el:"#app",
         data:{
           message:'',
           message1:''
         },
         computed:{
             handleInput(e){
                 this.message1 = e.target.value
             }
         }
     })
 </script>

```

当只有一个radio时，可以使用v-bind，不需要使用v-model

当radio组合使用实现互斥选择时，需要v-model配合value实现，不需要使用相同name属性值

当checkbox使用v-model时，绑定的是一个布尔值,v-model="checked"

当checkbox组合使用v-model时，选中的value值 会自动push到数组中

```javascript
<div id="app">
   <div>
        <p>单选框使用v-model：</p>
        <p><input type="radio" v-model="picked" value="html" id="html">
            <lable for="html">HTML</label></p>
        <p><input type="radio" v-model="picked" value="js" id="js">
            <lable for="js">JAVAScript</label></p>
        <p><input type="radio" v-model="picked" value="css" id="css">
            <lable for="css">CSS</label></p>
        <p>选中input的value值是：{{picked}}</p>
   </div>
    <div>
        <p>复选框使用v-model：</p>
         <p><input type="checkbox" v-model="checked" value="html" id="html">
            <lable for="html">HTML</label></p>
        <p><input type="checkbox" v-model="checked" value="js" id="js">
            <lable for="js">JAVAScript</label></p>
        <p><input type="checkbox" v-model="checked" value="css" id="css">
            <lable for="css">CSS</label></p>
        <p>选中input的value值是：{{checked}}</p>
   </div>
</div>
<script>
     var app = new Vue({
         el:"#app",
         data:{
            picked:'html',
            checked:['html','js']
         }
     })
 </script>

```

v-model也可以绑定动态的value值，即在input中使用v-bind:value

#### v-model的修饰符

- v-model.lazy：不是实时改变，而是失焦或者按回车时才会更新

- v-model.number：平时输入框输入数字时，value是string类型，使用此修饰符可以让其改为number类型。

- v-model.trim：自动过滤首尾空格
