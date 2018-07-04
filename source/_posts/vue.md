---
layout: post
title: "vue入门知识小结"
date: 2017-9-26
tags: [vue]
comments: true
---
Vue.js是轻量级的前端界面框架，于2016年10月发布了2.0版本，目前得到了很多公司的青睐。实现了：数据渲染/数据同步、组件化/模块化、路由、ajax、数据流等。
### vue.js基础功能
#### 实例
Vue为构造函数，new是关键字，Vue中的内容是构造参数，el与data是参数选项。
```javascript
var my = new Vue({
    el:"#app",//对象装载的位置
    template:"<div>{{ message }}</div>",
    components:{ App },//引入子组件
    data:{
        message:"hello Vue.js!"
    }
})

//取data中的数据 my.message、my.$data
```
#### 组件
全局注册组件与局部注册组件、组件树的概念。
```javascript
//1. 全局注册组件的方法
Vue.component("my-header",{
    template: "<p>this is my header</p>",
    data:{

    }
});
//在html文件中
<div id = "app">
    <my-header></my-header>
</div>

//2.组件与子组件（局部注册）
var myHeaderChild = {
    template: "<p>this is my header child</p>"
}
var myHeader = {
    template: "<p><my-header-child></my-header-child></p>",
    components:{
        "my-header-child":myHeaderChild
    }
}
new Vue({
    el:"#app",
    data:{
        word:"hello world"
    },
    components:{
        "my-header":myHeader
    }
})
//在html文件中
<div id = "app">
    <my-header></my-header>
</div>

```
内置组件
```javascript
var myHeader = {
    template: '<p><my-header-child></my-header-child>'+
              '<component :is=""></component>'+
              '<keep-alive></keep-alive>'+
              '<router-view></router-view>'+
                '</p>',
    components:{
        "my-header-child":myHeaderChild
    }
}

```
在组件中的data需注意，因为会被其他组件引用，所以应该避免引用赋值,如
```javascript
new Vue({
    el:"#app",
    components:{
        "my-header":myHeader
    },
    data:{
        a:1,
        b:2
    }
})
```
建议使用函数返回值方式，如
```javascript
new Vue({
    el:"#app",
    components:{
        "my-header":myHeader
    },
    data: function(){
        return {
            a:1,
            b:2
        }
    }
})
```
这样做是为了避免组件之间的数据相互影响。

#### 指令
指令是指带有v-前缀的特殊属性，当表达式的值改变时，将其产生的连带影响，响应式的作用于DOM，指令可以有参数（指令只有以冒号表示）和修饰符（以.修饰）。
```javascript
var myHeader = {
    template: "<p v-html = 'test' v-on:keydown.enter = ''><my-header-child></my-header-child></p>",
    components:{
        "my-header-child":myHeaderChild
    }
}
```
自定义指令
```javascript
    <p v-color="'red'">hello world</p>
    <script>
        export default{
            directives:{
                color:function(el,binding){
                    el.style.color = binding.value
                }
            }
        }
    </script>

```


#### 计算属性
conputed计算属性 选项会被缓存
也可以通过调用方法来计算属性，使得每次的属性都是重新调用生成的
watch--监听属性变化
```javascript
<input type = "text" v-model = "myVal">
{{ myValueWithoutNum }}
{{ getMyValWithoutNum() }}

export default {
    data () {
        return {
            myVal:''
        }
    },
    watch:{
        myVal:function(val,oldVal) {
            console.log(val,oldVal);
        }
    },
    computed:{
        myValueWithoutNum () {
            return this.myVal.replace(/\d/g,'');//删掉所有数字
        }
    },
    methods:{
        getMyValWithoutNum (){
            return this.myVal.replace(/\d/g,'');//删掉所有数字
        }
    }
}
```

