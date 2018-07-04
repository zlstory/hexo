---
layout: post
title: "居中问题"
date: 2018-1-15
tags: [css]
comments: true
---

在有浮动时，使用定位解决div居中问题。

<!--more-->

```html
<style type="text/css" media="screen">
    .container {
        float: left;
        width: 100%;
        overflow: hidden; position:
        relative;padding-bottom: 10px;
        border: 1px solid #eee;
    }
    .container ul {
        clear: left;
        float: left;
        position: relative;
        left: 50%;/*整个分页向右边移动宽度的50%*/
        text-align: center;
    }
    .container ul li {
        margin: 0 5px;
        display: block;
        float: left;
        position: relative;
        right: 50%;/*将每个分页项向左边移动宽度的50%*/
    }
    .box{width: 70px;height: 40px;background: #eee;border: 1px solid #ccc;}
    .box1{width: 30px;height: 30px;background: #eee;border: 1px solid #ccc;}
    .box1{width: 50px;height: 50px;background: #eee;border: 1px solid #ccc;}

</style>

<body>

    <div  class="container">
        <ul>
            <li><div class="box"> </div></li>
            <li><div class="box1"> </div></li>
            <li><div class="box2"> </div></li>
            <li><div class="box1"> </div></li>
            <li><div class="box"> </div></li>
            <li><div class="box2"> </div></li>
        </ul>
    </div>

</body>

```

