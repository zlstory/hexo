---
layout: post
title: "axios传参报错400问题总结"
date: 2018-8-9
tags: [vue]
comments: true
---

在使用axios传参的时候，发现只用post方法的时候，默认请求方式为payload，

![ ](http://p09xm7bj0.bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_20180810163720.png)

百度了各种操作之后，试过以下几种方法：

1. 改变headers

```javascript
 headers: {  'Content-Type': 'application/x-www-form-urlencoded; charset=UTF-8' }
```

![ ](http://p09xm7bj0.bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_201808101637202.png)
这种操作虽然将payload改为了formData形式，但是依旧报400错

2. Json.stringify()

请求头问题排除之后，开始研究参数自身问题，使用`json.stringify()`格式化参数之后，依旧不见效。

3. qs.stringify()

之前没有接触过qs.stringify，发现他与json.stringify转换成最后的格式并不相同，所以对参数进行qs.stringify(data)之后，即使不设置headers也没有出现问题。成功。

![ ](http://p09xm7bj0.bkt.clouddn.com/%E5%BE%AE%E4%BF%A1%E5%9B%BE%E7%89%87_201808101637201.png)

4. 两者区别

```javascript
var a = {name:'hehe',age:10};

JSON.stringify(a) // "{"a":"hehe","age":10}"

qs.stringify(a) // name=hehe&age=10
```

总结：(1)axios在使用post方法出现400问题时，不能直接传递一个js对象，而需要通过qs.stringify()将参数格式转换一下，注意：不能使用json.stringify（）。

(2)axios请求头是随你的请求方式改变而改变的。