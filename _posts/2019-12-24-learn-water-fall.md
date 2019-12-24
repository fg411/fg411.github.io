---
date: 2019-12-24 18:00:00
layout: post
title: jQuery实现瀑布流
subtitle: 带你五分钟写出自己的瀑布流
description: 带你五分钟写出自己的瀑布流，包教包会
image: http://pic1.win4000.com/wallpaper/f/544a04c831e05.jpg
optimized_image: http://pic1.win4000.com/wallpaper/f/544a04c831e05.jpg
category: javascript
tags:
  - 瀑布流
  - jQuery
author: fg411
---

　　瀑布流，又称瀑布流式布局。是比较流行的一种网站页面布局，视觉表现为参差不齐的多栏布局，随着页面滚动条向下滚动，这种布局还会不断加载数据块并附加至当前尾部

## 瀑布流的排序规律
 - 每张图片的宽度是一致的
 - 从左往右进行排序，第一排放不下则从第二排从新开始排列
 - 第二排或者往后的每排图片放置的位置为上一排高度最小图片的下面依次排放

## 瀑布流实现的原理

1、图片的位置摆放
 - 图片位置的摆放大致可以不添加样式一次放置
 - 通过设置成块级元素，进行摆放
 - 通过绝对定位来进行摆放

2、 瀑布流图片/div容器的实现是通过对其进行绝对定位来实现的
 - 假设X轴列有4张图片，宽度为100px; 那第一张left 值为0，第二张left值为100px，第三张为200px,第四张为300px
 - 图片的高度，我们需要通过对其图片的高度进行判断，让第二横对的图片首先放到 X 列高度最小的图片下面(如图所示，一横列为4张图片，第五张图片放在第二横列中，寻找到第二列的图片高度最低，就将图片当道第二列中。第6张图片寻找到第四列的高度低就将图片放到第四列中，下面每次都是这样)
 - 因为图片的高度很重要，我们需要保存并进行比较，所以我们要用一个数组来接受图片的高度


```html
<!DOCTYPE html>
<html>
<head>
    <title>瀑布流</title>
    <script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
    <style type="text/css">
    .waterfall{
        max-width: 860px;
        margin: 0 auto;
        position: relative;
    }
    .waterfall img{
        width: 100px;
        margin: 10px;
        position: absolute;
        transition: all .4s;
    }
</style>
</head>

<body>
    <div class="waterfall">
        <img src="http://via.placeholder.com/100x100" alt="300*40">
        <img src="http://via.placeholder.com/100x80" alt="300*100">
        <img src="http://via.placeholder.com/100x150" alt="300*100">
        <img src="http://via.placeholder.com/100x140" alt="300*100">
        <img src="http://via.placeholder.com/100x120" alt="300*100">
        <img src="http://via.placeholder.com/100x110" alt="300*100">
        <img src="http://via.placeholder.com/100x160" alt="300*100">
        <img src="http://via.placeholder.com/100x30" alt="300*100">
        <img src="http://via.placeholder.com/100x50" alt="300*100">
        <img src="http://via.placeholder.com/100x90" alt="300*100">
        <img src="http://via.placeholder.com/100x20" alt="300*100">
        <img src="http://via.placeholder.com/100x60" alt="300*100">
        <img src="http://via.placeholder.com/100x120" alt="300*100">
        <img src="http://via.placeholder.com/100x180" alt="300*100">
        <img src="http://via.placeholder.com/100x200" alt="300*100">
        <img src="http://via.placeholder.com/100x125" alt="300*100">
        <img src="http://via.placeholder.com/100x70" alt="300*100">
        <img src="http://via.placeholder.com/100x120" alt="300*100">
        <img src="http://via.placeholder.com/100x40" alt="300*100">
        <img src="http://via.placeholder.com/100x140" alt="300*100">
    </div>
    <script type="text/javascript">
        var colCount; // 列数
        var colHeightArray; // 保存每列高度的数组
        var imgWidth = $('.waterfall img').outerWidth(true);
        // outerWidth outerWidth() 方法返回第一个匹配元素的外部宽度。该方法包含 padding 和 border。提示：如需包含 margin，请使用 outerWidth(true)
        
        init_cols(); // 设置 colHeightArray 默认数据

        // 当图片加载完成后我们在对其进行排序
        $('.waterfall img').on('load', function() {
            set_position(this)
            
        })

        function init_cols(){ //重置 colHeightArray 数据
            colHeightArray = [];
            colCount = Math.floor($('.waterfall').width() / imgWidth);
            for (var i = 0; i < colCount; i++) {
                colHeightArray[i] = 0;
            }
        }

        function set_position(that) { // 对图片进行排序
            var minValue = colHeightArray[0];
            var minIndex = 0;
            for(let i =0 ; i < colCount; i++) {
                if(colHeightArray[i] < minValue) {
                    minValue = colHeightArray[i];
                    minIndex = i;
                }
            }
            $(that).css({
                left: minIndex * imgWidth,
                top: minValue
            })
            colHeightArray[minIndex] += $(that).outerHeight(true);
        }

        $(window).on('resize', function() {
            init_cols();
            $('.waterfall img').each(function() {
                set_position(this);
            })
        })
    </script>
</body>
</html>
```