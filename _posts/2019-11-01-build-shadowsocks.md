---
date: 2019-11-01 10:30:00
layout: post
title: 搭建shadowsocks服务
subtitle: 搭建ss服务，科学网上冲浪，你懂的
description: 科学上网从shadowsocks开始，一起搭建一个ss服务
image: http://b.hiphotos.baidu.com/image/pic/item/0eb30f2442a7d9337119f7dba74bd11372f001e0.jpg
optimized_image: http://b.hiphotos.baidu.com/image/pic/item/0eb30f2442a7d9337119f7dba74bd11372f001e0.jpg
category: tutorial
tags:
  - ubuntu
  - shadowsocks
author: fg411
---

更新软件源
``` shell
sudo apt-get update
```

安装PIP环境
``` shell
sudo apt-get install python-pip
```

安装shadowsocks
``` shell
sudo pip install shadowsocks
```

后台运行
``` shell
sudo ssserver -p 23564 -k password -m rc4-md5 --user nobody -d start
```

端口号：23564
密码：password
加密方法：rc4-md5