#### 事件绑定
```javascript
<button v-on:click="addItem">addItem</button>
<button v-on:click="toogle">toogle</button>

export default {
    data () {
        return {
            hello:'<span>hello world</span>',
            link:'http://www.baidu.com',
            className:['red-font','big-font'],
            hasError:false,
            classA:'hello',
            classB:'world',
            isPartA:true,
            list:[
                {
                    name: 'apple',
                    price: 34
                },
                {
                    name: 'banana',
                    price: 34
                },
            ]
        }
    },
    methods:{
        addItem () {//es6缩写，原为：addItem:function(){}
            Vue.set(this.list,1,{
                name:'pinaapple',
                price:231
            })
        },
        toggle () {
            this.isPartA = !this.isPartA
        }
    }
}

```
表单的数据双向绑定:v-model
```javascript
<input v-model.lazy = "myValue" type = "text"/>
{{ myValue }}

<input v-model="myBox" type = "checkbox" value = "111">
<input v-model="myBox" type = "checkbox" value = "222">
<input v-model="myBox" type = "checkbox" value = "333">

<select v-model = ="selection">
    <option v-for="item in selectOption" :value = "item.value">{{ item in text }}</option>
    <option value = "2">2222222</option>
</select>
{{ selection }}
export default {
    data () {
        return {
            myValue:'',
            myBox:[],
            selection:null,
            selectOption:[
                {
                    text:'apple',
                    value:0
                },
                {
                    text:'banana',
                    value:1
                },  
                {
                    text:'pinaapple',
                    value:2
                },
            ]
        }
    }
}


```



#### 模板渲染
```javascript
<template>
    <div>{{ hello }}</div>

    <div v-html="hello"></div>

    <div v-text="hello"></div>

    {{ status ? 'success' : 'fail' }}

    <p v-for = "(item,index) in list">{{ item.name }} - {{ item.price }} - {{ index }}</p>
    <p v-for = "(value,key) in objList">{{ value }}-{{ key }}</p>
</template>
<script>
    export default {
        data (){
            return {
                hello: "<p>world</p>",
                num: 1,
                status: true,
                list:[
                    {
                        name: 'apple',
                        price: 34
                    },
                    {
                        name: 'banana',
                        price: 34
                    },
                ],
                objList:{
                    name: "apple",
                    price: 34,
                    color: "pink",
                    weight: 12
                }
            }
        }
    }
</script>

```
#### 内置动画
```javascript
    <button v-on:click="show = !show"> Toogle </button>
    <div>
        <transition name="fade">  
           <p v-show = "show"> i am show </p>
        </transition>
    </div>
    <div>
        <transition 
        @before-enter="beforeEnter" 
        @enter="enter" 
        @leave="leave" 
        :css="false" >
            <p class = "animate-p" v-show="show">i am show</p>
        </transition>
    </div>

    <style>
        .fade-enter-active,.fade-leave-active {
            transition:all .5s;
        }
        .fade-enter,.fade-leave-active{
            opacity:0;
        }
    </style>
    <script>
        export default {
            methods:{
                beforeEnter:function(el){
                    $(el).css({
                        left:"-500px",
                        opacity:0
                    })
                },
                enter:function(el,done){
                    $(el).animate({
                        left:0,
                        opacity:1
                    },{
                        duration:1500,
                        complete:done
                    })
                },
                leave:function(el,done){
                    $(el).animate({
                        left:"500px",
                        opacity:0
                    },{
                        duration:1500,
                        complete:done
                    })
                }

            }
        }
    </script>
```

#### 组件交互
```javascript
<template>
    <div>
        <comp-a></comp-a> 

        <p :is="comToRender"></p>
       // is的好处是可以渲染不同的组件
       
    </div>
</template>

<script>
    import ComA from './components/a'
    export default{
        components:{
            ComA
        },
        data (){
            return {
                comToRender:"com-a"
            }
        }
    }
</script>

```
父组件向子组件传递信息的是passProps，props接受的参数形式可以是数组，也可以是对象。
子组件向父组件传递信息的是emitEvents

```javascript
//在父组件中引入子组件,并在子组件标签中传递参数,参数大小写不敏感（父组件页面）
<template>
    <div>
        <comp-a number=5></comp-a> 

        <p :is="comToRender"></p>

       //接受子组件的事件
        <com-a @my-event="getMyEvent"></com-a>

       
    </div>
</template>

<script>
    import ComA from './components/a'
    export default{
        components:{
            ComA
        },
        data (){
            return {
                comToRender:"com-a"
            }
        },
        methods:{
            getMyEvent (hello){
                console.log('i got my event'+hello);
            }
        }
    }
</script>

//将父组件传给子组件的参数number渲染到子组件中（子组件页面）
<template>
    <div>
       {{ hello }}
       {{ number }}       
       <button @click="emitMyEvent">emit</button>
    </div>
</template>

<script>
    export default{
        props:['number'],
        data (){
            return {
               hello:'i am a child component'
            }
        },
        methods:{
            emitMyEvent () {
                this.$emit('my-event',this.hello);//my-event:自定义事件，this.hello:参数
            }
        }
    }
</script>

```
