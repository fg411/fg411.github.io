---
date: 2020-01-15 16:00:00
layout: post
title: 用canvas画个闹钟吧
subtitle: 如果你也觉得用JS画闹钟很复杂的话，请过来瞅一瞅，你会发现Wow，so easy
description: 如果你也觉得用JS画闹钟很复杂的话，请过来瞅一瞅，你会发现Wow，so easy
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: https://ss1.bdstatic.com/70cFvXSh_Q1YnxGkpoWK1HF6hhy/it/u=3588512660,3283157211&fm=26&gp=0.jpg
category: blog
tags:
  - canvas
  - clock
author: fg411
---

　　之前想学canvas，今天看到个课程是用canvas画的闹钟，一开始以为会很难，按照课程写了一遍后发现如此简单，下面把 `js` 部分贴上来

```javascript 1.6
var dom = document.getElementById('clock');
var ctx = dom.getContext('2d');
var height = ctx.canvas.height;
var width = ctx.canvas.width;
var r = width / 2; // 计算闹钟半径
var rem = width / 200; // 计算比例，防止放大缩小时变型

function drawBackground() {
	ctx.save()
	ctx.translate(r, r)
	ctx.beginPath()
	ctx.lineWidth = 4 * rem
	ctx.arc(0, 0, r - ctx.lineWidth / 2, 0, 2 * Math.PI)
	ctx.stroke()

	var hourNumber = [3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 1, 2 ]
	ctx.font = 15 * rem + 'px Arial'
	ctx.textAlign = 'center'
	ctx.textBaseline = 'middle'
	hourNumber.forEach(function(hour, i) {
		var rad = 2 * Math.PI / 12 * i
		var x = Math.cos(rad) * (r - 24 * rem)
		var y = Math.sin(rad) * (r - 24 * rem)
		ctx.fillText(hour, x, y)
	})

	for (var i = 0; i < 60; i++) {
	    var rad = 2 * Math.PI / 60 * i
		var x = Math.cos(rad) * (r - 12 * rem)
		var y = Math.sin(rad) * (r - 12 * rem)
		ctx.beginPath()
		if(i % 5 === 0) {
			ctx.fillStyle = '#333'
			ctx.arc(x, y, 2 * rem, 0, 2 * Math.PI)
		} else {
			ctx.fillStyle = '#ccc'
			ctx.arc(x, y, rem, 0, 2 * Math.PI)
		}
		ctx.fill()
	}
}

function drawHour(hour, minute){
	ctx.save()
	ctx.beginPath();
	var rad = hour * 2 * Math.PI / 12
	var mrad = minute *2 * Math.PI / 12 / 60
	ctx.rotate(rad + mrad)
	ctx.lineWidth = 3 * rem
	ctx.lineCap = 'round'
	ctx.moveTo(0, 10 * rem)
	ctx.lineTo(0, 10 - r / 2)
	ctx.stroke()
	ctx.restore()
}

function drawMinute(minute) {
	ctx.save()
	ctx.beginPath();
	var rad = minute * 2 * Math.PI / 60
	ctx.rotate(rad)
	ctx.lineWidth = 2 * rem
	ctx.lineCap = 'round'
	ctx.moveTo(0, 15 * rem)
	ctx.lineTo(0, 45 * rem - r)
	ctx.stroke()
	ctx.restore()
}

function drawSecond(second) {
	ctx.save()
	ctx.beginPath();
	ctx.fillStyle = '#c14543'
	var rad = second * 2 * Math.PI / 60
	ctx.rotate(rad)
	ctx.moveTo(-2 * rem, 20 * rem)
	ctx.lineTo(2 * rem, 20 * rem)
	ctx.lineTo(1 * rem, 30 * rem - r)
	ctx.lineTo(-1 * rem, 30 * rem - r)
	ctx.fill()
	ctx.restore()
}

function drawDot() {
	ctx.beginPath()
	ctx.fillStyle = '#fff'
	ctx.arc(0, 0, 3 * rem, 0, 2 * Math.PI, false)
	ctx.fill()
}

function draw(){
	ctx.clearRect(0, 0, width, height)
	drawBackground()
	var now = new Date()
	var hour = now.getHours()
	var minute = now.getMinutes()
	var second = now.getSeconds()
	drawHour(hour, minute)
	drawMinute(minute)
	drawSecond(second)
	drawDot()
	ctx.restore()
}

draw()
setInterval(draw, 1000)
```
---------

```html
<canvas id="clock" height="100px" width="100px"></canvas>
```

　　本想整理代码中使用到的`canvas`方法，奈何太多，就算了

---------

### 参考：

- [HTML5 <canvas> 参考手册](https://www.runoob.com/tags/ref-canvas.html)
