---
layout: post
title: "VUE+SSR课程学习笔记"
date: 2018-10-18
tags: [vue]
comments: true
---

### vue+webpack工程流搭建

#### 新建以下目录：
1. build/webpack.config.base.js

这个js是放所有环境中webpack配置用到的共同部分

2. webpack.config.client.js

依赖于webpack.config.base.js文件，扩展配置文件方式:
```
npm i webpack-merge -D
```
在client中引入,作用是非常合理的去帮助我们合并webpack的不同配置，类似于object.assign，
```javascript
const merge = require('webpack-merge')

config = merge(baseConfig, {
    devtool: '#cheap-module-eval-source-map',
    module: {
        rules: [
        {
            test: /\.styl/,
            use: [
            'vue-style-loader',
            'css-loader',
            {
                loader: 'postcss-loader',
                options: {
                sourceMap: true
                }
            },
            'stylus-loader'
            ]
        }
        ]
    },
    devServer,
    plugins: defaultPluins.concat([
        new webpack.HotModuleReplacementPlugin(),
        new webpack.NoEmitOnErrorsPlugin()
    ])
})
```
安装`npm i vue-style-loader -D`使页面中的样式改变时，也会热更新。 

安装`npm i rimraf -D`使得每次打包的时候都先删除之前的dist目录，安装完成之后修改package.json
```json
 "scripts": {
    "build:client": "cross-env NODE_ENV=production webpack --config build/webpack.config.client.js",
    "build:server": "cross-env NODE_ENV=production webpack --config build/webpack.config.server.js",
    "build": "npm run clean && npm run build:client && npm run build:server && npm run upload",
    "practice": "cross-env NODE_ENV=development webpack-dev-server --config build/webpack.config.practice.js",
    "clean": "rimraf public && rimraf server-build",
  },
```

### vue+vue-router+vuex项目架构




### vue服务器渲染深度集成