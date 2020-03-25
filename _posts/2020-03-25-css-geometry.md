---
date: 2020-03-25 11:20
layout: post
title: CSS画几何图形
subtitle: CSS画几何图形
description: 使用CSS画一些常见的几何图形
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: http://file02.16sucai.com/d/file/2014/1006/e94e4f70870be76a018dff428306c5a3.jpg
category: blog
tags:
  - css
author: fg411
---

　　一直对css不在行，最近发现CSS可以很简单的画出几何图形，这里选了几个贴上来，想看更多请 [点击这里](http://www.jb51.net/css/41448.html)

```css
/*正方形*/
#square { 
width: 100px; 
height: 100px; 
background: blue; 
}

/*圆形*/
#circle { 
width: 100px; 
height: 100px; 
background: red; 
-moz-border-radius: 50px; 
-webkit-border-radius: 50px; 
border-radius: 50px; 
}

/*椭圆*/
#oval { 
width: 200px; 
height: 100px; 
background: red; 
-moz-border-radius: 100px / 50px; 
-webkit-border-radius: 100px / 50px; 
border-radius: 100px / 50px; 
} 

/*正三角形*/
#triangle-up { 
width: 0; 
height: 0; 
border-left: 50px solid transparent; 
border-right: 50px solid transparent; 
border-bottom: 100px solid red; 
}

/*倒三角*/
#triangle-down { 
width: 0; 
height: 0; 
border-left: 50px solid transparent; 
border-right: 50px solid transparent; 
border-top: 100px solid red; 
} 

/*左三角*/
#triangle-left { 
width: 0; 
height: 0; 
border-top: 50px solid transparent; 
border-right: 100px solid red; 
border-bottom: 50px solid transparent; 
}

/*左上三角*/
#triangle-topleft { 
width: 0; 
height: 0; 
border-top: 100px solid red; 
border-right: 100px solid transparent; 
} 

/*左下三角*/
#triangle-bottomleft { 
width: 0; 
height: 0; 
border-bottom: 100px solid red; 
border-right: 100px solid transparent; 
} 

/*平行四边形*/
#parallelogram { 
width: 150px; 
height: 100px; 
margin-left:20px; 
-webkit-transform: skew(20deg); 
-moz-transform: skew(20deg); 
-o-transform: skew(20deg); 
background: red; 
} 

/*梯形*/
#trapezoid { 
border-bottom: 100px solid red; 
border-left: 50px solid transparent; 
border-right: 50px solid transparent; 
height: 0; 
width: 100px; 
} 

/*五角星*/
#star-five { 
margin: 50px 0; 
position: relative; 
display: block; 
color: red; 
width: 0px; 
height: 0px; 
border-right: 100px solid transparent; 
border-bottom: 70px solid red; 
border-left: 100px solid transparent; 
-moz-transform: rotate(35deg); 
-webkit-transform: rotate(35deg); 
-ms-transform: rotate(35deg); 
-o-transform: rotate(35deg); 
} 
#star-five:before { 
border-bottom: 80px solid red; 
border-left: 30px solid transparent; 
border-right: 30px solid transparent; 
position: absolute; 
height: 0; 
width: 0; 
top: -45px; 
left: -65px; 
display: block; 
content: ''; 
-webkit-transform: rotate(-35deg); 
-moz-transform: rotate(-35deg); 
-ms-transform: rotate(-35deg); 
-o-transform: rotate(-35deg); 
} 
#star-five:after { 
position: absolute; 
display: block; 
color: red; 
top: 3px; 
left: -105px; 
width: 0px; 
height: 0px; 
border-right: 100px solid transparent; 
border-bottom: 70px solid red; 
border-left: 100px solid transparent; 
-webkit-transform: rotate(-70deg); 
-moz-transform: rotate(-70deg); 
-ms-transform: rotate(-70deg); 
-o-transform: rotate(-70deg); 
content: ''; 
} 

/*8角星*/
#burst-8 { 
background: red; 
width: 80px; 
height: 80px; 
position: relative; 
text-align: center; 
-webkit-transform: rotate(20deg); 
-moz-transform: rotate(20deg); 
-ms-transform: rotate(20deg); 
-o-transform: rotate(20eg); 
transform: rotate(20deg); 
} 
#burst-8:before { 
content: ""; 
position: absolute; 
top: 0; 
left: 0; 
height: 80px; 
width: 80px; 
background: red; 
-webkit-transform: rotate(135deg); 
-moz-transform: rotate(135deg); 
-ms-transform: rotate(135deg); 
-o-transform: rotate(135deg); 
transform: rotate(135deg); 
} 

/*食逗人（Pac-Man）*/
#pacman { 
width: 0px; 
height: 0px; 
border-right: 60px solid transparent; 
border-top: 60px solid red; 
border-left: 60px solid red; 
border-bottom: 60px solid red; 
border-top-left-radius: 60px; 
border-top-right-radius: 60px; 
border-bottom-left-radius: 60px; 
border-bottom-right-radius: 60px; 
} 

/*爱心*/
#heart { 
position: relative; 
width: 100px; 
height: 90px; 
} 
#heart:before, 
#heart:after { 
position: absolute; 
content: ""; 
left: 50px; 
top: 0; 
width: 50px; 
height: 80px; 
background: red; 
-moz-border-radius: 50px 50px 0 0; 
border-radius: 50px 50px 0 0; 
-webkit-transform: rotate(-45deg); 
-moz-transform: rotate(-45deg); 
-ms-transform: rotate(-45deg); 
-o-transform: rotate(-45deg); 
transform: rotate(-45deg); 
-webkit-transform-origin: 0 100%; 
-moz-transform-origin: 0 100%; 
-ms-transform-origin: 0 100%; 
-o-transform-origin: 0 100%; 
transform-origin: 0 100%; 
} 
#heart:after { 
left: 0; 
-webkit-transform: rotate(45deg); 
-moz-transform: rotate(45deg); 
-ms-transform: rotate(45deg); 
-o-transform: rotate(45deg); 
transform: rotate(45deg); 
-webkit-transform-origin: 100% 100%; 
-moz-transform-origin: 100% 100%; 
-ms-transform-origin: 100% 100%; 
-o-transform-origin: 100% 100%; 
transform-origin :100% 100%; 
} 

/*钻石*/
#cut-diamond { 
border-style: solid; 
border-color: transparent transparent red transparent; 
border-width: 0 25px 25px 25px; 
height: 0; 
width: 50px; 
position: relative; 
margin: 20px 0 50px 0; 
} 
#cut-diamond:after { 
content: ""; 
position: absolute; 
top: 25px; 
left: -25px; 
width: 0; 
height: 0; 
border-style: solid; 
border-color: red transparent transparent transparent; 
border-width: 70px 50px 0 50px; 
} 

/*阴阳八卦*/
#yin-yang { 
width: 96px; 
height: 48px; 
background: #eee; 
border-color: #444; 
border-style: solid; 
border-width: 2px 2px 50px 2px; 
border-radius: 100%; 
position: relative; 
} 
#yin-yang:before { 
content: ""; 
position: absolute; 
top: 50%; 
left: 0; 
background: #fff; 
border: 18px solid #444; 
border-radius: 100%; 
width: 12px; 
height: 12px; 
} 
#yin-yang:after { 
content: ""; 
position: absolute; 
top: 50%; 
left: 50%; 
background: #444; 
border: 18px solid #eee; 
border-radius:100%; 
width: 12px; 
height: 12px; 
} 
```
