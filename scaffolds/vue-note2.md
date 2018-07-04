---
layout: post
title: "VUE实战笔记2"
date: 2018-1-25
tags: [vue]
comments: true
---

### 组件

组件（components）就是为了提高重用性的，让代码可以复用。

#### 注册组件

1 全局注册

```javascript
<div id="app">
    <my-component></my-component>
</div>
<script>
    Vue.component('my-component',{
        template:"<div>全局组件</div>"
    })
    var app = new Vue({
        el:'#app'
    })
</script>
```

2 局部注册组件

使用components选项注册局部组件

使用is属性可以挂载组件

在components中除了template之外还可以有data/computed/methods等，但是data必须为函数，最终将数据return出去。目的是为了组件之间的数据互不影响，达到组件复用的目的。

```javascript
<div id="app">
    <table>
        <tbody is="my-component"></tobody>
    </table>
</div>
<script>
    var Child = {
        template:"<div>局部组件</div>"
    }
    var app = new Vue({
        el:'#app',
        components:{
            'my-component': Child`
        }
    })
</script>
```

#### 传递数据

正向传递数据使用props：父组件向子组件传递数据或者参数，子组件根据参数不同来渲染不用的内容或者执行操作。

props的值只接受两种形式：1.字符串数组  2.对象

在script中的props使用驼峰命名，在DOM中使用短横分隔命名

可以使用v-bind来动态绑定props的值

由于对象和数组是引用类型，指向同一个内存空间，所以props在子组件改变内容时，是会影响父组件的。
；
为了不改变父组件的原始值，有两种方式，第一种：可以在子组件中定义一个变量来继承父组件的数据，以后改变的是子组件的变量，。