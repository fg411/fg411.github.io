---
date: 2020-03-09 15:40:00
layout: post
title: 罗列一些常见的页面布局.md
subtitle: 常见的页面布局你知道几个？
description: 常见的页面布局你知道几个？
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1583843054234&di=dd8592aebe0534e386d90900abc4fc4f&imgtype=0&src=http%3A%2F%2Fpic1.win4000.com%2Fwallpaper%2Fc%2F583b9bbdaa9e9.jpg
category: blog
tags:
  - 圣杯布局
  - 双飞翼布局
  - flex布局
author: fg411
---

 - 圣杯布局
 - 双飞翼布局
 - flex 布局
 
　　昨天看博客时无意间发现`圣杯布局`和`双飞翼布局`，知(gu)识(lou)渊(gua)博(wen)的我赶紧学习一下

------

　　圣杯布局和双飞翼布局是前端工程师需要日常掌握的重要布局方式。两者的功能相同，都是为了实现一个*两侧宽度固定，中间宽度自适应的三栏布局*。

　　圣杯布局是为了讨论「三栏液态布局」的实现，最早的完美实现是由 Matthew Levine 在 2006 年写的一篇文章 《In Search of the Holy Grail》 ，而双飞翼布局来源于淘宝UED。虽然两者的实现方法略有差异，不过都遵循了以下要点：
*   两侧宽度固定，中间宽度自适应
*   中间部分在DOM结构上优先，以便先行渲染
*   允许三列中的任意一列成为最高列
*   只需要使用一个额外的`<div>`标签

------

 **圣杯布局**
```html
<body>
    <div id="hd">header</div>
    <div id="bd">
      <div id="middle">middle</div>
      <div id="left">left</div>
      <div id="right">right</div>
    </div>
    <div id="footer">footer</div>
</body>

<style>
#hd{
    height:50px;
    background: #666;
    text-align: center;
}
#bd{
    /*左右栏通过添加负的margin放到正确的位置了，此段代码是为了摆正中间栏的位置*/
    padding:0 200px 0 180px;
    height:100px;
}
#middle{
    float:left;
    width:100%;/*左栏上去到第一行*/
    height:100px;
    background:blue;
}
#left{
    float:left;
    width:180px;
    height:100px;
    margin-left:-100%;
    background:#0c9;
    /*中间栏的位置摆正之后，左栏的位置也相应右移，通过相对定位的left恢复到正确位置*/
    position:relative;
    left:-180px;
}
#right{
    float:left;
    width:200px;
    height:100px;
    margin-left:-200px;
    background:#0c9;
    /*中间栏的位置摆正之后，右栏的位置也相应左移，通过相对定位的right恢复到正确位置*/
    position:relative;
    right:-200px;
}
#footer{
    height:50px;
    background: #666;
    text-align: center;
}
</style>
```

------

**双飞翼布局**
```html
<body>
<div id="hd">header</div> 
  <div id="middle">
    <div id="inside">middle</div>
  </div>
  <div id="left">left</div>
  <div id="right">right</div>
  <div id="footer">footer</div>
</body>

<style>
#hd{
    height:50px;
    background: #666;
    text-align: center;
}
#middle{
    float:left;
    width:100%;/*左栏上去到第一行*/     
    height:100px;
    background:blue;
}
#left{
    float:left;
    width:180px;
    height:100px;
    margin-left:-100%;
    background:#0c9;
}
#right{
    float:left;
    width:200px;
    height:100px;
    margin-left:-200px;
    background:#0c9;
}

/*给内部div添加margin，把内容放到中间栏，其实整个背景还是100%*/ 
#inside{
    margin:0 200px 0 180px;
    height:100px;
}
#footer{  
   clear:both; /*记得清楚浮动*/  
   height:50px;     
   background: #666;    
   text-align: center; 
} 
</style>
```

------
**flex 布局**

```html
<body>
  <div id="header">header</div>
  <div id="container">
    <div id="left">This is the left sidebar.</div>
	<div id="middle">This is the main content.</div>
	<div id="right">This is the right sidebar.</div>
	</div>
	<div id="footer">footer</div>
</body>

<style type="text/css">
#header, #footer{
	border: 1px solid #333;
    background: #ccc;
    text-align: center;
}
#container {
	display: flex;
}
#left{
	width: 200px;
	background-color: #8F8;
}
#middle{
	flex: 1;
	background-color: #88F;
}
#right{
	width: 150px;
	background-color: #F88;
}
</style>
```

------

　　总结一下，圣杯布局在DOM结构上显得更加直观和自然，且在日常开发过程中，更容易形成这样的DOM结构（通常`<aside>`和`<article>`/`<section>`一起被嵌套在`<main>`中）；而双飞翼布局在实现上由于不需要使用定位，所以更加简洁；flex则最简单

------
**参考：**
-[圣杯布局和双飞翼布局的理解与思考](https://www.jianshu.com/p/81ef7e7094e8)
-[前端面试每日 3+1（每日三问）](https://github.com/haizlin/fe-interview)
-[如何实现一个圣杯布局？](https://segmentfault.com/a/1190000017540629)
