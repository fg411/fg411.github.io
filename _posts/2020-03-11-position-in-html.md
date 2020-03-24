---
date: 2020-03-11 16:50
layout: post
title: 关于html中的position属性
subtitle: 复习一下position的relative和absolute属性
description: 复习一下position的relative和absolute属性
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1583929931789&di=7a0481b19261eaed1db03c155fe579bc&imgtype=0&src=http%3A%2F%2Fn.sinaimg.cn%2Fsinacn13%2F429%2Fw640h589%2F20180611%2F53ab-hcufqif9221154.jpg
category: blog
tags:
  - position
  - relative
  - absolute
author: fg411
---

　　
　　今天按demo写例子，发现`relative`和`absolute`可以配合使用，所以又来复习一下`position`的常用属性:

 - **sticky**：基于用户的滚动位置来定位。也就是以sticky标签定义的标签会随着页面上下移动，但是其内容却不会超过屏幕比如在网站侧边那些移动导航栏
 - **static**：HTML元素的默认值，即没有定位，元素出现在正常的流中。静态定位的元素不会受到 top, bottom, left, right影响。也就是和没写position一样的效果
 - **fixed**：元素的位置相对于浏览器窗口是固定位置。即使窗口是滚动的它也不会移动，相当一个壁纸标签一样一动不动像镶嵌在屏幕里一样，在很多网站尤其是购物网站里面经常能看到，你看到页面侧边那静静躺在那的导航栏就是用fixed实现的
 - **relative**：生成相对定位的元素，相对于其正常位置进行定位；因此，"left:20" 会向元素的 LEFT 位置添加 20 像素
 - **absolute**：生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位；元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定

看完各个属性，上代码
```html
<div class="box">
	<div class="back"></div>
	<div class="modal">
		<span class="desc">
			描述文字
		</span>
	</div>
</div>
<style type="text/css">
.box{
	position: relative;
	height: 200px; width: 300px
}
.back{
	background-image: url(http://www.imaoda.com/s/img/github/21.jpg);
	height: 100%; width: 100%;
}

.modal{
	position:absolute;
	bottom: 0;
	height: 20%; width: 100%;
	background: rgba(0,0,0,.5)
}
.desc{
	 color: white
}
</style>
```

　　`position`:`absolute`; 他的意思是绝对定位，他是参照浏览器的左上角，配合`TOP`、`RIGHT`、`BOTTOM`、`LEFT`(下面简称`TRBL`)进行定位，在没有设定`TRBL`，默认依据父级的做标原始点为原始点。如果设定`TRBL`并且父级没有设定`position`属性，那么当前的`absolute`则以浏览器左上角为原始点进行定位，位置将由`TRBL`决定

　　`position`:`relative`;  他的意思是绝对相对定位，他是参照父级的原始点为原始点，无父级则以BODY的原始点为原始点，配合`TRBL`进行定位，当父级内有`padding`等CSS属性时，当前级的原始点则参照父级内容区的原始点进行定位

　　`relative`没有跳出文本流，定位是相对于父级和兄弟节点。`absolute`跳出文本流，是相对于父级且带有定位，如果父级没有定位属性，那么就会往上一级再找是否带定位，找到顶级之后如果还没有定位，就以`body`定位

**相对定位元素经常被用来作为绝对定位元素的容器块，当标签有嵌套时，子标签的位置样式设置是相对于父标签的。**

------

**参考：**

 - [用最短的CSS样式，勾勒大数据演示屏](https://juejin.im/post/5b1e2b50f265da6e5546c15d)

 - [HTML的relative与absolute区别](https://blog.csdn.net/Toomeout/article/details/81220756)

 - [关于position的relative和absolute分别是相对于谁进行定位的](https://www.cnblogs.com/SimpleGIS/p/11233086.html)
