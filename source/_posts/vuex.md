---
layout: post
title: "vuex的学习总结"
tags: [vue，vuex]
date: 2019-5-20
comments: true
---


## 什么是vuex
 vuex：专门为vue.js应用程序开发的状态管理模式

 ## 为什么用vuex
 当我们构建一个中大型的单页面应用程序时，vuex可以更好的帮助我们在组建外部统一管理状态

## 核心概念

### state(必须)

1. state是唯一的数据源

2. 单一状态树，即没有层级

```javascript
computed:{
    count(){
        return this.$store.state.count
    }
}
```

### getters

 通过getters可以基于state派生出一些新的状态

```javascript
const store = new Vuex.Store({
    state:{
        todos:[
            {id:1,text:"111",done:true},
            {id:2,text:"222",done:false},
        ]
    },
    getters:{
        doneTodos: state=>{
            return state.todos.filter(todo => todo.done)
        }
    }
})

```


### mutations(必须)

更改vuex的store中状态唯一的方法就是提交mutation

```javascript
const store = new Vuex.Store({
    state:{
        count:1
    },
    mutations:{ //唯一的方法去改变
        increment(state){
            state.count ++
        }
    }
})

//调用increment方法

store.commit("increment")

```

### action

action提交的是mutation，而不是直接变更状态

action可以包含任意异步操作

```javascript
const store = new Vuex.Store({
    state:{
        count:0
    },
    mutations:{ //只能进行同步操作
        increment(state){
            state.count ++
        }
    },
    actions:{
        increment(context){ //可以执行异步操作
            context.commit("increment")
        }
    }
})
```

### modules

面对复杂的应用程序，当管理的状态比较多的时候，我们需要将vuex的store对象分割成模块（modules）

```javascript

const moduleA = {
    state : { ... },
    mutation : { ... },
    actions : { ... },
    getters : { ... },
}

const moduleB = {
    state : { ... },
    mutation : { ... },
    actions : { ... },
    getters : { ... },
}

const store = new Vuex.Store({
    modules:{
        a:moduleA,
        b:moduleB 
    }
})


```

