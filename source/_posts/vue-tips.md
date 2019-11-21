---
layout: post
title: "vue知识小结"
tags: [vue]
date: 2018-7-20
comments: true
---

#### computed、watch与methods

computed与watch具有缓存机制，如果涉及的变量不改变，则不会执行。

methods是任何变量改变都会触发methods函数。

####  v-if 与 v-show

v-if 与 v-show 都能控制模板标签是否在页面中显示，但是条件是false时，v-if对应的标签不存在于dom中，而v-show是在标签内加入display：none隐藏dom；所以v-show的性能更高一点，因为他不会频繁的去操作dom。

####  key值

Vue提供了一种方式来表达“这两个元素是完全独立的，不要复用它们”。只需添加一个具有唯一值的 key 属性即可。

在v-for循环时可以使用key值来提高性能，需注意key尽量唯一且不要用index来标识key。

#### 改变数组并实时触发视图更新有三种方法
1. 使用vue提供的数组变异方法

    pop push shift unshift splice sort reverse

2.  直接改变引用数据

3.  使用Vue.set()方法或者实例

    因为 Vue 无法探测普通的新增属性，所以直接向vue中数组以及对象中直接添加数据虽然改变了数据但是并不会改变页面中的视图。

    set方法用于向响应式对象中添加一个属性，并确保这个新属性同样是响应式的，且触发视图更新。它必须用于向响应式对象上添加新属性
