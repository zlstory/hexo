---
layout: post
title: "vue的知识点"
tags: [vue]
date: 2019-4-12
comments: true
---



1. nextTick 可以让我们在下次 DOM 更新循环结束之后执行延迟回调，用于获得更新后的 DOM。

2. 生命周期函数就是组件在初始化或者数据更新时会触发的钩子函数。

主要注意的几点：

a. beforeCreate调用的时候，是获取不到props或者data中的数据。

b. beforeMount是在挂载前执行的，然后开始创建VDOM并替换成真实DOM，最后执行mounted钩子。

c. activated和deactivated 是keep-alive组件独有的。

d. 在执行销毁操作前调用beforeDestory钩子函数，如果有子组件的话，也会递归销毁子组件，所有子组件都销毁完毕之后才会执行根组件的destroyed钩子函数

3. computed和watch的区别与使用场景

区别：

    watch：监听属性的变化；需要在数据变化时执行异步或者开销较大的操作时使用；

    computed：通过属性计算儿的来的属性;值有缓存；需要用return来返回最终结果。


场景：

   computed：当一个属性收到多个属性影响的时候

   watch：当一条数据影响多条数据的时候 


