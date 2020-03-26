---
date: 2020-03-25 11:20
layout: post
title: var、let和const的区别
subtitle: var、let和const
description: 整理一下`var`、`let`和`const`之间的区别
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: http://01.minipic.eastday.com/20170721/20170721212631_c195b10d28c04da33503cf28bb45b34e_15.jpeg
category: blog
tags:
  - js
  - let
  - var
  - const
author: fg411
---

## let、var和const的区别

　　现在讨论一下`ES5`的`var`和`ES6`的`let`、`const`

### 为什么会有`let` 和 `const`

　　ES5只有 全局作用域 和 函数作用域，有时遇到一些不合理的场景，比如遇到下面的代码，大部分人会选择使用闭包来解决这个问题。而`ES6`引入的`let`可以完美的解决这个问题。至于如何使用闭包实现`let`功能，可以看[这里](https://fg411.github.io/learn-es6-let/)，有错误的话，还望指正。

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

### var、let、const 三个命令

　　`var` 命令
 - 定义变量
 - 函数作用域
 - 会变量提升

　　`let` 命令
 - 定义变量
 - 块级作用域
 - 不会变量提升
 - 存在暂时性死区

　　`const` 命令
 - 定义常量
 - 块级作用域
 - 不会变量提升
 - 存在暂时性死区
 - 定义时需赋值

------

### 全局作用域

　　我们在全局范围内声明一个变量时，这个变量自动成为全局对象的属性（在浏览器和Node.js环境下，这个全局对象分别是window和global)，但let是独立存在的变量，不会成为全局对象的属性

```javascript
var a = 1;
console.log(window.a); //1

let b = 2;
console.log(window.b); // undefined
// 不管let 还是 var 放到外边，都是全局作用域
```

### 函数作用域

　　函数作用域也叫局部作用域，局部（函数内）声明的变量拥有函数作用域

```javascript
function myFunction() {
  var carName = "porsche";
  console.log(carName)
  // code here CAN use carName
}
```

### 块级作用域
```javascript

if(1) {
    var a = 123
}
console.log(a) // 123

if(1) {
    let b = 123
}
console.log(b) // Uncaught ReferenceError: b is not defined

let c = 1
if(c) {
    let c = 321
    console.log(c)
}
console.log(c)
```

### 变量提升的区别

　　如果使用var声明的变量在函数里面，则将变量声明提升到函数的开头

```javascript
if(1){
    console.log(tmp);
    var tmp = 123
} // undefined


if(1){
    console.log(tmp);
    let tmp = 123
} // Uncaught ReferenceError: Cannot access 'tmp' before initialization
```

### 暂时性死区(temporal dead zone)

```javascript
if (true) {
  // TDZ开始
  tmp = 'abc'; // ReferenceError
  console.log(tmp); // ReferenceError

  let tmp; // TDZ结束
  console.log(tmp); // undefined

  tmp = 123;
  console.log(tmp); // 123
}
```

### 常量 const

```javascript
const PI = 3.1415926
console.log(PI)     // 3.1415926
PI = 3              // Uncaught TypeError: Assignment to constant variable.
console.log(PI)
```

　　`const` 声明一个只读的常量。一旦声明，常量的值就不能改变

```javascript
const tmp // Uncaught SyntaxError: Missing initializer in const declaration
tmp = 1234
console.log(tmp)
```

　　`const`声明的变量不得改变值，这意味着，`const`一旦声明变量，就必须立即初始化，不能留到以后赋值

```javascript
const d = {
  name:'allen'
}
d.name = 'zhaosi'
console.log(d)    // {name: "zhaosi"}
```

　　`const`只是修饰栈空间的地址不可更改，并没有修饰堆空间的值不可修改

------

### 总结

　　*变量提升*：`let`和`const`没有变量提升，在代码块内，声明之前是不可用的（暂时性死区）；而`var`申明的变量会存在变量提升的，在变量申明前使用会提示`undefined`

　　*作用域*：`let`和`const`是块级局部变量，只在当前代码块起作用；使用`var`关键字声明的变量不具备块级作用域的特性，它在 {} 外依然能被访问到。
　　*`const` 特殊性*：`const`用于声明常量，普通数据类型的值是无法改变的，但如果是一个引用数据类型，只要内存地址不变，对象中的值是可以变化的

　　其实这总结的很简单，一开始我也觉的内容很少，不需要仔细研究，当我看到别人博客里的代码块才发现，简单的知识点要扎实，平时要仔细看，仔细想，仔细研究，不然等哪天面试时，只能靠猜咯

### 别人的例子

```javascript
console.log(a)    // ƒ a(){}
var a = 1
console.log(a)    // 1
function a(){}
console.log(a)    // 1
```
------

## 资料
 - [let 和 const 命令](https://es6.ruanyifeng.com/#docs/let)
 - [var与let，const的区别详解](https://blog.csdn.net/qq_39009348/article/details/93042516)
 - [总结下var、let 和 const 的区别](https://segmentfault.com/a/1190000016491581)
