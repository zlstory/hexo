---
layout: post
title: "ES6中一不注意就掉进去的那些坑"
date: 2018-4-11
tags: [es6]
comments: true
---

### 块级作用域

1.使用let、const与var的区别为前两者声明的变量不会被提升到作用域顶部

```javascript
alert(typeof value2) //undefined

if(true){
    alert(typeof value1) //undefined
    alert(typeof value2) //value2 is not defined

    var value1 = 111
    let value2 = 222
}
```

2.使用const声明时，不允许修改绑定，但是允许修改绑定的值。

```javascript
const person = {
    name : "Crystal",
    age : 24
}

//修改绑定的值
person.name = "小懒"


//直接修改绑定会抛出语法错误
person = {
    name : "Crystal",
    age : 24
}
//VM60:1 Uncaught TypeError: Assignment to constant variable. at <anonymous>:1:8

```

3.在循环中创建的函数,使用var时，我们期望着输出0-9，但是最后只输出10。因为循环中的每次迭代都同时共享着变量i，循环内部引用的都是相同变量。而使用let每次循环都会创建一个新的i。此种情况下使用const时将在第二次赋值时报错。

```javascript
for( let i = 0; i < 10; i++){
  setTimeout(function() {
        console.log(i)  //0-9
    })
}

for( var i = 0; i < 10; i++){
  setTimeout(function() {
        console.log(i)  //10
    })
}

for( var i = 0; i < 10; i++){
  (function(val){
        return function(){
            console.log(val)
        }
    }(i)()) //结果为：0-9  使用闭包解决
}

```

4.var会覆盖已存在的全局变量，let与const不会覆盖全局变量，只能遮盖它;var创建新的全局变量时会将此作为window对象的属性，let与const不会将声明的变量添加为全局对象的属性,即不会破坏全局作用域。

```javascript

var Number = 123
console.log(Number)//123
console.log(window.Number)//123

let Number = 123
console.log(Number)//123
console.log(window.Number)//ƒ Number() { [native code] }


```

### 
