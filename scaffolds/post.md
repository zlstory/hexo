---
title: {{ title }}
date: {{ date }}
tags:
---

<!-- 




<!
<!-- 例如对时间进行格式化处理： 

```javascript
// <div id = '#app'>
//     {{ date | formatDate }}
// </div>

// <script>
//     //在小于10之前补0

//     var padDate = function(value){
//         return value < 10 ? '0' + value : value;
//     }
//     var app = new Vue({
//         el:"#app",
//         data:{
//             date:new Date()
//         },
//         filters:{
//             formatDate:function(value){
//                 var date = new Date(value);
//                 var year =date.getFullYear();
//                 var month =padDate(date.getMonth()+1);
//                 var day =padDate(date.getDate());
//                 var hours =padDate(date.getHours());
//                 var minutes =padDate(date.getMinutes());
//                 var second =padDate(date.getSeconds());
//                 return year + '-' + month + '-' + day + ' ' + hours + ':' + minutes + ':' + seconds;
//             }
//         },
//         mounted(){
//             var _this = this;//声明一个变量只想VUE实例的this，保证作用域一致
//             this.timer = setInterval(function(){
//                 _this.date = new Date()
//             },1000);
//         },
//         beforeDestroy:function(){
//             if(this.timer){
//                 clearInteral(this.timer)
//             }
//         }
//     })
// </script>

```

## 指令

指令的作用就是当其表达式的值改变时，相应的将某些行为应用到DOM上

数据驱动是Vue.js的核心理念，所以不是万不得已的情况下，不要主动去操作DOM，只需维护好数据,DOM的事Vue自己本身会处理好。

### v-bind

v-bind的基本用途是动态更新HTML元素上的属性，当数据变化时，就会重新渲染

### v-on

v-on:用来绑定事件监听器，事件的方法都是以函数的形式写在Vue实例的methods属性中，方法内的this也是指向Vue实例。

Vue将methods里的方法也代理了，所以可以像访问Vue数据那样来调用方法

```javascript
// <div id = "#app">
//     <p v-if = "show">hello world </p>
//     <button v-on:click = "handleClose">
//         点击隐藏
//     </button>
// </div>

// <script>
//     var app = new Vue({
//         el:"#app",
//         data:{
//             show:true
//         },
//         methods:{
//             handleClose(){
//                 this.close();//可以像访问Vue数据那样来调用方法
//             },
//             close(){
//                 this.show = false;
//             }
//         }
//     })
// </script>

```

## 语法糖
<!-- 
语法糖是指在不影响功能的情况下，添加某种方法实现同样的效果，从而方便程序开发。

比如v-bind简写为一个冒号:,v-on简写为一个@ 

## computed计算属性
<!-- 
模板内的表达式常用于简单的运算，但是当其过长或逻辑复杂时，会变得难以维护，所以需要计算属性来解决此问题。所有的计算属性都是以函数的形式写在Vue实例中computed选项内，最终返回的是计算后的结果。

在一个计算属性里可以完成各种复杂的逻辑，包括运算、函数调用等，只需要最后返回一个结果就可以了。如：通过计算展示购物车中的两个包裹的物品总价 

```javascript
// <div id = 'app'>商品总价为：{{ prices }}</div>

// <script>
//     var app = new Vue({
//         el:"#app",
//         data:{
//             package1:[
//                 {
//                     name:'iphone7',
//                     price:7000,
//                     count:2
//                 },
//                  {
//                     name:'iphone8',
//                     price:5888,
//                     count:1
//                 }
//             ],
//             package2:[
//                 {
//                     name:'apple',
//                     price:3,
//                     count:3
//                 },
//                 {
//                     name:'banana',
//                     price:1.5,
//                     count:4
//                 }
//             ]
//         },
//         computed:{
//             price(){
//                 var prices = 0;
//                 for(var i = 0; i < this.package1.length; i++){
//                     price += this.package1[i].price * this.package1[i].count
//                 }
//                 for(var i = 0; i < this.package2.length; i++){
//                     price += this.package2[i].price * this.package2[i].count
//                 }
//                 return prices;
//             }
//         }
//     })
// </script>
```
<!-- 
每一个计算属性都包含一个getter与setter，上述例子只是计算属性的默认用法，用了getter来获取。

setter函数：当手动修改计算属性的值就像修改一个普通数据一样，会触发setter函数，执行一些自定义的操作。 

```javascript
// <div id = "app">姓名：{{ fullName }}</div>

// <script>
//     var app = new Vue({
//         el:"#app",
//         data:{
//             firstName:"Jack",
//             lastName:"Green"
//         },
//         computed:{
//             fullName:{
//                 get(){
//                     return this.firstName + ' ' + this.lastName;
//                 },
//                 set(newValue){
//                     var names = newValue.split(' ');
//                     this.firstName = names[o];
//                     this.lastName = names[names.length - 1];
//                 }
//             }
//         }
//     })

//     // use setter

//     app.fullName = 'John Doe';
// </script>

```
<!-- 
计算属性的两个小技巧：

1 计算属性可以依赖其他计算属性

2 计算属性不仅可以依赖当前Vue实例的数据，还可以依赖其他实例的数据

computed与methods不同点：有些功能使用methods定义函数，也可以实现该功能，那为什么还要使用计算属性呢？原因是因为计算书型是基于依赖缓存的，即：当一个计算属性所依赖的数据发生变化时，才会重新取值。而methods不同，只要重新渲染就会被调用，函数就会被执行。

总结：当遍历大数组和做大量计算时，应当使用计算属性，除非是你不希望得到缓存。 

## 绑定class的几种方式

### 对象语法

给class设置一个对象，可以动态的切换class

```javascript
// <div id="app">
//     <div class="demo"   v-bind : class="{'active':isActive,'error':isError}"></div>
// </div>
// <script>
//     var app = new Vue({
//         el:"#app",
//         data:{
//             isActive: true,
//             isError: false
//         }
//     })

// </script>
```

### 数组语法

当需要应用多个class时，可以使用数组语法。

```javascript
// <div id="app">
//     <div class="demo"   v-bind : class="{isActive,isError}"></div>
// </div>
// <script>
//     var app = new Vue({
//         el:"#app",
//         data:{
//             isActive: 'active',
//             isError: 'error'
//         }
//     })

// </script>

```

在写复用组件时，如果表达式较长或者逻辑较为复杂，应优先使用计算属性动态设置类名。

## 绑定内联样式

当使用内联样式时，css属性需要使用驼峰命名或者短横分隔命名。

```javascript
// <div id="app">
//     <div class="demo"   v-bind : style="{'color':color,'fontSize':size+'px'}"></div>
// </div>
// <script>
//     var app = new Vue({
//         el:"#app",
//         data:{
//             color: 'green',
//             fontSize: 16
//         }
//     })

// </script>

```

但是一般不会使用内联样式，都是写在data中或者computed里。

```javascript
// <div id="app">
//     <div class="demo"   v-bind : style="styles"></div>
// </div>
// <script>
//     var app = new Vue({
//         el:"#app",
//         data:{
//            styles:{
//                color: "blue",
//                fontSize: 14+"px"
//            }
//         }
//     })

// </script>

```

注意：使用:style时，vue.js会自动给特殊的css属性名称增加前缀，如transform。

<p style = "color: rgb(114, 171, 200);font-size:14px;margin:14px;font-family:Arial">风默默地站在路口,黑暗在用灯盏敬酒</p> -->