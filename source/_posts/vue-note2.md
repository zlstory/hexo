---
layout: post
title: "VUE去哪儿网学习笔记1"
date: 2018-9-27
tags: [vue]
comments: true
---

知识点：
    1. 使用axios进行ajax数据的获取
    2. 使用vue-router来进行多页面之间的路由跳转
    3. 使用vuex各个组件之间的数据共享
    4. 使用异步组件来优化性能
    5. 使用stylus编写样式
    6. 使用递归组件来实现组件调用自身组件
    7. 各种插件的调用：如swiper
    8. 自己对公用组件的拆分


# 第一章
## 查看vue.js的官方文档
1. 使用vue.js实现todoList的功能，体现了vue的编程思想是MVVM：不改变DOM，而是只操作数据，最后dom随着数据的改变而改变
```javascript
 <div id="app">
        <input type="text" v-model = "inputValue">
        <button @click = "handleBtnClick">提交</button>
        <ul>
            <li v-for="item in list">
                {{item}}
            </li>
        </ul>
    </div>


    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var app = new Vue({
            el:"#app",
            data:{
                list:[],
                inputValue:''
            },
            methods:{
                handleBtnClick:function(){
                  //  console.log(this.inputValue)
                  this.list.push(this.inputValue);
                  this.inputValue = '';
                }
            }
        })
    
    </script>


```


2. 使用JavaScript实现todolist的功能，体现的是MVP的编程思想：M是模型层（此段代码并没有体现），dom是V层，P(核心层)是js部分，指的是控制器，当点击按钮的时候，控制器里面的代码会执行，负责了所有的逻辑部分，控制器可以调用模型层来发起ajax请求，也可以操作dom改变视图。   Presenter层是View层以及Model层的中转站
```javascript
 
    <div id="app">
        <input type="text" id="input">
        <button id="btn">提交</button>
        <ul id="list">
         
        </ul>
    </div>

    <script src="http://code.jquery.com/jquery-1.11.3.min.js"></script>
    <script>
        function Page() {  }

        $.extend(Page.prototype,{
            init:function(){
                this.bindEvents()
            },
            bindEvents:function(){
                var btn = $("#btn");
                btn.on("click",$.proxy(this.handleBtnClick,this))
            },
            handleBtnClick:function(){
                var inputValue = $("#input").val();
                var ulElem = $("#list")
                ulElem.append("<li>"+inputValue+"</li>");
                $("#input").val("");
            }
        })

        var page = new Page();
        page.init()
    </script>

```




3. MVP与MVVM设计模式
MVVM:  View - ViewModel - Model

Model:负责存储数据
View:视图层，负责显示数据
ViewModel:vue自带的一层(内置))，不需要我们去关心怎么实现的。
当我们使用MVVM设计模式的时候，我们只需要关心view和model。

在mvp设计模式开发的时候是面向DOM，mvvm设计模式面向数据进行编程，大大的简化了DOM的操作，可以节约代码量

4. 前端组件化

使用Vue.component创建全局组件
全局注册的行为必须在根 Vue 实例 (通过 new Vue) 创建之前发生

```javascript
Vue.component("TodoItem",{
    props:["content"],
    template:"<li>{{content}}</li>"
});

```
注册局部组件

```javascript
 var TodoItem = {
    props:["content"],
    template:"<li>{{content}}</li>"
}
//在根部注册
var app = new Vue({
    el:"#root",
    components:{
        TodoItem
    }
})
```

