---
layout: post
title: "JSON"
date: 2017-7-05
tags: [javaScript,JSON]
comments: true
toc: true
---
## JSON的两种方法小记

### JSON简介

JSON的全称是：JavaScript Object Notation，意思是JavaScript对象表示法，是一种轻量级的数据交换格式。JSON的语法是JavaScript语法的子集，与js中对象和数组的语法十分相近。

JSON是一种数据序列化格式，基于JavaScript的直接量，可以表示null、Boolean、Number、String、array以及Object对象。

JSON里不可以表示undefined、NaN与Infinity、函数、日期以及正则。

在JSON中，有两种结构：数组与对象

> JSON中没有变量的概念，因为JSON不是JavaScript语句，所以末尾不需要加分号。且对象属性必须加双引号，不能是单引号。


## JSON方法

### JSON.parse()

JSON.parse(s,func):解析json格式中的字符串，返回该字符串表示的JavaScript值。s:需要解析的字符串；func:用来转换解析值的可选函数。返回值：一个对象、数组或者原始值。


### JSON.stringify()

JSON.stringify(o,func,indet):序列化对象、数组或者原始值。o:需要转换成JSON字符串的对象、数组或者原始值；func:对字符串化前对值做一些替换；indet：指定字符串缩进字符的空格个数。


```javascript
//JSON对象
var JSON_obj = {"name":"zlstory","age":"23","sfydx":"yes"}; //object

//JSON数组
var JSON_arr = [{"name":"Crystal"},{"name":"Sinsle"}]; //array

//JSON字符串
var JSON_str = '{"name":"zlstory","age":"23","sfydx":"yes"}';

console.log(JSON.parse(JSON_str)); //转换成JSON_obj

console.log(JSON.stringify(JSON_obj)); //由object转换成string类型
//{"name":"zlstory","age":"23","sfydx":"yes"}

console.log(JSON.stringify(JSON_arr));  //由array转换成string类型
//[{"name":"Crystal"},{"name":"Sinsle"}]


```

```javascript
o = {"info":[{
        "t": "兰蔻根源补养气色水凝乳液15ml*2",
        "pcp": "48",
        "img": "img/pic0.jpg",
        "sid": "1"
    }]
};

var a = JSON.stringify(o);  //转换成字符串

console.log(a);
//{"info":[{"t":"兰蔻根源补养气色水凝乳液15ml*2","pcp":"48","img":"img/pic0.jpg","sid":"1"}]}

var b = JSON.parse(a);  //还原成对象

console.log(b);
//{"info":[{
//       "t": "兰蔻根源补养气色水凝乳液15ml*2",
//        "pcp": "48",
//        "img": "img/pic0.jpg",
//        "sid": "1"
//    }]
//};

```
