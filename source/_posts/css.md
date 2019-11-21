---
layout: post
title: "常用css汇总"
tags: [vue]
date: 2018-9-11
comments: true
---

## 改变placeholder颜色
```css
input::-webkit-input-placeholder {
    color:red;
}

input::-moz-placeholder {
    /* Mozilla Firefox 19+ */
    color:red;
}

input:-moz-placeholder {
    /* Mozilla Firefox 4 to 18 */
    color:red;
}

input:-ms-input-placeholder {
    /* Internet Explorer 10-11 */
    color:red;
}
```
## 替换背景图改变checkbox样式
```html
<label>
    <input type="checkbox" checked="">
    <i class="spot"></i>我已阅读并接受《芒果用户服务协议》
</label>
```
```css
label{
display:inline-block;
width:100%;
height:50px;
line-height:50px;
text-align:left;
position: relative;
}
label input{
width: 24px;
height: 24px;
vertical-align: bottom;
margin-right:10px;
opacity: 0;
}
.spot{
display:inline-block;
width:24px;
height:24px;
background:url("../images/uncheck.png") no-repeat;  /*未选中的样式图片*/
background-size:24px;
position: absolute;
top:12px;
left:0;
z-index:5;
cursor: pointer;
}
input:checked + .spot{
background:url("../images/checked.png") no-repeat;  /*选中后的样式图片*/
background-size:24px;
}
```
