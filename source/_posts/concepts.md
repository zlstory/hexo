---
layout: post
title: " 模块化"
tags: [理论]
date: 2019-3-13
comments: true
---


#### 为什么需要模块化：

首先前端复杂程度有限，没有模块也会死可以的。但是 2009 年出现 node.js，将 JavaScript 语言用于服务端编程，在服务端一定要有模块来与操作系统和其他应用程序互动，node 编程中的核心思想就是模块，由此模块化编程在 js 中流行。

#### 模块化分类

在 es6 以前，通用的 JavaScript 模块规范有两种：CommonJS 和 AMD

1. CommonJS 规范（服务端模块）

代表：requireJS、seaJS

加载模块：require()

暴露模块：module.exports 和 exports

如使用 math.js

```javascript
//加载
var math = require("math");
//调用
math.add(1, 2);
```

commonJS 是用于服务端编程，所有的模块都放在本地硬盘中，可以同步加载完成，等待的时间就是硬盘读取的时间。

2. AMD 规范（客户端模块）

因为模块放在服务端，commonJS 不支持异步操作，所以并不适用于浏览器环境。所以出现了 AMD 规范：异步模块定义。

代表：require.js和curl.js

```javascript
//公式
define(id?, dependencies?, factory)
```

定义模块：define()

载入模块：dependencies(数组格式)

工厂方法：factory(返回模块函数)

```javascript
//不依赖其他模块：直接定义

define(function（）{
    var add = function (x,y){
        return x+y
    }
    return {
        add:add
    }
})


// 依赖其他模块：define()第一个参数为数组格式，指定所需模块
define(['Lib'],function(lib){
    function foo(){
        Lib.doSomething()
    }
    return {
        foo:foo
    }
})

// 使用require加载模块
require(["math"],function(math){
    math.add(1,2)
})

```

3. CMD规范（客户端模块）

代表：seajs

特点：依赖就近

```javascript
//公式
define(id?, dependencies?, factory)
```

定义模块：define()

载入模块：dependencies(数组格式)

工厂方法：factory(返回模块函数)

```javascript
//直接定义
define(function(require,exports,module){

})

```

4. AMD与CMD的异同
 
 异：对依赖模块的执行时机处理不同
    
 AMD依赖前置，js可以知道模块是谁，立即加载；CMD就近依赖，需要把模块转换成字符串再解析一边才知道依赖哪些模块。

 同：都是异步加载模块


5. module（ES6发布之后）

引入模块：import

导出模块：export
