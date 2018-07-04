---
layout: post
title: "浅谈前端项目模块化工具webpack"
date: 2017-11-30
tags: [webpack]
toc: true
comments: true
---

### webpack简介

webpack是一个模块打包器，他可以将前端中的静态文件根据一定的规则打包成一个或多个文件。简单的来说，webpack就是一个代码打包工具。

### 为什么要使用webpack

随着前端开发的越来越复杂，不可能将所有前端代码写在一个文件中，但是若写在多个文件中会在全局作用域下造成冲突、http请求次数增多且后期维护困难。为了方便管理及组织，我们需要给代码划分不同的模块及文件中，进行模块化开发。

另一方面，前端优化有两大方面：1.减少http请求 2.减小静态文件体积。

webpack可以同时支持CommonJs与AMD规范,不仅是针对于JavaScript，其他文件如css、图片、字体等一切前端文件皆可兼容，且都可以分模块打包(实现了按需加载)，且可以将es6的代码编译成浏览器识别的es5。


### commonJS 与 AMD 规范

#### commonJS模块规范

每一个文件都是一个模块，有着自己的作用域，在文件里定义的变量函数等都是私有的，对其他文件不可见。模块加载的顺序与其在代码中出现的顺序一致，即：不可异步加载。

模块之间必须通过module.exports进行自身文件的暴露，再其他文件中通过require()将暴露的模块引入到当前作用域中。


#### AMD规范

由于commonJS只能同步加载，这样对浏览器来说很不友好，于是AMD定义了一套JavaScript模块以异步加载模块。



### webpack安装

安装webpack需要node支持（前端电脑中应该都有node环境吧！）

```
npm install webpack -g
```

安装完成之后，查看webpack的所有命令。
```
webpack -help
```

cd进入目标文件之后，建立一个package.json文件。

