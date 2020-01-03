---
date: 2019-12-06 15:00:40
layout: post
title: CSS小技巧
subtitle: 
description: CSS小技巧
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046601374&di=1539b226782b2dd2a3e4c629cf4da227&imgtype=0&src=http%3A%2F%2Fpic1.win4000.com%2Fwallpaper%2Fd%2F5513a960ca8ce.jpg
optimized_image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046601374&di=1539b226782b2dd2a3e4c629cf4da227&imgtype=0&src=http%3A%2F%2Fpic1.win4000.com%2Fwallpaper%2Fd%2F5513a960ca8ce.jpg
category: http
tags:
  - css
  - clip-path
  - shape-outside
author: fg411
---

## 用 `font-size: 0` 来清除间距

　　`inline-block` 的元素之间会受空白区域的影响，也就是元素之间差不多会有一个字符的间隙。如果在同一行内有4个25%相同宽度的元素，会导致最后一个元素掉下来。你可以利用元素浮动 `float`，或者压缩html，清除元素间的空格来解决。但最简单有效的方法还是设置父元素的 `font-size` 属性为 0

``` css
*{
  box-sizing: border-box;
}

.items {
  font-size: 0;
}

.items > .item {
    display: inline-block;
    width: 25%;
    height: 50px;
    border: 1px solid #ccc;
    text-align: center;
    line-height: 50px;
    background-color: #eee;
    font-size: 16px; // 不要忘了给子元素设置字号
  }
```

``` html
<div class="items">
    <div class="item">1</div>
    <div class="item">2</div>
    <div class="item">3</div>
    <div class="item">4</div>
</div>
```
------

## 用 `border` 绘制三角形

``` css
.border-arrow {
 width: 0;
 height: 0;
 border: 72px solid ;
 border-color : transparent transparent transparent orange ;
}
```
------

参考:
 - [你不知道的CSS](https://segmentfault.com/a/1190000010993048)
 - [神奇的 CSS](https://segmentfault.com/a/1190000012242526?utm_source=weekly&utm_medium=email&utm_campaign=email_weekly)
 - [奇妙的 CSS shapes(CSS图形)](https://segmentfault.com/p/1210000009779482/read)