5. 子组件向父组件传值
在子组件的模板中定义事件`deleteSelf`，并在methods中使用$emit()向外触发事件，并且可以传值;
```javascript
//在子组件中的methods中
 var TodoItem = {
    props:["content",'index'],
    template:"<li @click='deleteSelf'>{{content}}</li>",
    methods:{
        deleteSelf:function(){
            this.$emit('delete',this.index);
        }
    }
}
```
在局部组件中，动态的监听刚刚在子组件中定义的事件`delete`，并将其动态的绑定到父组件的事件中 `deleteSon`,此时需要父组件改变数据，则DOM树会相应改变。
```javascript
  <todo-item v-for="(item,index) in list " 
            v-bind:index="index"  
            v-bind:content="item"   
            v-on:delete = "deleteSon">
  </todo-item>

     var app = new Vue({
        el:"#root",
        components:{
            TodoItem
        },
        data:{
            todoValue:"",
            list:[]
        },
        methods:{
            deleteSon:function(index){
                this.list.splice(index,1)
            }
        }
    })
```
6. 生命周期函数

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

7. computed、watch与methods

computed与watch具有缓存机制，如果涉及的变量不改变，则不会执行。

methods是任何变量改变都会触发methods函数。

8. 样式绑定

(1) class的对象绑定
```javascript
<div id="root">
    <button @click="handleColor" :class="{active : isActive}">样式改变</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    var vm = new Vue({
        el:"#root",
        data:{
            isActive: false
        },
        methods:{
            handleColor:function(){
                this.isActive = !this.isActive
            }
        }
    })
</script>
```

(2) class的数组绑定

```javascript
<div id="root">
    <button @click="handleColor" :class="[isActive]">样式改变</button>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    var vm = new Vue({
    el:"#root",
    data:{
        isActive: ""
    },
    methods:{
        handleColor:function(){
            this.isActive = this.isActive == "active" ? "" : "active";
        }
    }    
})
</script>
```

(3) style的内联样式

```javascript
<div id="root">
    <div :style="styleObj" @click="handleColor">
        hello world
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    
    var vm = new Vue({
        el:"#root",
        data:{
            styleObj:{
                color : "black"
            }
        },
        methods:{
            handleColor:function(){
                this.styleObj.color = this.styleObj.color == 'black' ? "red" : "black";
            }
        }
    })
</script>
```

(4) style的数组样式

```javascript
 <div id="root">
    <div :style="[styleArr,{fontSize:'20px'}]" @click="handleColor">
        hello world
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    
    var vm = new Vue({
        el:"#root",
        data:{
            styleArr:{
                color : "black"
            }
        },
        methods:{
            handleColor:function(){
                this.styleArr.color = this.styleArr.color == 'black' ? "red" : "black";
            }
        }
    })
</script>
```
9. v-if 与 v-show
v-if 与 v-show 都能控制模板标签是否在页面中显示，但是条件是false时，v-if对应的标签不存在于dom中，而v-show是在标签内加入display：none隐藏dom；所以v-show的性能更高一点，因为他不会频繁的去操作dom。

10. key值：Vue提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 key 属性即可。
在v-for循环时可以使用key值来提高性能，需注意key尽量唯一且不要用index来标识key。

11. 当我们要改变数据操作数组时，必须要用vue已经定义的七种方法来操作数组数据，不能够直接通过数组下标来操作(数据改变但是页面并不会改变)。

数组的变异方法：

pop push shift unshift splice sort reverse

12. set方法

因为 Vue 无法探测普通的新增属性，所以直接向vue中数组以及对象中直接添加数据虽然改变了数据但是并不会改变页面中的视图。

set方法用于向响应式对象中添加一个属性，并确保这个新属性同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新属性

```javascript
 <div id="root">
    <div v-for = "(item,key,index) of userInfo">
        {{item}} --- {{key}} --- {{index}}
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    
    var vm = new Vue({
        el:"#root",
        data:{
            userInfo:{
                name:"张三",
                age:"23",
                gender:"male"
            }
        }
    })

    Vue.set(vm.userInfo,"address","hangzhou");
    // vm.set(vm.userInfo,"address","hangzhou");
</script>
```

总结：改变数组并实时触发视图更新有三种方法
(1) 使用vue提供的数组变异方法

(2) 直接改变应用数据

(3) 使用Vue.set()方法或者实例


