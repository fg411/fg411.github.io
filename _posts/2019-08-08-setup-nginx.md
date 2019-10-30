---
date: 2019-10-30 17:30:00
layout: post
title: nginx编译安装
subtitle: nginx编译安装、启动与配置
description: nginx入门指北，简单介绍nginx的编译安装、启动与配置虚拟站点
image: https://res.cloudinary.com/dm7h7e8xj/image/upload/v1559825288/theme17_nlndhx.jpg
optimized_image: https://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_380/v1559825288/theme17_nlndhx.jpg
category: nginx
tags:
  - nginx
  - 反向代理
author: fg411
---

　　Debian系统`apt-get`安装Linux系统简单，为啥还要编译安装呢，因为编译安装优点是可以修改编译参数啊！

## 准备

　　需安装 `gcc`、`openssl`、`zlib`、`pcre`

　　对于 `gcc`，因为安装nginx需要先将官网下载的源码进行编译，编译依赖gcc环境，如果没有gcc环境的话，需要安装gcc

　　对于 `pcre`，`prce`(Perl Compatible Regular Expressions)是一个Perl库，包括 perl 兼容的正则表达式库。nginx的http模块使用pcre来解析正则表达式，所以需要在linux上安装pcre库

　　对于 `zlib`，`zlib`库提供了很多种压缩和解压缩的方式，`nginx`使用`zlib`对http包的内容进行gzip，所以需要在linux上安装zlib库

　　对于 `openssl`，`OpenSSL` 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。nginx不仅支持http协议，还支持https（即在ssl协议上传输http），所以需要在linux安装openssl库


添加用户 `nginx`
```shell
>groupadd -r nginx
>useradd -r -g nginx nginx
```

解压 `nginx`
``` shell
>tar -zxvf nginx-1.14.0.tar.gz
```

进入 解压后的文件夹
```shell
>cd nginx-1.14.0
```

## 编译安装 `nginx`

------

```shell
>./configure
```

`./configure` 后可添加nginx配置参数，如 `--prefix=/usr/local/nginx` 等

```shell
>make
```
也可运行 `make -j4` 或者 `make -j8`

make -j带一个参数，进行并行编译，一般以CPU的核心数目的两倍，这样可以有效利用CPU资源

```shell
>make install
```

------

如果整个过程中没有报错，此时已成功安装 `nginx`。此时，在 `/usr/local/nginx` 下有四个主要的目录：

　　1、'conf'：保存nginx所有的配置文件，其中nginx.conf是nginx服务器的最核心最主要的配置文件，其他的.conf则是用来配置nginx相关的功能的，例如fastcgi功能使用的是fastcgi.conf和fastcgi_params两个文件，配置文件一般都有个样板配置文件，是文件名.default结尾，使用的使用将其复制为并将default去掉即可。

　　2、'html'：保存了nginx服务器的web文件，但是可以更改为其他目录保存web文件,另外还有一个50x的web文件是默认的错误页面提示页面。

　　3、'logs'：保存nginx服务器的访问日志错误日志等日志，logs目录可以放在其他路径，比如/var/logs/nginx里面。

　　4、'sbin'：保存nginx二进制启动脚本，可以接受不同的参数以实现不同的功能。

## `nginx` 的常用命令语句

```
nginx -t            // 测试配置是否正确
nginx -s reload     // 加载最新配置
nginx -s stop       // 立即停止
nginx -s quit       // 优雅停止
nginx -s reopen     // 重新打开日志
```

## `nginx` 编译配置参数

``` shell
>./configure  --prefix=/usr/local/nginx  --sbin-path=/usr/local/nginx/sbin/nginx --conf-path=/usr/local/nginx/conf/nginx.conf --error-log-path=/var/log/nginx/error.log  --http-log-path=/var/log/nginx/access.log  --pid-path=/var/run/nginx/nginx.pid --lock-path=/var/lock/nginx.lock  --user=nginx --group=nginx --with-http_ssl_module --with-http_stub_status_module --with-http_gzip_static_module --http-client-body-temp-path=/var/tmp/nginx/client/ --http-proxy-temp-path=/var/tmp/nginx/proxy/ --http-fastcgi-temp-path=/var/tmp/nginx/fcgi/ --http-uwsgi-temp-path=/var/tmp/nginx/uwsgi --http-scgi-temp-path=/var/tmp/nginx/scgi --with-pcre
```

## `nginx` 配置参数详解（参考）


```
#user  nobody;
worker_processes  1;    #默认启动进程数目

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;

events {
    worker_connections  1024;   #设置nginx可以接受的最大并发
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    #开启高效传输模式。
    sendfile        on;
    #防止网络阻塞
    tcp_nopush on;
    tcp_nodelay on;

    #keepalive_timeout  0;
    keepalive_timeout  65;  #长连接超时时间，单位是秒

    #gzip  on;
    #负载均衡
    upstream my_pdi_pool {
        server 192.168.7.156:8081;
        server 192.168.7.157:8081;
    }
    upstream my_web_pool {
        ip_hash;
        server 192.168.7.156:8091;
        server 192.168.7.157:8091;
    }

    #反向代理
    server {
        listen       80;
        server_name  localhost;

        location / {
            proxy_pass   http://my_web_pool;
        }
    }

    server {
        listen       8080;
        server_name  localhost;

        location / {
            proxy_pass   http://my_pdi_pool;
        }
    }
    #PDI
    server {
        listen       8081;
        server_name  localhost;

        #charset koi8-r;

        access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```

## 参考：

--------

1. [Nginx 之一：编译安装nginx 1.8.1 及配置](https://www.cnblogs.com/zhang-shijie/p/5294162.html)
2. [Nginx(1.14.0)负载均衡+Tomcat(8.0)集群搭建](https://blog.csdn.net/weixin_38371113/article/details/81671819)
3. [Nginx（一）------简介与安装](https://www.cnblogs.com/ysocean/p/9384877.html)