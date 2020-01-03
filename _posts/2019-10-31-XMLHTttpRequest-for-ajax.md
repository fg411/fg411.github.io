---
date: 2019-10-31 15:00:00
layout: post
title: 使用XMLHttpRequest实现AJAX
subtitle: 通过 XMLHttpRequest, ActiveXObject 实现 ajax
description: 使用 XMLHttpRequest, ActiveXObject 实现 ajax
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046196637&di=5c99086c0b9a3a428c92c114f34c7bc4&imgtype=0&src=http%3A%2F%2Fimg5.autotimes.com.cn%2Fnews%2F2017%2F10%2F1029_104214303492.gif
optimized_image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046196637&di=5c99086c0b9a3a428c92c114f34c7bc4&imgtype=0&src=http%3A%2F%2Fimg5.autotimes.com.cn%2Fnews%2F2017%2F10%2F1029_104214303492.gif
category: javascript
tags:
  - XMLHttpRequest
  - ActiveXObject
  - ajax
  - javascript
author: fg411
---

通过 `XMLHttpRequest`, `ActiveXObject` 实现 `ajax` 请求

因项目是Web App，在接手项目时就已使用`fetch`,而 ipad 下并不支持 `fetch`。所以，在做网络请求部分做了点处理

`XMLHTttpRequest`和`fetch`是浏览器的原生API，而`ajax`的核心是 `XMLHTttpRequest`对象

### 创建XHR

``` javascript
const XHR = {
    createActiveXObject: function() {
        try{
            return new window.ActiveXObject("Microsoft.XMLHttp");
        }catch(e){}
    },
    createStandardXHR: function(){
        try{
            return new window.XMLHttpRequest();
        }catch(e){}
    },
    getXHR: function(){
        var XHR = null;
        if(typeof window.ActiveXObject != 'undefined'){
            XHR = xhr.createActiveXObject();
        }else{
            XHR = xhr.createStandardXHR();
        }
        return XHR;
    },
    onreadystatechange: function(xhr, callback){
        if(!callback){return;}
        if(xhr.readyState == 4){
            if((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
                callback(xhr.responseText);
            }
        }
    },
    ontimeout: function(callback){
        if(!callback){return;}
        callback();
    },
    onprogress: function(event,callback){
        if(!callback){return;}
        callback(event);
    }
}
```

### 使用XHR

``` javascript
let xhr = XHR.getXHR();
xhr.onreadystatechange = XHR.onreadystatechange(function(data){
    console.log(data);
});

xhr.timeout=10000;

xhr.ontimeout = XHR.ontimeout(function(){
    alert("timeout");
});

xhr.onprogress = XHR.onprogress(function(event){
    console.log(event.totalSize);
});

xhr.open("get","url",true);
xhr.send(null);
```

### 大体的流程

#### 初始化请求

使用XHR对象时，使用的第一个方法就是open()，这个方法不会发送请求，但会初始化一个请求准备发送

open方法接收三个参数: 规定请求类型（POST或GET）、请求地址url、异步（true）同步（false）

``` javascript
xhr.open('get', 'example.php', false);
```
#### 发送请求

分`GET`和`POST`两种。`GET` 请求方式是通过URL参数将数据提交到服务器的，`POST` 则是通过将数据作为send的参数提交到服务器

GET请求:发送的值为空，一般写上null
``` javascript
xhr.open('GET', url);
xhr.send(null);
```

POST请求,要设置表单提交的内容类型。要模拟表单提交请求的话就将`Content-type`头部信息设置为`application/x-www-form-urlencoded`，并将发送的参数经过`encodeURIComponent()`方法进行编码。

定义请求的头部信息，在send()与open()方法之间进行设置; `setRequestHeader(header,value)` 向请求添加请求头,header:规定头的名称，value:规定头的值

参数列表以“key=value”的形式，key和value都需要进行编码，因为包含特殊字符。每次请求的时候都会在参数列表中拼入一个“v=xx”的随机字符串，这样是为了拒绝缓存，每次都直接请求到服务器上

