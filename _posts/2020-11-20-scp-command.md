---
date: 2020/11/20 9:22
layout: post
title: scp命令
subtitle: 使用scp命令上传下载文件
description: 使用scp命令上传下载文件
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3623108049,2020416334&fm=26&gp=0.jpg
category: blog
tags:
  - Linux
  - Scp
author: fg411
---


# 语法

``` shell
>scp [可选参数] file_source file_target 
```

**参数说明**

```
-1： 强制scp命令使用协议ssh1
-2： 强制scp命令使用协议ssh2
-4： 强制scp命令只使用IPv4寻址
-6： 强制scp命令只使用IPv6寻址
-B： 使用批处理模式（传输过程中不询问传输口令或短语）
-C： 允许压缩。（将-C标志传递给ssh，从而打开压缩功能）
-p： 保留原文件的修改时间，访问时间和访问权限。
-q： 不显示传输进度条。
-r： 递归复制整个目录。
-v： 详细方式显示输出。scp和ssh(1)会显示出整个过程的调试信息。这些信息用于调试连接，验证和配置问题。
-c： cipher： 以cipher将数据传输进行加密，这个选项将直接传递给ssh。
-F： ssh_config： 指定一个替代的ssh配置文件，此参数直接传递给ssh。
-i： identity_file： 从指定文件中读取传输时使用的密钥文件，此参数直接传递给ssh。
-l： limit： 限定用户所能使用的带宽，以Kbit/s为单位。
-o： ssh_option： 如果习惯于使用ssh_config(5)中的参数传递方式，
-P： port：注意是大写的P, port是指定数据传输用到的端口号
-S： program： 指定加密传输时所使用的程序。此程序必须能够理解ssh(1)的选项
```

 - 从本地复制到远程

``` shell
# 复制文件命令格式
>scp local_file remote_username@remote_ip:remote_folder 
>scp local_file remote_username@remote_ip:remote_file
>scp local_file remote_ip:remote_folder
>scp local_file remote_ip:remote_file

# 复制目录命令格式
scp -r local_folder remote_username@remote_ip:remote_folder 
scp -r local_folder remote_ip:remote_folder 
```

　　第1，2个指定了用户名，命令执行后需要再输入密码，第1个仅指定了远程的目录，文件名字不变，第2个指定了文件名

　　第3，4个没有指定用户名，命令执行后需要输入用户名和密码，第3个仅指定了远程的目录，文件名字不变，第4个指定了文件名

　　第5个指定了用户名，命令执行后需要再输入密码；第5个没有指定用户名，命令执行后需要输入用户名和密码

 - 从远程复制到本地

　　从远程复制到本地，只要将从本地复制到远程的命令的后2个参数调换顺序即可
