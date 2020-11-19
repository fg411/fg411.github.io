---
date: 2020/11/19 17:26
layout: post
title: Less命令
subtitle: 使用Less命令查看文件
description: 使用Less命令查看文件
image: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3623108049,2020416334&fm=26&gp=0.jpg
optimized_image: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3623108049,2020416334&fm=26&gp=0.jpg
category: blog
tags:
  - fg411
author: fg411
---


　　我们常用`tail -n`  或者 `tail -f` 或者 `grep` 或者 `vi` 、`cat` 等各种命令去查看异常信息，记下`less`  的使用方法

------

#### 基操

 - 第一步：打开日志文件
``` shell
>less sigma.log
```
 - 第二步：定位到日志文件的最后一行（ `shift+g` 可移动到最后一行）
 - 第三步：`ctrl+b` 往前一页一页翻页查看

------

#### 常见使用方法

**搜索**

当使用命令 `less file-name` 打开一个文件后,可以使用下面的方式在文件中搜索。搜索时整个文本中匹配的部分会被高亮显示


 - 1.1 向前搜索 `/` : 使用一个模式进行搜索，并定位到下一个匹配的文本（按 `n` : 向前查找下一个匹配的文本；按 `N` : 向后查找前一个匹配的文本）

 - 1.2 向后搜索 `?` : 使用模式进行搜索,并定位到前一个匹配的文本（`n` : 向后查找下一个匹配的文本；`N` : 向前查找前一个匹配的文本）

**全屏导航**

> `ctrl + F` - 向前移动一屏
> 
> `ctrl + B` - 向后移动一屏
>
> `ctrl + D` - 向前移动半屏
>
> `ctrl + U` - 向后移动半屏

**单行导航**

>` j`  - 向前移动一行
>
>`k` - 向后移动一行

**其它导航**

>`G` - 移动到最后一行
>
>`g` - 移动到第一行
>
>`q` / `ZZ`  - 退出 less 命令

**编辑文件**

v : 进入编辑模式,使用配置的编辑器编辑当前文件

**动态查看**

　　在 Linux 动态查看日志文件常用的命令非 `tail -f` 莫属,其实 `less` 也能完成这项工作,使用 `F` 命令。使用 `less file-name` 打开日志文件,执行命令 `F`，可以实现类似 `tail -f` 的效果

****
