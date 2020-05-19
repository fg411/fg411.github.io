---
date: 2020/05/19 11:50
layout: post
title: tree常用命令
subtitle: 查询目录结构么，那就试试tree命令吧
description: 查询目录结构么，那就试试tree命令吧
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1589869677949&di=ed0473f18c6a5f780563b58cb0bc4787&imgtype=0&src=http%3A%2F%2Fcdn.duitang.com%2Fuploads%2Fblog%2F201404%2F07%2F20140407230129_FPMFN.jpeg
category: blog
tags:
  - tree
author: fg411
---


## `Mac`安装

``` shell
>brew install tree
```

## 实用命令

　　我们可以在目录遍历时使用 -L 参数指定遍历层级。例：显示项目三层结构，`tree -l 3`

``` shell
>tree -L n
```

　　如果你想把一个目录的结构树导出到文件 Readme.md ,可以这样操作

``` shell
>tree -L 2 >README.md //然后我们看下当前目录下的 README.md 文件
```

　　只显示文件夹

``` shell
>tree -d
```

　　`tree -I pattern` 用于过滤不想要显示的文件或者文件夹。比如要过滤项目中的node_modules文件夹

``` shell
>tree -I "node_modules"
```

------

## 资料

 - [Mac下的 tree 命令 输出目录树层结构](https://www.jianshu.com/p/9411d60950bf)
