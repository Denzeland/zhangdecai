---
title: canvas绘图api
date: 2019-10-17 16:54:51
tags:
- 前端开发
---
## canvas绘图api

canvas元素自身的宽高和绘图区域的宽高是不同的概念。需要注意的是width和height这两个属性，用css设置的宽高实质上是设置元素自身的宽高，而通过元素对象设置的这两个属性会同时设置自身的宽高和绘图区域的宽高。元素`var canvas = document.getElementById('canvas')`有两个属性和三个方法，列出如下：

- width： 绘图区域的宽度
- height：绘图区域的高度
- getContext(): 返回绘图环境对象，参数'2d'和'3d'
- toDataUrl(type, quality): 返回dataUrl， type为图片类型，quality为0~1之间整数
- toBlob(callback, type, quality): 表示canvas元素图像文件的blob，浏览器会以指向blob的引用去调回调函数，type为图片类型， quality为0~1之间整数

绘图环境`var context = canvas.getContext('2d')`对象的api:

属性：
- canvas: 获取关联的canvas元素
- fillstyle：绘图填充使用的颜色、图案
- font: 
- globalAlpha
- globalCompsiteOperation
- lineCap
- lineWidth
- lineJoin
- miterLimit
- shadowBlur
- shadowColor
- shadowOffsetX
- shadowOffsetY
- strokeStyle
- textAlign
- textBaseline

状态保存与恢复：
- save()
- restore()

绘图方法：
- arc()
- beginPath()
- clearRect()
- fill()
- fillText()
- lineTo()
- moveTo()
- stroke()