```
npm init
```
![ ](http://p09xm7bj0.bkt.clouddn.com/webpack1.png)

- name：这个包的名字
- version：这个包的版本
- description： 一句话描述这个包是做什么用的
- entry point：入口文件，默认是index.js，你可以写成自己的文件
- test command：测试命令
- git repository：Git仓库地址，没有直接回车
- keyword：作为搜索包的关键词
- author：作者
- license：开源文件

填完这些信息之后，它会问你 Is this OK ？
回车之后，目标文件中就有新增一个package.json，内容就是刚刚填的那些信息。


```javascript

    {
    "name": "zlstory",
    "version": "1.0.0",
    "description": "a webpack demo",
    "main": "index.js",
    "scripts": {
        "test": "echo \"Error: no test specified\" && exit 1"
    },
    "author": "zlstory",
    "license": "ISC"
    }

```

接下来安装webpack的依赖，生成node_modules文件夹

```
npm install webpack --save-dev
```
若要安装指定版本的webpack版本

```
npm install webpack@3.8.1 --save-dev
```

### 使用webpack打包js

#### 单个入口文件

在目标文件夹中建立index.html与entry.js文件，如下图所示：

![ ](http://p09xm7bj0.bkt.clouddn.com/webpack2.png)

在entry.js中

```javascript
document.write("webpack works!");
```

在index.html中
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>webpack 入门</title>
</head>
<body>

    <script src="bundle.js"></script>
    
</body>
</html>
```
注意我们引入的是bundle.js，而并非是新建的entry.js，若直接在浏览器打开index.html一定是报错的，因为找不到bundle.js，那么webpack就是可以将entry.js打包成一个bundle.js，并生成bundle.js在指定目录中。
```
webpack entry.js bundle.js
```
此时在浏览器中打开index.html会发现<span style="font-size:16px;">webpack works!<span>

#### 多个入口文件

通过require()可以将其他文件引入进来，如新建head.js文件。
```javascript
//require之前需要进行接口的暴露
module.exports = "This is head.js";
```
在entry.js中将head.js引入
```javascript
document.write("webpack works!");

console.log(require("./head.js"));
```


重新使用webpack entry.js bundle.js打包可以，成功后打开index.html，可以看出既有<span style="font-size:16px;">webpack works!<span>，也console出了 This is head.js,证明webpack将entry.js以及head.js都打包到了bundle.js中。

将之前生成的bundle.js删除，新建一个webpack.config.js文件：
```javascript
var webpack = require('webpack')
module.exports = {
    devtool: "sourcemap",
    entry: "./js/entry.js",
    output: {
        filename: "bundle.js"
    }
}

```
再使用cmd执行 webpack 命令也会生成bundle.js,同时会生成一个.map文件（错误信息文件）。

### 使用不同的loader处理不同的静态文件

webpack本身只能处理js文件，若需要模块化打包css、图片以及字体等文件，需要加载对应的loader。
loader是一个预处理函数，接受源文件作为参数，返回的是转换的结果。loader为JavaScript生态系统提供了更多的能力，例如压缩、打包、语言翻译等。

如css文件需要加载css-loader或style-loader：
```
npm install css-loader --save-dev

npm install ts-loader --save-dev
```
在webpack.config.js(没有就新建)中指定loader。

```javascript
var webpack = require('webpack')
module.exports = {
    module:{
        rules:[
            { test:/\.css$/, use:"css-loader" },
            { test: /\.ts$/, use: 'ts-loader' }
        ]
    }
}
```
简单实现：

新建css文件，在entry.js中require该css文件，使用webpack entry.js bundle.js命令，发现css文件生效。

当然也可以使用内联方式来指定该文件需要使用什么的loader
如在entry.js中:
```javascript
require("!style-loader!css-loader!./style.css") // 载入 style.css
```


### 使用webpack加载第三方库
```
npm install jquery --save-dev
```
在entry.js中，

```javascript
document.write("webpack works!");

console.log(require("./head.js"))

var $ = require('jquery')

$("h1").html("this is jquery");
```
在index.html中
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8">
    <title>webpack 入门</title>
</head>
<body>
    <h1>this is h1</h1>
    <script src="bundle.js"></script>

</body>
</html>

```
在webpack.config.js中
```javascript
var webpack = require('webpack')
module.exports = {
    devtool: "sourcemap",
    entry: "./entry.js",
    output: {
        filename: "bundle.js"
    }
}

```
使用webpack 命令，打开浏览器即可看见页面中：

![ ](http://p09xm7bj0.bkt.clouddn.com/webpack3.jpg)

### webpack与babel、vue

[babel中文网](https://babeljs.cn/)

[vue官网](https://cn.vuejs.org/)

首先安装相关的loader：

```
 npm install babel-core babel-loader babel-plugin-transform-runtime babel-preset-es2015 babel-preset-stage-0 babel-runtime --save-dev


npm install vue-loader vue-html-loader vue-style-loader --save-dev
```

在webpack.config.js中：
```javascript
var webpack = require('webpack')
module.exports = {
    devtool: "sourcemap",
    entry: "./js/entry.js",
    output: {
        filename: "bundle.js"
    },
    module:{
        loaders:[
            {
                test:/\.css$/,
                loader:"style!css"
            },
            {
                test:/\.js$/,
                loader:"babel",
                exclude:/node_modules/
            },
            {
                test:/\.vue$/,
                loader:"vue"
            }
        ]
    },
    babel:{
        presets:['es2015','stage-0'],
        plugins:['transform-runtime']
    },
        resolve: {//在使用npm构建vue环境的时候，需要在打包工具里配置一个别名
        alias: {
            'vue$': 'vue/dist/vue.esm.js' // 'vue/dist/vue.common.js' for webpack 1
        }
    }
}

```

#### webpack命令

安装全局的webpack服务器
```
npm install -g webpack-dev-server
```
安装项目依赖的服务器
```
npm install webpack-dev-server --save-dev
```
查看所有打包文件以及在什么文件中调用了哪个文件

```
webpack  --display-modules  --display-reasons
```

实时监控
```
webpack -watch

```
热更新：
```
webpack-dev-server --content-base build --inline --hot

```
配置服务器与热更新,在package.json中

```
"scripts": {
    "start": "webpack",
    "build": "webpack-dev-server --content-base build --inline --hot"
}
```

区分线上与线下环境

```javascript
var　debug = process.env.MODE_ENV != "production";

module.exports = {
    devtool: debug ?  "sourcemap" :null,
    entry: "./js/entry.js",
    output: {
        filename: "bundle.js"
    },
    module:{
        loaders:[
            {
                test:/\.css$/,
                loader:"style!css"
            }
        ]
    },
    plugins:debug?[]:[
        new webpack.optiomize.DebugPlugin(),
        new webpack.optiomize.OccurenceOrderPlugin()

    ],
    babel:{
        presets:['es2015','stage-0'],
        plugins:['transform-runtime']
    },
    resolve: {
        alias: {
            'vue$': 'vue/dist/vue.esm.js' // 'vue/dist/vue.common.js' for webpack 1
        }
    }
}
```