---
layout: post
title: "在vue中使用iframe"
tags: [vue]
date: 2019-10-21
comments: true
---
在vue-cli的项目中使用iframe引入html文件的方法：

1. 将需要引入的html文件及style文件放入在static文件夹中


2. 在vue文件中
```html
<iframe
    :src="iframeUrl"
    width="100%"
    height="440"
    frameborder="0"
    scrolling="auto"
    style="position:absolute;top:100px;left: 0px;"
></iframe>
```
```javascript
data(){
    return {
        iframeUrl:"../../static/demo.html",
    }
}

```