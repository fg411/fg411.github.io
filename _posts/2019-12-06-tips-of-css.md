---
date: 2019-12-06 15:00:40
layout: post
title: CSS小技巧
subtitle: 
description: CSS小技巧
image:  https://segmentfault.com/img/bVZwyL?w=900&h=385
optimized_image:  https://segmentfault.com/img/bVZwyL?w=900&h=385
category: http
tags:
  - css
  - http-proxy
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