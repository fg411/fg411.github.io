---
date: 2020-04-10 17:03
layout: post
title: JS实现粘贴图片
subtitle: 教你如何把剪切板里的图片信息粘贴到页面上
description: 使用JS教你如何粘贴图片，包教不包会
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: https://bkimg.cdn.bcebos.com/pic/9825bc315c6034a81358c82ac1134954082376e6?x-bce-process=image/watermark,image_d2F0ZXIvYmFpa2UyMjA=,g_7,xp_5,yp_5
category: blog
tags:
  - 粘贴图片
author: fg411
---

　　之前同事写过把剪切板内容粘贴到富文本框编辑器，当时只想当咸鱼就没有研究，最近研究`FileReader`时发现一个例子也是粘贴图片内容，闲来无事就写个demo试一试，顺便还发现了很多不常见的知识点

``` html
<div id="preview" contenteditable="true">
    江湖笑，<br />
    恩怨了，<br />
    人过招，<br />
    笑藏刀。 <br />
</div>
```
``` javascript
document.addEventListener('paste', function(event){
    let items = (event.clipboardData || window.clipboardData).items;
    let blob = null;
    let str = null;
    if(items && items.length) {
        for(let i = 0; i < items.length; i++) {
            if(items[i].kind == 'file' && items[i].type.indexOf('image') !== -1) {
                blob = items[i].getAsFile();
                break;
            } else if(items[i].kind === "string" && items[i].type.indexOf('text/plain') != -1) {
                items[i].getAsString(function (str) {
                    insertHtmlAtCaret(document.createTextNode(str)); // 插入文本到光标处并移动光标到新位置
                })
                return;
            }
        }
    }
    var reader = new FileReader();
    reader.onload = function(event) {
        var img = document.createElement('img');
        img.src = event.target.result;
        insertHtmlAtCaret(img);

        // document.getElementById('preview').innerHTML = '<img src="' + event.target.result + '" class="upload-image">';
        event.preventDefault();
    }
    reader.readAsDataURL(blob)
})

function insertHtmlAtCaret(childElement){
    var selection, range;
    if(window.getSelection) { // IE11 and non-IE
        selection = window.getSelection();
        if(selection.getRangeAt && selection.rangeCount) {
            range = selection.getRangeAt(0);
            range.deleteContents();
            var el = document.createElement("div");
            el.appendChild(childElement);
            // document.createDocumentFragment() 创建一个新的空白的文档片段
            var frag = document.createDocumentFragment(), node, lastNode;
            while ((node = el.firstChild)) {
                lastNode = frag.appendChild(node);
            }
            range.insertNode(frag);
            if (lastNode) {
                range = range.cloneRange();
                range.setStartAfter(lastNode);
                range.collapse(true);
                selection.removeAllRanges();
                selection.addRange(range);
            }
        }
    } else if(document.selection && document.selection.type != "Control") { // IE < 9
        //document.selection.createRange().pasteHTML(html);
    }
}
```
　　看demo说可以兼容IE10，写了半天还是不行，*我TM心态都崩了啊*。其实获取剪切板的内容并难，把剪切板内容添加到光标处才是全篇最麻烦的地方，demo里的`insertHtmlAtCaret`是网上看到的，内容我也没太明白，等哪天需要再好好研究

---

### 知识点：
 - [ClipboardEvent.clipboardData](https://developer.mozilla.org/zh-CN/docs/Web/API/ClipboardEvent/clipboardData)
 - [Document.createDocumentFragment](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createDocumentFragment)
 - [FileReader.readAsDataURL](https://developer.mozilla.org/zh-CN/docs/Web/API/FileReader/readAsDataURL)

------

### 资料

 - [js实现ctrl+v粘贴上传图片](https://www.jb51.net/article/80657.htm)
 - [HTML-DOM-createDocumentFragment方法](https://www.runoob.com/jsref/met-document-createdocumentfragment.html)
 - [Element: paste事件](https://developer.mozilla.org/zh-CN/docs/Web/Events/paste)
 - [div中粘贴图片并上传服务器 div中拖拽图片文件并上传服务器](https://www.cnblogs.com/fj99/p/5499233.html)
 - [Window.getSelection](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/getSelection)
 - [FileReader获取上传图片的宽高](http://www.mamicode.com/info-detail-2579337.html)
 - [基于js粘贴事件paste简单解析以及遇到的坑](https://www.jb51.net/article/123071.htm)
