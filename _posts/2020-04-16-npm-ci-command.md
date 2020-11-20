---
date: 2020-04-16 09:55
layout: post
title: npm-ci命令行
subtitle: 详解npm ci命令行
description: 详解npm ci命令行
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3623108049,2020416334&fm=26&gp=0.jpg
category: blog
tags:
  - npm
author: fg411
---

　　刷微博，刷到阮一峰的微博提到`npm ci`，出于好奇，查了一下，在这记录下来

## 用途

　　`npm ci` 和 `npm i` 类似，都可以用来安装依赖。他比常规的 `npm i` 安装快，也比常规安装更严格，他可以npm依赖安装的一致和稳定 (锁版本)

　　在`package.json`中，每次install后，对应的版本前都有个 `^` 符号。在这种情况下，你再次install时安装的包的版本可能与前次不一样。具体的，你可以到 `package-lock.json` 中查看实际的包版本

　　`^`的匹配规则是：>= 当前版本，且保持从左至右的第一个非零版本。举例说明：

```
"^1.2.3": 大于等于 1.2.3 且小于 2.0.0版本
"^0.3.4": 大于等于 0.3.4 且小于 0.4.0版本
"^0.0.6": 大于等于 0.0.6 且小于 0.0.7版本
```

　　若我们一直使用install命令时，便会遇到开发和测试、发布时包版本不同的问题，这种细微的差别往往会导致严重的结局

## 用法

　　在 `npm i(install)` 的地方改用 `npm ci`，当然项目中必须有一个 `package-lock.json` 或 `npm-shrinkwrap.json`

　　注：npm版本要 >= 5.7

## 与`npm i`的区别

- `npm i`依赖`package.json`，而`npm ci`依赖`package-lock.json`, 项目里面必须存在 `package-lock.json` 或 `npm-shrinkwrap.json`
- 当`package-lock.json`中的依赖于`package.json`不一致时，`npm ci`会报错并且退出， 而不是更新 `package-lock.json`
- `npm ci`只可以一次性的安装整个项目依赖，但无法添加单个依赖项
- `npm ci`安装包之前，如果 `node_modules` 文件夹存在，它会在安装依赖之前删除这个文件夹
- npm安装时，不会修改 `package.json` 与 `package-lock.json`

　　所以，提交Git时，`package-lock.json`不能再忽略提交了。自从见过别人把`package-lock.json`忽略提交，我就没再提交过 `package-lock.json`。 真是 好的不学坏的学。o(╥﹏╥)o

------

## 资料

 - [npm ci命令](https://www.zhuyuntao.cn/npm-ci%E5%91%BD%E4%BB%A4)
 - [npm ci vs. npm install — 在 Node.js 项目中你需要使用哪个](https://www.codercto.com/a/92251.html)
