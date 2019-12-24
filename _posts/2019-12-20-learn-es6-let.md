---
date: 2019-12-20 16:00:00
layout: post
title: ES6中let的实现
subtitle: 还只会用let，不好奇怎么实现的么？好奇就一起来看看吧
description: 还只会用let，不好奇怎么实现的么？好奇就一起来看看吧
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1576840386413&di=065306b0c63868bcb6b7fed421d7e4f3&imgtype=0&src=http%3A%2F%2Fpic.gamersky.com%2Fdown%2Findex%3Furl%3Dhttps%3A%2F%2Fimg1.gamersky.com%2Fupimg%2Fpic%2F2018%2F09%2F30%2F201809301147136080.jpg
optimized_image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1576840386413&di=065306b0c63868bcb6b7fed421d7e4f3&imgtype=0&src=http%3A%2F%2Fpic.gamersky.com%2Fdown%2Findex%3Furl%3Dhttps%3A%2F%2Fimg1.gamersky.com%2Fupimg%2Fpic%2F2018%2F09%2F30%2F201809301147136080.jpg
category: javascript
tags:
  - var
  - let
  - 变量提升
author: fg411
---

　　之前看到过一篇博客写`ES6`中`let`的实现，没太在意，最近闲来搜了一下也看了几篇博客，在这贴一些代码

　　`ES6`出来之前，作为菜鸟的我遇到下面这种，就会抱怨`JS`真是满满的坑

```javascript
`use strict`
var a = []

for(var i = 0;i<5;i++){
    a[i] = function(){
        console.log(i)
    }
}

for (var k of a){
    k();
}

// 我们想在 console.log 中打印 0 ~ 4，而事实打印出来的却是 5 个 5
```

　　现在使用 `ES6` 的 `let` 就可以了
```javascript
var a = []

for(let i = 0;i<5;i++){
    a[i] = function(){
        console.log(i)
    }
}

for (var k of a){
    k();
}
```

　　为什么`let`可以很轻松的达到呢，如果不使用`let`那可以用什么别的方法吗？如下：

```javascript
`use strict`
var a = [];

for(var i = 0; i < 5; i++) {
    _loop(i)
}

function _loop(i) {
    a[i] = function(){
        console.log(i)
    }
}

for(var k of a) {
    k()
}
```
　　这是使用`babel`将上面`let`方法`polyfill`的版本，用以支持不支持`ES6`语法的浏览器。可以发现，是使用了一个函数将i的值存储下来达到块级作用域的目的。这样临时的变量`i`就得以保存下来

　　*使用 `try catch`方法*

```javascript
`use strict`

var a = []

for(var i = 0; i < 5; i++) {
    try {
        throw i
    } catch(e) {
        a[e] = function(){
            console.log(e)
        }
    }
}

for (var k of a){
    k();
}

```

　　*使用自执行函数的方法*

```javascript
`use strict`

var a = []

for(var i = 0; i < 5; i++) {
    (function(i){
        a[i] = function() {
            console.log(i)
        }
    })(i)
}

for (var k of a){
    k();
}
```
　　*使用`map`函数*

```javascript
`use strict`

var a = []

for(var i = 0; i < 5; i++) {
    [i].map((value) => {
        a[value] = function() {
            console.log(value)
        }
    })
}

for (var k of a){
    k();
}
```

　　可以发现，在这些方法里，除了`try catch`以外都是使用函数的作用域来保存`i`的值

-----

　　上面介绍是在循环中使用的实例，那么在非循环中使用又是怎样的实现方法呢

```javascript
var name = 'World!';

(function(){
    if(typeof name === 'undefined') {
        var name = 'Kit'
        console.log('Goodbye, ' + name)
    } else {
        console.log('Hello, ' + name)
    }
})()
```

　　答案是 `Goodbye Jack`。首先JS引擎拿到这段代码会先进行变量提升，实际上的顺序应该是这样的

```javascript
`use strict`
var name;
name = 'World!';

(function(){
    var name;
    if(typeof name === 'undefined') {
        name = 'Kit'
        console.log('Goodbye, ' + name)
    } else {
        console.log('Hello, ' + name)
    }
})()
```

>外面的name实际上没有什么作用，主要是if里面的这个name。在某些编程语言里面，if实际上也是一个块级作用域，在块级作用域里面声明的变量，外面是不可以使用的。但是在JS里面，除了函数具备这个功能外再没有块级作用域了。所以在if里面声明的name会被提升到函数最顶部先行执行，然后判断name是不是undefined类型，因为没有赋值所以答案是true。所以name = Jack所以打印出Goodbye Jack

　　如果题目是以下，答案又是什么呢

```javascript
`use strict`
var name = 'World!';

(function(){
    if(typeof name === 'undefined') {
        let name = 'Kit'
        console.log('Goodbye, ' + name)
    } else {
        console.log('Hello, ' + name)
    }
})()
```
　　因为使用的是`let`声明，没有变量提升。所以if判断里的name引用的是全局变量name，所以答案是`Hello World`

　　简单的`polyfill`版本实现 `let`

```javascript
`use strict`
var name = 'World'
(function(){
    if(typeof name === 'undefined') {
        (function() {
            var name = 'Kit'
            console.log('Goodbye, ' + name)
        })()
    } else {
        console.log('Hello, ' + name)
    }
})()
```

　　可以看到，`let`的出现简化了大量的JS操作。特有的作用域弥补的JS块级作用域的短板。针对`let`的`polyfill`也多是使用函数的作用域来完成，由此可见函数对于JS的重要性

-----

参考：
 - [ES6之let声明的实现](https://blog.csdn.net/qq_32758013/article/details/79574558)
