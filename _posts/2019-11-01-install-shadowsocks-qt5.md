---
date: 2019-11-01 10:30:00
layout: post
title: Raspberry安装shadowsocks-qt5
subtitle: 安装shadowsocks-qt5，实现科学上网
description: 科学上网从shadowsocks开始，搭建一个ss客户端
image: http://pic1.win4000.com/wallpaper/2017-12-15/5a33962b679e0.jpg
optimized_image: http://pic1.win4000.com/wallpaper/2017-12-15/5a33962b679e0.jpg
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
