---
layout: post
title: "面试题汇总"
tags: [面试]
date: 2018-12-26
comments: true
---


### html 篇

1. HTML 标签的语义化：通过使用包含语义的标签（如 h1-h6）恰当地表示文档结构

2. css 命名的语义化是指：为 html 标签添加有意义的 class

3. Doctype 作用？标准模式与兼容模式各有什么区别?

   a. <!DOCTYPE>声明位于位于 HTML 文档中的第一行，处于 <html> 标签之前。告知浏览器的解析器用什么文档标准解析这个文档。DOCTYPE 不存在或格式不正确会导致文档以兼容模式呈现

   b.标准模式的排版 和 JS 运作模式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式显示,模拟老式浏览器的行为以防止站点无法工作



### JavaScript篇
1. 使以下代码正常运行
```javascript
const a = [1,2,3,4,5]
a.multiply()
console.log(a)  //1,2,3,4,5,1,4,9,16,25
```
答案为
```javascript
const a = [1, 2, 3, 4, 5];
Array.prototype.multiply = function () {
    var arr = this;
    var len = this.length;
    var arr1 = [];
    for (let i = 0; i < len; i++) {
        (function () {
            arr1 = arr[i] * arr[i];
        })()
        arr.push(arr1)
    }
    return arr.join();
}
console.log(a.multiply());//1, 2, 3, 4, 5, 1, 4, 9, 16, 25



Array.prototype.multiply = function (){ 
        this.forEach( (item,index,arr) => { 
            this.push(arr[index] ** 2) 
        }) 
    } 
    var a = [1,2,3,4,5] 
    a.multiply() 
    console.log(a)//[1, 2, 3, 4, 5, 1, 4, 9, 16, 25]

```

2. 为什么在js中`0.2+0.1 == 0.3`返回false？

回答：在js进行数字运算时，会有**精度缺失**的问题，简单的来说，由于0.1转换成二进制时是无限循环的，所以在计算机中只能存储一个近似值，0.1与0.2都是取得近似值，所以返回的是false。但是这并非绝对，有时两个近似值在进行计算的时候，得到的值在js的近似范围内，就可以返回true。

规避方法：为了避免小数计算的精度问题，最常用的方式是将浮点数转化成整数去进行计算。

3. JavaScript 中有哪些不同的数据类型？

回答： 有两种：主要数据类型和引用类型（也称原始类型和对象类型）

主要数据类型为：Number（数字）、String（字符串） 、Boolean（布尔值）、Null（空）和Undefined（未定义）
引用类型为： Object（对象）

4. 使用proxy实现数据绑定

答：proxy可以理解为在目标对象之前设置一层"拦截"，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写


5. new 关键字在 JavaScript 中有什么作用？

   a.声明一个中间对象

   b.将该中间对象的 proto 指向构造函数的原型

   c.将构造函数的 this 通过 apply 指向中间对象

   d.返回该中间对象,也就是返回了实例对象

6. 解释单向数据流和双向数据绑定。

   a. 单向数据绑定:指的是我们先把模板写好，然后把模板和数据（数据可能来自后台）整合到一起形成 HTML 代码，然后把这段 HTML 代码插入到文档流里面。 单向数据绑定缺点：HTML 代码一旦生成完以后，就没有办法再变了，如果有新的数据来了，那就必须把之前的 HTML 代码去掉，再重新把新的数据和模板一起整合后插入到文档流中。 简单的来说就是 DOM 操作直接改变

   b.数据模型（Module）和视图（View）之间的双向绑定。 用户在视图上的修改会自动同步到数据模型中去，同样的，如果数据模型中的值发生了变化，也会立刻同步到视图中去。

7. 解释 JavaScript 并发模型

   JavaScript 是单线程的，这意味着在任何时候只能有一段代码执行。JavaScript 主线程在运行时，会建立一个执行同步代码的栈和执行异步代码的队列.JavaScript 主线程在执行时，如果遇到异步的代码，就会将这些代码加入到异步队列中，然后继续执行同步代码栈中的代码。
