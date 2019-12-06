---
date: 2019-11-08 14:30:00
layout: post
title: 浏览器的同源策略与跨域
subtitle: 一个前后端分离的项目基本都会遇到跨域问题，在这里就了解一下为何会存在跨域并实现跨域访问
description: 同源政策（same-origin policy）是浏览器安全的基石
image: https://gss0.bdstatic.com/-4o3dSag_xI4khGkpoWK1HF6hhy/baike/c0%3Dbaike80%2C5%2C5%2C80%2C26/sign=a81a4c8e39fa828bc52e95b19c762a51/060828381f30e924acbd54ba46086e061d95f724.jpg
optimized_image: http://img.zcool.cn/community/01ff9d55420cb90000019ae9aff6a0.jpg@1280w_1l_2o_100sh.jpg
category: http
tags:
  - http
  - 同源策略
  - 跨域
  - JSONP
  - CORS
author: fg411
---

　　1995年，同源政策由 Netscape 公司引入浏览器。目前，所有支持JavaScript的浏览器都都会使用这个策略。"同源政策"（same-origin policy）是浏览器安全的基石

------

### 1 概述

#### 1.1 含义

　　所谓同源是指，域名，协议，端口相同

#### 1.2 目的

　　同源政策的目的，是为了保证用户信息的安全，防止恶意的网站窃取数据

#### 1.3 缺点和权衡

　　如果严格的遵循同源策略，会面临很多的问题。比如，图片，css，js等都得从同域名网站下去获取，个人网站，小网站这样是没问题的。但是对于用户量很大的网站，显然对服务器的压力将会很大，图片等大文件都会占用服务器的带宽

　　不要安全不行，不要性能也不行。在安全和性能上的考虑，使得现代浏览器在安全性和可用性之间选择了一个平衡。所以，现在的浏览器，对于一些资源标签，都开了后门权限。比如，`img` `script` `style`等标签，都允许垮域引用资源

　　正因为跨域访问的存在，web世界才能更加的精彩

#### 1.4 限制范围

　　随着互联网的发展，"同源政策"越来越严格。目前，如果非同源，共有三种行为受到限制

 * Cookie、LocalStorage 和 IndexDB 无法读取
 * DOM 无法获得
 * AJAX 请求不能发送

　　同源策略下的web世界, 域的壁垒高筑, 从而保证各个网页相互独立, 互相之间不能直接访问, `iframe`, `ajax` 均受其限制, 而`script`标签不受此限制

**iframe限制**

  * 可以访问同域资源, 可读写
  * 访问跨域页面时, 只读

**Ajax限制**

  * 同域资源可读写
  * 跨域请求会直接被浏览器拦截(chrome下跨域请求不会发起, 其他浏览器一般是可发送跨域请求, 但响应被浏览器拦截)

**Script限制**

　　script并无跨域限制, 这是因为`script`标签引入的文件不能够被客户端的 `js` 获取到, 不会影响到原页面的安全, 因此`script`标签引入的文件没必要遵循浏览器的同源策略. 相反, `ajax` 加载的文件内容可被客户端 `js` 获取到, 引入的文件内容可能会泄漏或者影响原页面安全, 故 `ajax` 必须遵循同源策略

------

### 2 跨域访问

 * document.domain
 * window.name
 * window.postMessage
 * Access Control
 * JSONP
 * CORS 跨域访问

#### 2.1 document.domain

　　Cookie 是服务器写入浏览器的一小段信息，只有同源的网页才能共享。但是，两个网页一级域名相同，只是二级域名不同，浏览器允许通过设置document.domain共享 Cookie

　　这种方法只适用于 Cookie 和 iframe 窗口，LocalStorage 和 IndexDB 无法通过这种方法，规避同源政策，而要使用下文介绍的PostMessage API

#### 2.2 window.name

　　浏览器窗口有window.name属性。这个属性的最大特点是，无论是否同源，只要在同一个窗口里，前一个网页设置了这个属性，后一个网页可以读取它

　　这种方法的优点是，window.name容量很大，可以放置非常长的字符串；缺点是必须监听子窗口window.name属性的变化，影响网页性能

#### 2.3 window.postMessage

　　上面两种方法都属于破解，HTML5为了解决这个问题，引入了一个全新的API：跨文档通信 API（Cross-document messaging）。

　　这个API为window对象新增了一个window.postMessage方法，允许跨窗口通信，不论这两个窗口是否同源

　　通过window.postMessage，读写其他窗口的 LocalStorage 也成为了可能


#### 2.4 Access Control

　　此跨域方法目前只在很少的浏览器中得以支持, 这些浏览器可以发送一个跨域的HTTP请求（Firefox, Google Chrome等通过XMLHTTPRequest实现, IE8下通过XDomainRequest实现）, 请求的响应必须包含一个Access- Control-Allow-Origin的HTTP响应头, 该响应头声明了请求域的可访问权限

#### 2.5 JSONP

　　JSONP是服务器与客户端跨源通信的常用方法。最大特点就是简单适用，老式浏览器全部支持，服务器改造非常小

　　它的基本思想是，网页通过添加一个`<script>`元素，向服务器请求JSON数据，这种做法不受同源政策限制；服务器收到请求后，将数据放在一个指定名字的回调函数里传回来

#### 2.6 CORS 跨域访问

　　跨来源资源共享 - CORS（`Cross-origin resource sharing`），亦译为跨域资源共享。相比 `JSONP` 只能发 `GET` 请求，`CORS` 允许任何类型的请求.上述的 `JSONP`, `postMessage` 等, 资源本身没有能力保证自己不被滥用. `CORS`的目标是保护资源只被可信的访问源以正确的方式访问

------

### 3 参考资料

 * [浏览器同源政策及其规避方法](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)
 * [由同源策略到前端跨域](https://juejin.im/post/58f816198d6d81005874fd97#heading-7)
 * [前端跨域解决方案](https://segmentfault.com/a/1190000012256432?utm_source=weekly&utm_medium=email&utm_campaign=email_weekly)
