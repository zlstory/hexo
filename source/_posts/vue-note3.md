---
layout: post
title: "VUE去哪儿网学习笔记2"
date: 2018-10-18
tags: [vue]
comments: true
---


### vue组件中的细节

在table中使用tr作为全局组件注入到table时，会发生问题：tr并没有按照预期所想放在tbody中，而是与table同级。

```javascript
<div id="root">
    <table>
        <tbody>
            <row></row>
        </tbody>
    </table>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    Vue.component("row",{
        template:"<tr>this is tr</tr>"
    })
    
    var vm = new Vue({
        el:"#root"
    })
</script>

//<div id="root">
//     <tr>this is tr</tr>
//     <table>
//         <tbody></tbody>
//     </table>
// </div>


```
为了解决这个问题，可以使用在tr标签中使用vue中的is，这样写既能保证组件的数据时正确的，又能保证符合h5的规范。同ul li


```javascript
<div id="root">
    <table>
        <tbody>
            <tr is="row"></tr>
        </tbody>
    </table>
</div>

```

#### 子组件的data问题

在子组件中定义data时必须是一个函数形式 使用return返回数据，而不能是一个对象，因为根组件只会被调用一次，而子组件可能会被不同得地方中调用多次，避免各个地方得同一个子组件中得数据混淆(避免公用数据)，使用函数的return一个对象可以保证子组件拥有独立的数据存储。


#### 在vue中获取dom

虽然vue不建议在dom上面操作事件，但是当你需要操作dom的时候，可以使用ref来获取。

#### 父子组件之间传值问题

父组件向子组件传递数据是通过属性的方式

父组件可以向子组件传递参数，但是子组件不能修改父组件中的变量

#### slot
一般父组件向子组件之间进行传值是通过prop的形式，但是当我们需要向子组件传递html文本的时候，发现prop并不好用，如:
```javascript
<div id="root">
    <child content="<p>通过prop向子组件中传值：</p>"></child>
</div>
  var child = {
    template:`
        <div>
            <div v-html = "this.content"></div>
            this is child
        </div>
    `,
    props:['content']
}
var app = new Vue({
    el:"#root",
    components:{
        child:child
    }
})
```
此时我们浏览器解析出来的dom结构为：
```html
<div id="root">
    <div>
        <div>
             <p>通过prop向子组件中传值：</p>
        </div>
        this is child
    </div>
</div>
```
可以发现p标签是被一个div包裹着的，这并不是我们所需要的，而且当我们需要向子组件传递大量html的时候，content中需要写入大量的代码，所以slot(插槽)的作用就可以体现出来了。

slot：父组件向子组件中优雅的传递dom

