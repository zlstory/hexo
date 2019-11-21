---
layout: post
title: "通过todolist简单了解vue的mvvm模式"
tags: [vue]
date: 2018-7-19
comments: true
---


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


2. 使用JavaScript实现todolist的功能，体现的是MVP的编程思想
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
(1) MVP: Model - View - Presenter

M是模型层（上段代码并没有体现）

V是DOM

P是View层以及Model层的中转站(js部分)，当点击按钮的时候，控制器里面的代码会执行，负责了所有的逻辑部分，控制器可以调用模型层来发起ajax请求，也可以操作dom改变视图。

(2) MVVM:  View - ViewModel - Model

Model:负责存储数据
View:视图层，负责显示数据
ViewModel:vue自带的一层(内置)，不需要我们去关心怎么实现的。
当我们使用MVVM设计模式的时候，我们只需要关心view和model。

(3) 总结：在mvp设计模式开发的时候是面向DOM进行操作，mvvm设计模式面向数据进行编程，大大的简化了DOM的操作，可以节约代码量，提高代码性能。