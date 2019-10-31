---
date: 2019-10-31 18:00:00
layout: post
title: 使用 sendemail 发送邮件
subtitle: 使用 sendemail 发送天气信息到网易企业邮箱
description: 使用 sendemail 发送天气信息到网易企业邮箱
image: http://i0.hdslb.com/bfs/article/861103ff9e37eae23f4144c74b1974d77969d87b.jpg
optimized_image: http://i0.hdslb.com/bfs/article/861103ff9e37eae23f4144c74b1974d77969d87b.jpg
category: linux
tags:
  - 树莓派
  - sendemail
  - 和风天气
author: fg411
---

　　看树莓派的书，发现可以使用 **sendemail** 发送天气信息，闲来无事，折腾一波

## 获取天气数据
　　在这里可以使用[和风天气开发者](https://dev.heweather.com/) ，注册并获取 `key`

　　进入开发文档，可以使用获取常规天气数据的接口（免费版）

>https://free-api.heweather.net/s6/weather/{weather-type}?{parameters}

　　`{weather-type}` 代表不同的天气数据类型，必选。使用以下值：

weather-type 值     | 描述
-------- | -----
now |  实况天气
forecast |  3-10天预报
hourly  |  逐小时预报
lifestyle |  生活指数

　　`{parameters}` 代表请求参数，包括必选和可选参数。所有请求参数使用 & 进行分隔，参数值存在中文或特殊字符的情况，需要对参数进行 url encode

parameters 值     | 描述  | 描述
-------- | ----- | -----
location |  需要查询的城市或地区（经纬度 / 城市名 [拼音、汉字] /IP 等）  | 必选
lang |  多语言，可以不使用该参数，默认为简体中文  | 选填
unit |  单位选择，公制（m）或英制（i），默认为公制单位 | 选填
key |  用户认证key | 必填

　　具体请自行查看和风天气API

　　API 返回的数据是 json 字符串，需要对数据进行解析，这里可以安装`jq`工具

```shell
> sudo apt-get install jq -y
```

　　以下是获取天气数据的代码

```bash
#!/bin/bash
# Weather Data
CITY=hefei
TOKEN=和风KEY
WEATHER=$(curl "https://free-api.heweather.net/s6/weather/lifestyle?location=${CITY}&key=${TOKEN}")
SUGGESTIONS=$(echo ${WEATHER} | jq -r '.HeWeather6[0].lifestyle| values[].txt')
echo ${SUGGESTIONS}
```

## 安装 sendemail

　　可以直接使用`apt-get`安装`sendemail`，并很有可能需要安装一些依赖

```shell
> sudo apt-get install sendemail -y
> sudo apt-get install libio-socket-ssl-perl libnet-ssleay-perl -y
```

## 发送email
```bash
#!/bin/bash
#Email Send Test
SERVER="smtp.sina.com:22"
FROM=""
TO=""
SUBJECT="test"
MESSAGE="test_content"
CHARSET="utf-8"
USERNAME=""
PASSWORD=""

sendemail \
 -f ${FROM} \
 -t ${TO} \
 -u ${SUBJECT} \
 -s ${SERVER} \
 -m ${MESSAGE} \
 -xu ${USERNAME} \
 -xp ${PASSWORD} \
 -v -o message-charset=${CHARSET}
```

各参数对应的内容如下：

```
-f：           表示发送者的邮箱
-t：           表示接收者的邮箱
-s：           表示SMTP的服务器的域名或者IP，也可以加端口号 域名：port
-u：           表示邮件主题
-m：           表示的内容
-xu：          表示SMTP验证的用户名（也就是登录邮箱的用户名）
-xp：          表示SMTP验证的密码（也就是登录邮箱的密码）
-cc：          表示抄送
-bcc：         表示暗抄送
-a：           后加文件名，会以附件的形式发送
-o message-charset=utf8             邮件内容的编码
-o message-content-type=html        邮件内容的格式
-o message-file=a.txt               把文件内容以邮件正文发出
```

整理如下：

```bash
#!/bin/bash
# Weather Data
CITY=hefei
TOKEN=和风KEY
WEATHER=$(curl "https://free-api.heweather.net/s6/weather/lifestyle?location=${CITY}&key=${TOKEN}")
SUGGESTIONS=$(echo ${WEATHER} | jq -r '.HeWeather6[0].lifestyle| values[].txt')
echo ${SUGGESTIONS}

#Email Send
SERVER="smtp.sina.com:22"
FROM=""
TO=""
SUBJECT="天气预报来啦"
MESSAGE=${SUGGESTIONS}
CHARSET="utf-8"
USERNAME=""
PASSWORD=""

sendemail \
 -f ${FROM} \
 -t ${TO} \
 -u ${SUBJECT} \
 -s ${SERVER} \
 -m ${MESSAGE} \
 -xu ${USERNAME} \
 -xp ${PASSWORD} \
 -v -o message-charset=${CHARSET}
```
