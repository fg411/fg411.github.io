---
date: 2020-01-03 16:00:00
layout: post
title: curl 和 wget 的区别和使用
subtitle: curl 和 wget 基础功能有诸多重叠，整理一下
description: curl 和 wget 基础功能的罗列
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046015201&di=a2aac8c9fec3fe96e50f8e67c0b84f5d&imgtype=0&src=http%3A%2F%2Fattach.bbs.miui.com%2Fforum%2F201303%2F21%2F102406y3330470vtfz8y93.jpg
category: tutorial
tags:
  - curl
  - wget 
author: fg411
---

　　非要说区别的话，`curl` 由于可自定义各种请求参数所以在模拟web请求方面更擅长；`wget` 由于支持 `ftp` 和 `Recursive` 所以在下载文件方面更擅长。类比的话 `curl` 是浏览器，而 `wget` 是迅雷

#### 1. 下载文件

```shell
>curl -O http://man.linuxde.net/text.iso        #O大写，不用O只是打印内容不会下载
>wget http://www.linuxde.net/text.iso           #不用参数，直接下载文件
```

#### 2. 下载文件并重命名
```shell
>curl -o rename.iso http://www.linuxde.net/text.iso         #o小写
>wget -O rename.zip http://www.linuxde.net/text.iso         #O大写
```

#### 3. 断点续传
```shell
>curl -O -C - http://man.linuxde.net/text.iso           #O大写，C大写
>wget -c http://www.linuxde.net/text.iso                #c小写
```

#### 4. 限速下载
```shell
>curl --limit-rate 50k -O http://man.linuxde.net/text.iso
>wget --limit-rate=50k http://www.linuxde.net/text.iso
```

#### 5. 显示响应头部信息
```shell
>curl -I http://man.linuxde.net/text.iso
>wget --server-response http://www.linuxde.net/test.iso
```

#### 6. wget利器--打包下载网站
```shell
>wget --mirror -p --convert-links -P /var/www/html http://man.linuxde.net/
```

-----------

　　`wget` 是个专职的下载利器，简单，专一，极致；而 `curl` 可以下载，但是长项不在于下载，而在于模拟提交web数据，`POST`/`GET`请求，调试网页

　　在下载上，也各有所长，`wget` 可以递归，支持断点；而`curl`支持`URL`中加入变量，因此可以批量下载


### curl（文件传输工具）

```shell
-c，–cookie-jar：将cookie写入到文件

-b，–cookie：从文件中读取cookie

-C，–continue-at：断点续传

-d，–data：http post方式传送数据

-D，–dump-header：把header信息写入到文件

-F，–from：模拟http表达提交数据

-s，–slient：减少输出信息

-o，–output：将信息输出到文件

-O，–remote-name：按照服务器上的文件名，存在本地

–l，–head：仅返回头部信息

-u，–user[user:pass]：设置http认证用户和密码

-T，–upload-file：上传文件

-e，–referer：指定引用地址

-x，–proxy：指定代理服务器地址和端口

-w，–write-out：输出指定格式内容

–retry：重试次数

–connect-timeout：指定尝试连接的最大时间/s
```

### `wget`（文件下载工具）

1、 启动参数
```
-V，–version：显示版本号

-h，–help：查看帮助

-b，–background：启动后转入后台执行

```

2、 日志记录和输入文件参数

```
-o，–output-file=file：把记录写到file文件中

-a，–append-output=file：把记录追加到file文件中

-i，–input-file=file：从file读取url来下载
```

3、 下载参数
```
-bind-address=address：指定本地使用地址

-t，-tries=number：设置最大尝试连接次数

-c，-continue：接着下载没有下载完的文件

-O，-output-document=file：将下载内容写入到file文件中

-spider：不下载文件

-T，-timeout=sec：设置响应超时时间

-w，-wait=sec：两次尝试之间间隔时间

–limit-rate=rate：限制下载速率

-progress=type：设置进度条
```

4、目录参数
```
-P，-directory-prefix=prefix：将文件保存到指定目录
```

5、 HTTP参数
```
-http-user=user：设置http用户名

-http-passwd=pass：设置http密码

-U，–user-agent=agent：伪装代理

-no-http-keep-alive：关闭http活动链接，变成永久链接

-cookies=off：不使用cookies

-load-cookies=file：在开始会话前从file文件加载cookies

-save-cookies=file：在会话结束将cookies保存到file文件
```

6、 FTP参数
```
-passive-ftp：默认值，使用被动模式

-active-ftp：使用主动模式
```

7、 递归下载排除参数
```
-A，–accept=list：分号分割被下载扩展名的列表

-R，–reject=list：分号分割不被下载扩展名的列表

-D，–domains=list：分号分割被下载域的列表

–exclude-domains=list：分号分割不被下载域的列表
```

### 参考
 - [curl命令](https://man.linuxde.net/curl)
 - [wget命令](https://man.linuxde.net/wget)
 - [curl和wget的区别和使用](https://www.cnblogs.com/lsdb/p/7171779.html)
