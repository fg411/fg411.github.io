---
date: 2019-11-18 18:00:40
layout: post
title: node实现反向代理
subtitle: 
description: node实现反向代理
image:  https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1573551287340&di=a249d3826e7d641eb1fb342e57eab8fd&imgtype=0&src=http%3A%2F%2Fcdn.duitang.com%2Fuploads%2Fitem%2F201204%2F30%2F20120430173137_EVYWR.thumb.700_0.jpeg
optimized_image:  https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1573551287340&di=a249d3826e7d641eb1fb342e57eab8fd&imgtype=0&src=http%3A%2F%2Fcdn.duitang.com%2Fuploads%2Fitem%2F201204%2F30%2F20120430173137_EVYWR.thumb.700_0.jpeg
category: http
tags:
  - node
  - http-proxy
  - 反向代理
author: fg411
---

　　公司在做银行项目，开发都在云桌面。不能直接访问服务器上的接口服务，但是可以连接到云桌面，所以搭建个代理之后就可以愉快的在本地开发啦

　　安装`http-proxy`模块
``` shell
>npm install http-proxy --save-dev
```

　　新建js文件
``` javascript
// server.js
const http = require('http');
const httpProxy = require('http-proxy');

var proxy = httpProxy.createProxyServer({});

proxy.on('error', function(err, req, res){
    res.writeHead(500, {'Content-Type': 'text/plain'});
    res.end('Something went wrong. And we are reporting a custom error message');
})

var server = http.createServer(function(req, res) {
    console.log('connect...');
    proxy.web(req, res, {target: 'http://10.30.30.210:8881'});
})

console.log('listening on port 9999');
server.listen(9999);
```

　　运行js
```
>node server.js
```

　　启动服务后，前端开发的代理地址填写`http://localhost:9999`，访问时会代理请求到`http://10.30.30.210:8881`

------

　　`http-proxy`的功能还有不少，因为暂时只用到这些，所以没有再深入研究，等哪天用到的时候再整理整理吧

　　参考：
 - [http-proxy](https://www.npmjs.com/package/http-proxy)
