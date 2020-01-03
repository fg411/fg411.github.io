---
date: 2019-11-11 14:00:00
layout: post
title: 更换brew镜像源并安装oh-my-zsh
subtitle: brew命令太慢，修改镜像,顺便安装个oh-my-zsh
description: 修改brew镜像源,安装oh-my-zsh
image: https://res.cloudinary.com/dm7h7e8xj/image/upload/v1559825288/theme17_nlndhx.jpg
optimized_image: https://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_380/v1559825288/theme17_nlndhx.jpg
category: javascript
tags:
  - brew
  - oh-my-zsh 
author: fg411
---

　　`Homebrew` 通过 `Git` 来工作的, 默认的源是 `Github`. 所以`update`,`install`超级慢!

### 更换 brew 镜像源

　　**1.替换 brew.git**
``` shell
> cd "$(brew --repo)"
# 切换到 Homebrew 目录
>git remote set-url origin https://mirrors.aliyun.com/homebrew/brew.git
# 切换成阿里源, 其实就是改了远程仓库的地址
```
　　**替换 homebrew-core.git**
``` shell
>cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
>git remote set-url origin git://mirrors.ustc.edu.cn/homebrew-core.git
```
　　**替换 homebrew-bottles** : 二进制文件
```shell
>echo export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.ustc.edu.cn/homebrew-bottles’ >> ~/.bash_profile
>source ~/.bash_profile
```
　　更换镜像源到这里基本已经完成，可以无障碍运行`brew update`语句了。如果不爽可以随时改回去, 以下是官方源

 - https://github.com/Homebrew/brew.git
 - https://github.com/Homebrew/homebrew-core.git
 - https://github.com/Homebrew/homebrew-cask
 
### 安装 Oh-my-zsh

　　Mac 终端默认 `shell` 为 `bash`，zsh 可能是目前最好的 `shell`，只是配置过于复杂，而 `oh-my-zsh` 只需简单的安装配置

　　**使用 zsh**

　　查看当前使用的 `shell`

```shell
>echo $SHELL

/bin/bash
```

　　查看安装的 `shell`，`shell` 俗称壳，C语言编写的命令解析器程序，是用户使用 `linux` 的桥梁。`Linux` / `Unix` 提供了很多种 `Shell` 。常用的 `Shell` 有这么几种，`sh`、`bash`、`csh`等

```shell
>cat /etc/shells

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
```
　　目前常用的 `Linux` 系统和 `OS X` 系统的默认 `Shell` 都是 `bash`。但是真正强大的 `Shell` 是深藏不露的 `zsh`，但因配置过于复杂，所以初期无人问津。直到有个程序员开发出了一个能快速上手的zsh项目，叫做[「oh my zsh」](https://github.com/robbyrussell/oh-my-zsh)

　　使用 `brew` 安装/更新 `wget` 和 `zsh`

```shell
>brew install wget
>brew install zsh
```

　　切换为 `zsh`

```html
>chsh -s /bin/zsh
```
　　重启终端即可使用 zsh

### 安装 oh-my-zsh

　　执行从 `oh-my-zsh` 的 `GitHub` 下载的安装脚本

```shell
>sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

　　打开 `oh-my-zsh` 配置文件

```shell
>vim ~/.zshrc
```
　　配置项 `ZSH_THEME` 为 `oh-my-zsh` 的主题配置

　　更新配置

```shell
>source ~/.zshrc
```

### 安装自动补齐插件
　　下载[自动补齐插件](https://mimosa-pudica.net/zsh-incremental.html)

```shell
>wget http://mimosa-pudica.net/src/incr-0.2.zsh
```
　　将此插件放到oh-my-zsh目录的插件库下
```shell
> cp ~/incr-0.2.zsh ~/.oh-my-zsh/plugins/incr/incr-0.2.zsh
```
　　修改配置文件
```shell
>vim ~/.zshrc
```
　　在 `plugins` 中添加 `incr`, 在配置文件结束添加内容
```
plugins=(
  git
  incr
)

source $ZSH/plugins/incr/incr*.zsh
```
　　更新配置
```shell
>source ~/.zshrc
```

-------
### 参考
 - [Mac 终端 oh-my-zsh 配置](https://www.jianshu.com/p/64344229778a)
 - [Zsh oh-my-zsh](https://www.jianshu.com/p/959a1fcf05cb)
