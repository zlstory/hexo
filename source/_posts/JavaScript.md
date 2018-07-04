---
layout: post
title: "JavaScript"
date: 2017-9-9
tags: [javaScript小知识]
comments: true
---

### JavaScript与其他语言的关系
- Java是JavaScript语法的参考模型，他同时影响JavaScript将值分为原始类型和对象类型，以及日期构造函数。(一直听说JavaScript与java没关系，现在发现还是有血缘关系的，而且es6之后增加了块级作用域等，更像java语法了)
- AWK给了JavaScript函数的灵感，JavaScript的关键字function来源于AWK
- 因为Scheme，JavaScript拥有第一类函数和闭包
- Perl和Python影响了JavaScript对字符串、数组和正则表达式的处理方式
- HyperTalk启发了JavaScript如何集成到浏览器，这使得HTML标签拥有事件处理属性

### 基础JavaScript
#### 原始值和对象
- 原始值包括布尔值、数字、字符串、null、undefined
- 其他的值为对象，常见的对象有:简单对象，数组，正则表达式。
原始值与对象最主要的区别在于他们的比较方式：每个对象都有唯一的标识且仅（严格的）等于本身。
```javascript
var obj1 = {};
var obj2 = {};
console.log(obj1 === obj2); //false
console.log(obj1 === obj1); //true
```
所有的原始值，只要编码值相同，则被认为相等。
```javascript
var num1 = 123;
var num2 = 123;
console.log(num1 === num2); //true

```
#### arguments
auguments不是数组，但是类似于数组：具有length属性，可以用[]去访问元素，但是不能移除他的元素，也不调用数组的方法。
强制函数参数的长度
```javascript
function pair(x,y){
    if(arguments.length !== 2){
        throw new Error("需要两个参数");
    }
    ...
}

```
强制将arguments转换成数组
```javascript
function toArray(arg){
    return Array.prototype.slice.call(arg);
}
```

#### 变量提升特性
所有变量生命都会被提升：声明会被移动到函数的开始出，而赋值则仍然会在原来的位置进行。
```javascript
//
console.log(abc);  //undefined

//函数执行顺序
var abc;
console.log(abc);  //undefined

```
#### 闭包
```javascript
 function add(num){
     num ++;
     console.log(num);
 }
add(1);//2
add(1);//2

//闭包函数
function closure(num){
    return function(){
            num ++;
            return num;
        }
}
var result = closure(1);
console.log(result());//2
console.log(result());//3

//闭包造成的无意共享
var arr = [];
for (var i = 0; i < 5; i ++){
    result.push(function () {return i;});//此时i永远为5
}

//使用IIFE:立即调用函数表达式
var arr1 = [];
for(var i = 0; i < 5; i ++){
    (function(){
        var j = i ;
        arr1.push(function(){
            return j;
        })
    })()
}

console.log(arr1[1]());//1
console.log(arr1[4]());//4
```

#### 对象
- 使用字面量创建普通对象
```javascript
var preson = {
    name: Crystal,
    age: 23,
    descroipt: function(){
        return 'an adorable girl'
    }
}
```
- 使用.操作符与[ ]获取与设置对象属性，获取不存在的属性将会得到undefined。
- 使用in运算符检查属性是否存在，结果为true或false。
- 使用delete运算符移除属性。

#### 构造函数与实例
构造函数就是对象工厂，按照惯例，构造函数的名称以大写字母开头
```javascript
function Point(x,y) {
    this.x = x;
    this.y = y;
}
Point.prototype.dist = function() {
    return (this.x + this.y);
}

//使用new运算符来使用Point

var p = new Point(3,4);//p为Point的一个实例
p.dist(); //7

```
#### 正则表达式的三种方法
1. test()方法：匹配
2. exec()方法：匹配以及捕获分组
3. replace()方法：搜索和替换

#### eval()
eval在语句的上下文中解析他的参数，如果需要eval返回一个对象，需要用小括号将对象字面量括起来
```javascript
    eval('{foo:233}');//233
    eval('({foo:223})');//{foo:223}

```