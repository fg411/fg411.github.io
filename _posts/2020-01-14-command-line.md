---
date: 2020-01-06 18:00:00
layout: post
title: Mac上实用的命令行语句
subtitle: 作为一个懒人，常常只想用terminal操作电脑，这里整理一些实用而不常用的命令行语句
description: 作为一个懒人，常常只想用terminal操作电脑，这里整理一些实用而不常用的命令行语句
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: http://picture.ik123.com/uploads/allimg/160506/3-160506140937.jpg
category: javascript
tags:
  - terminal
author: fg411
---

### 查看当前目录下的所有文件（包含文件夹）大小

```shell
>du -hs *
>du -shc *
```

两个命令都可以，第二个能在最后显示一个 `total` 大小，即当前目录的总大小

```shell
>du -sh * | sort -rh
```
显示当前目录下所有文件（包含文件夹）大小，并排序

---------

### 打开文件夹

```shell
>open .
```

### 查看硬盘容量
```shell
>df -h
```

### 查看文件

`tail`命令查看文件的最后十行内容

```shell
>tail -10 filename
```

---------

### 参考
 - [Mac 终端命令大全](https://www.jianshu.com/p/85f048998e92)
