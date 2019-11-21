---
layout: post
title: "vue常用的八种生命周期函数"
tags: [vue]
date: 2018-7-20
comments: true
---

生命周期函数就是vue实例在某一个时间点会自动执行的函数,可以直接在vue实例中执行，不需要在methods中定义。

常用的八种生命周期函数

(1) beforeCreate:创建vue实例并且实例进行了基础的初始化之后就会执行。
(2) created：接着vue会继续处理一些外部的注入以及双向绑定的相关内容，完成之后触发created函数。
(3) beforeMount：vue实例中有了数据并定义了template之后，在页面渲染之前会触发beforeMount函数
(4) mounted: vue中的dom挂载在页面之后，执行mounted函数
(5) beforeDestroy：当destory()方法调用时，当组件即将被销毁时触发该函数。
(6) destroyed:当组件完全被销毁时，会触发destroyed
(7) beforeUpdate:当数据发生改变的时候，触发beforeUpdate函数。
(8) updated:虚拟dom重新渲染之后，执行updated函数

```javascript
 <div id="root"></div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    
    var vm = new Vue({
        el:"#root",
        template:"<div>{{test}}</div>",
        data:{
            test:"hello world"
        },
        beforeCreate:function(){
            console.log('beforeCreate')
        },
        created:function(){
            console.log("created")
        },
        beforeMount:function(){
            console.log(this.$el);//<div id="root"></div>
            console.log("beforeMount")
        },
        mounted:function(){
            console.log(this.$el);//<div>hello world</div>
            console.log("mounted")
        },
        beforeDestroy:function(){
            console.log("beforeDestory")
        },
        destroyed:function(){
            console.log("destoryed")
        },
        beforeUpdate:function(){
            console.log("beforeUpdate")
        },
        updated:function(){
            console.log("updated")
        }
        
    })
```