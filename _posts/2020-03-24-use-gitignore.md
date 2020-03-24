---
date: 2020/3/24 15:44
layout: post
title: Git忽略规则及.gitignore规则正确姿势
subtitle: Git忽略规则及.gitignore规则正确姿势
description: 常用Git的你知道.gitignore规则如何使用么？
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1585046300475&di=33775a448662f82603f8874e01eacd3f&imgtype=0&src=http%3A%2F%2F00.minipic.eastday.com%2F20171003%2F20171003064839_0420469b50ba2f0fbd35788b1e12cf22_1.jpeg
category: blog
tags:
  - Git
  - gitignore
author: fg411
---


## 实现需求
在git中如果想忽略掉某个文件或者文件夹，不想这个文件或者文件夹提交到版本库中，可以使用修改根目录中 .gitignore 文件的方法（如无，则需自己手工建立此文件）。这个文件每一行保存了一个匹配的规则例如：

## 创建gitignore文件

```
touch .gitignore
```
### 注释Git忽略规则
```
# 此为注释 – 将被 Git 忽略
 
*.a       # 忽略所有 .a 结尾的文件
!lib.a    # 但 lib.a 除外
/-liberxuesite     # 仅仅忽略项目根目录下的 liberxuesite 文件，不包括 subdir/liberxuesite
liberxue/    # 忽略 liberxue文件夹/ 目录下的所有文件以及文件夹本身
doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt
```
### gitignore忽略规则不生效原因

规则很简单，不做过多解释，但是有时候在项目开发过程中，突然心血来潮想把某些目录或文件加入忽略规则，按照上述方法定义后发现并未生效，原因是.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交：

```
git rm -r --cached .
git add .
git commit -m 'update .gitignore'

```
