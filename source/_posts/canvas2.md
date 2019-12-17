---
layout: post
title: "canvas学习笔记——高级篇"
tags: [canvas]
date: 2019-12-17
comments: true
---

1. 阴影

```javascript

context.shadowColor 

context.shadowOffsetX
context.shadowOffsetY

context.shadowBlur //模糊程度


```

2. globalAlpha(默认为1):透明度设置
```javascript
context.globalAlpha = 0.7
```

3. globalCompositeOperation:互相遮盖时产生的效果（有十一种属性）
```javascript
context.globalCompositeOperation =  "source-over"
```


4. 剪辑区域 clip

```javascript
context.beginPath()
context.fillStyle ="black"
context.fillRect(0,0,canvas.width,canvas.height)           

context.beginPath()
context.arc(400,400,150,0,Math.PI*2)
context.fillStyle ="#fff"
context.fill()
context.clip()

context.font = "bold 100px Arial"
context.textAlign = "center"
context.textBaseline = "middle"
context.fillStyle = "pink"
context.fillText("CANVAS" ,canvas.width/2,canvas.height/2)

```

5. canvas中获取鼠标坐标
```javascript
var x = event.clientX - canvas.getBoundingClientRect().left
var y = event.clientY - canvas.getBoundingClientRect().top

```




