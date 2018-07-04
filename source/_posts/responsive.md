---
layout: post
title: "响应式开发"
date: 2017-7-25
tags: [html,css]
comments: true
toc: true
---
响应式设计：能够根据视口变化控制页面文档流，简单的来说就是针对任意设备对网页内容进行完美布局的一种显示机制。
媒体查询：在不改变页面内容的情况下，为特定的一些输出设备定制显示效果。

### 视口与屏幕尺寸

视口：浏览器窗口内的内容区域，不包含工具栏、标签栏等。
屏幕尺寸：设备的物理显示区域。

### 媒体查询的特性
1. width：视口宽度
2. height：视口高度
3. device-width：设备屏幕的宽度
4. device-height：设备屏幕的高度
5. orientation：检查设备处于横向还是纵向
6. aspect-ratio：基于视口宽度和高度的比
7. device-aspect-ratio：基于设备渲染平面宽度和高度的宽高比
8. color：每种颜色的位数
9. color-index：设备的颜色索引表中的颜色数，值为非负整数
10. monochrome：检测单色帧缓冲区中没像素所使用的位数，值为非负整数
11. resolution：用来检测屏幕或者打印机的分辨率
12. scan：电视机的扫描方式，值为：progressive(逐渐扫描)、interlace(隔行扫描)
13. grid：用来检测输出设备是网格设备还是位图设备

媒体查询可使用min和max来创建一个查询范围，除了scan和grid之外。

### 渐进增强与优雅降级

优雅降级：指的是为现代浏览器制作网站，然后保证为某些老版本浏览器提供基本可用的
体验。新特性在老版本浏览器中会降级，且一般会有一个分界点，声明不支持那些老掉
牙的浏览器。有些时候用户也仅会被警告他们所使用的浏览器有问题，建议其更换（如
“您的浏览器老得让人笑话——建议下载最新版浏览器！”）

渐进增强：与优雅降级恰好相反。渐进增强以恪守 Web标准的标签为基础，意味着它在所
有浏览器中均可用。然后通过 CSS 样式和必要的 JavaScript 来为更先进的浏览器提供渐
进式的增强体验

### px em与rem
px：像素，用px设置字体大小的时候，优点是精确，但是不支持浏览器缩放和移动端的兼容。因为有些手机屏幕太大啦，而像素却是固定的，所以有了em(相对值)
em：根据父元素来对应字体大小，麻烦的是每次都要找他父级元素的值，于是有了rem
rem：根据根元素html的font-size来设置字体大小。

但是IE8及以下都不支持em与rem属性，解决办法是px与rem一起使用，以达到兼容效果。例如：
p {font-size:14px; font-size:.875rem;}

在线转换工具：[http://pxtoem.com/](http://pxtoem.com/)

```javascript
//使用js判断设备宽度以改变文字大小
(function (doc, win) {
    var docEl = doc.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        recalc = function () {
            var clientWidth = docEl.clientWidth;
            if (!clientWidth) return;
            docEl.style.fontSize = 50 * (clientWidth / 375) + 'px';
        };
    if (!doc.addEventListener) return;

    win.addEventListener(resizeEvt, function() {
        clearTimeout(tid);
        tid = setTimeout(recalc, 300);
    }, false);
    win.addEventListener('pageshow', function(e) {
        if (e.persisted) {
            clearTimeout(tid);
            tid = setTimeout(recalc, 300);
        }
    }, false);


    doc.addEventListener('DOMContentLoaded', function () {
        tid = setTimeout(recalc, 0);
    }, false);

    recalc();

})(document, window);
```