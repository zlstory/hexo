---
layout: post
title: "canvas学习笔记——基础篇"
tags: [canvas]
date: 2019-12-16
comments: true
---


1. 创建canvas以及初始化

```html
<canvas id="canvas"></canvas>
```

```javascript

  var canvas = document.getElementById("canvas")
    if (!canvas.getContext) { //判断浏览器是否支持canvas
        return
    }
    var context = canvas.getContext("2d")
```


2. 绘制线条

```javascript

context.moveTo(x,y) //状态设置

context.lineTo(x,y) //状态设置

context.stroke() //绘制

```
在定义路径的时候如果有多个路径，我们希望他们互不干扰，需要用到

```javascript
context.beginPath() //重新绘制，可以不用closePath结束
context.closePath() //会用一条线将不封闭的首尾连接起来
```

3. 定义样式
```javascript
context.lineWidth //线条的宽度

context.strokeStyle //线条的颜色

context.stroke() //绘制线条

context.fillStyle //填充的颜色

context.fill() //绘制填充颜色
```

先stroke()之后再fill() 填充会将linewidth的一半遮挡住，若先fill再stroke不会有这种问题

4. 绘制弧线

```javascript
context.arc(
    centerx,centery,radius, //圆心坐标以及半径
    startingAngle,endingAngle, //开始与结束弧度值
    anticlockwise = false //可选，顺时针(false)
)

```

5. 刷新画布操作

```javascript
    context.clearRect(0,0,canvas.width,canvas.height)
```

6. 绘制矩形

```javascript
cxt.rect(x,y,width,height)

ctx.fillRect(x,y,width,height) //填充的矩形

ctx.strokeRect(x,y,width,height) //带边框的矩形

```

7. 线条的属性

```javascript
//lineCap：线条两边头突出的形状，默认无
//lineCap:butt,round,square

context.lineCap = "butt"

//lineJoin:线与线之间连接的部分
//lineJoin:miter,bevel,round

//miterLimit:当lineJoin属性是miter时，内角与外角的之间长度是10（默认值），超过limit值是会用beveil形式显示
context.lineJoin = "miter"
context.miterLimit = 10
```
8. 图形变换

```javascript
translate(x,y) //位移

rotate(deg) //旋转

scale(sx,sy) //缩放

```

图形变换会改变其他的图形位置，canvas为我们提供了两个接口用于保存状态和恢复

```javascript
//需成对出现
context.save() //在图形变换前保存状态
context.restore() //返回在save()节点所有的状态

```

9. 变换矩阵
```javascript
transform(a,b,c,d,e,f)
//a,d:水平、垂直缩放
//b,c:水平、垂直倾斜
//e,f:水平、垂直位移

//setTransform会让之前设置的transform失效
setTransform(a,b,c,d,e,f)

```

10. 线性渐变fillStyle

```javascript
var grd = context.createLinearGradient(xstart,ystart,xend,yend)
grd.addColorStop(stop,color)
```
demo:
```javascript
  window.onload = function(){
    var canvas = document.getElementById("canvas")

    canvas.width = 800
    canvas.height = 800
    var context = canvas.getContext("2d")

    var linearGrad = context.createLinearGradient(0,0,800,800)
    linearGrad.addColorStop(0.0,"#fff")
    linearGrad.addColorStop(0.5,"red")
    linearGrad.addColorStop(1.0,"#000")
    
    context.fillStyle=linearGrad
    context.fillRect(0,0,800,800)
}
```

11. 径向渐变Radial Gradient
用法与linearGradient相似
```javascript
var grd = context.createRadialGradient(x0,y0,r0,x1,y1,r1)
grd.addColorStop(stop,color)
```

12. 使用图片、画布或者video:createPattern
```javascript
createPattern(img,repeat-style)
createPattern(canvas,repeat-style)
//repeat-style:no-repeat,repeat-x,repeat-y,repeat

var bg = new Image()
bg.src = "demo.png"

var pattern = context.createPattern(bg,"no-repeat")
context.fillStyle = pattern
context.fillRect(0,0,800,800)

```
13. 绘制圆弧: arcTo（上面有arc的方式）
```javascript
context.moveTo(x0,y0) // 开始点

 context.arcTo(x1,y1,  //控制点
                x2,y2, //结束点
                radius)

```


14. 贝塞尔曲线：

```javascript
//二次贝塞尔曲线
context.moveTo(x0,y0) // 曲线起始点
context.quadraticCurveTo(
    x1,y1, //控制点
    x2,y2 //结束点
)
//三次贝塞尔曲线
context.bezeirCurveTo(
    x1,y1, x2,y2, //控制点
    x3,y3 //结束点
)


```

15. 文字渲染

```javascript
context.font = "bold 30px Arial"
context.fillText(string,x,y,[maxlen])
context.strokeText(string,x,y,[maxlen])

//设置文本水平对齐
context.textAlign = "left" //center right

//设置文本垂直对齐
context.textBaseline = "top" //middle bottom alphabetic(default) ideographic hanging


//获取渲染后的文字度量
context.measureText(string).width //目前只能获取宽度

```