* encodeURI():用于整个URI的编码，不会对本身属于URI的特殊字符串进行编码，如 : / ? # 其对应的解码函数`decodeURI()`
* encodeURIComponent():用于对URI中的某一部分进行编码，会对它发现的任何非标准字符进行编码；其他对应的解码函数`decodeURIComponent()`

XHR.send(null) ：发送请求，当没有参数传递时，参数为null;当为get请求时，携带的参数需要通过encodeURIComponent进行编码

``` javascript
xhr.open('POST', url);
xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
xhr.send(stringData);
```

#### 监控请求状态，处理请求数据
xhr对象有且仅有一个事件`onreadystatechange`，每一次xhr对象的 `readyState`状态值改变都会触发该事件，但是该方法不能为单独的一个xhr对象绑定多个事件处理逻辑，即 `onreadystatechange` 只能绑定一个事件处理的function，如果你想处理多件事情，那么只能在绑定的function中进行多事件处理的逻辑调用

当为异步请求时，XHR的`readystate`属性表示请求/响应过程的当前活动阶段

``` text
0-未初始化，尚未调用open()方法；
1-启动，调用了open()方法，未调用send()方法； 服务器连接已建立；
2-发送，已经调用了send()方法，未接收到响应； 请求已接收；
3-接收，已经接收到部分响应数据； 请求处理中；
4-完成，已经接收到全部响应数据； 请求已完成；
```
xhr对象有且仅有一个事件`onreadystatechange`，每一次xhr对象的 `readyState`状态值改变都会触发该事件，因此要在调用`open()`之前指定`onreadystatechange`事件以便判断是否已经响应完成且可以使用数据了;<strong>该方法不能为单独的一个xhr对象绑定多个事件处理逻辑，即 `onreadystatechange` 只能绑定一个事件处理的function，如果你想处理多件事情，那么只能在绑定的function中进行多事件处理的逻辑调用</strong>

在`readystatechange`事件中，先判断响应是否接收完成，然后判断服务器是否成功处理请求，`xhr.status` 是状态码，状态码以2开头的都是成功，304表示从缓存中获取

接收到响应后，响应的数据会自动填充XHR对象，相关属性有：
``` json
responseText:获得字符串形式的响应数据
responseXML:获得XML形式的响应数据
status:响应的HTTP状态码
statusText:HTTP状态的说明
```

``` javascript
xhr.onreadystatechange = function(){
  if(xhr.readyState == 4){
      if(xhr.status == 200){
          document.getElementById("unInfo").innerHTML = xhr.responseText;
      }
  }
}
```

### 创建 ajax 函数

```javascript
function ajax(options){
    options = options||{};
    optoins.type = (options.type||'GET').toUpperCase();
    options.dataType = options.dataType||'json';
    params = formatParams(options.data);

    // 创建
    let xhr = XHR.getXHR()
    xhr.timeout=10000

    //提交数据
    if(options.type === 'GET') {
        xhr.open('GET',options.url+'?' + params, true)
        xhr.send(null)
    }else if(options.type === 'POST') {
        xhr.open('POST',options.url,true);
        //设置表单提交时的内容类型
        xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
        xhr.send(params);
    }

    // 接收
    xhr.onreadystatechange = function(){
        if(xhr.readyState==4){
            var status=xhr.status;
            if(status>=200 && status < 300 || xhr.status == 304){
                options.success && options.success(xhr.responseText,xhr.responseXML);
            }else{
                options.error && options.error(status);
            }
        }
    }
}

function formatParams(data){
    var arr=[];
    for(var name in data){
        arr.push(encodeURIComponent(name)+'='+encodeURIComponent(data[name]));
    }
    arr.push(('v='Math.random()).replace('.',''));
    return arr.join('&');
}

ajax({
    url:'http://localhost:8080/test.php',
    type:'POST',
    dataType:'json',
    data:{name:'John', age:18},
    success:function(response,xml){
        //请求成功后执行的代码
    },
    error:function(status){
        //失败后执行的代码
    }
})
```

--------

## 参考

 - [再也不学AJAX了！（二）使用AJAX](https://segmentfault.com/a/1190000012237477?utm_source=weekly&utm_medium=email&utm_campaign=email_weekly)
