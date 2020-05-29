---
date: 2020/05/29 16:50
layout: post
title: 使用vue指令实现复制代码
subtitle: 自定义一个复制code的指令吧
description: 使用vue指令实现复制代码
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: http://desk.fd.zol-img.com.cn/t_s960x600c5/g5/M00/02/03/ChMkJlbKxqqIaOMbAAQgJgyQQIwAALHogFV7g4ABCA-804.jpg
category: blog
tags:
  - vue
  - Vue.directive
  - 复制代码
author: fg411
---

　　看掘金刷到`Vue.directive`，想想到现在自己也没写过指令，于是就想写一个指令试试。正考虑写什么好呢，瞅到了`复制代码`，不禁感叹：哥真是机智啊。废话不多说，撸起来！

　　`src`的目录下有关文件的路径：

```
├─ main.js
├─ libs
│  ├─  copy.js
│  └─  directives.js
└─ pages
   └─ Index.vue
```

　　`copy.js`：指令的具体内容

```javascript
const vCopy = {
  bind(el, binding, vNode) {
    function clickHandler(e) {
      if(el !== e.target) {
        return false;
      }
      let code = el.previousElementSibling.textContent
      // let code = el.previousSibling.textContent
      if(!code) {
        return false
      }

      const textarea = document.createElement('textarea')
      textarea.readOnly = 'readonly'
      textarea.style.position = 'absolute'
      textarea.style.left = '-9999px'
      textarea.value = code
      document.body.appendChild(textarea)
      textarea.select()
      const result = document.execCommand('Copy')
      if(result) {
        console.log('复制成功'); // 应该窗口提示复制成功
      }
      document.body.removeChild(textarea)
    }
    el.__vueClickOutside__ = clickHandler;
    document.addEventListener('click', clickHandler)
  },
  componentUpdated(el, {value}) {},
  unbind(el, binding) {
    document.removeEventListener('click', el.__vueClickOutside__)
    delete el.__vueClickOutside__
  }
}

export default vCopy
```

　　`main.js`：没有其他指令的话，使用方式一就好，只需要使用`Vue.directive`。如果多个指令，方式二是个不错的选择

```javascript
/**
** 方式一
**/

import vCopy from './libs/copy'
Vue.directive('copy', vCopy)

/**
** 方式二
**/
import Directives from './libs/directives'
Vue.use(Directives)
```

　　`directives.js`：把指令汇总

```javascript
import copy from './copy';

const directives = {
  copy
}

export default {
  install(Vue) {
    Object.keys(directives).forEach((key) => {
      Vue.directive(key, directives[key])
    })
  }
}

```

　　`Index.vue`：使用指令部分

```js
<template>
  <div class="code">
    <pre class="code_pre">
      <code>{{code}}</code>
      <span v-copy class="copy">复制代码</span>
    </pre>
  </div>
</template>

<script>
export default {
  data() {
    return {
      code: ''
    }
  },
  created() {
    this.code = `\nlet a = 1;\nconsole.log(a++);\nfunction a() {
  var brother2 = document.getElementById("test").previousElementSibling;
}`
  },
  methods: {}
}
</script>

<style scoped>
  .code {
    width: 100%;
    min-height: 50px;
    position: relative;
    word-wrap: break-word;
    margin: 1.813rem 0;
  }
  .code_pre {
    text-align: left;
    color: #CDC;
    background-color: rgba(0, 0, 0, 0.55);
    overflow-x: auto;
    padding: 0 10px;
  }
    .copy {
      width: 70px;
      position: absolute;
      top: 12px;
      right: 0;
      font-size: 12px;
      line-height: 1;
      cursor: pointer;
      color: hsla(0, 60%, 54.9%, .9);
      transition: color .1s;
    }
</style>

```

　　写完之后想着要不要做成组件，毕竟对布局的要求比较高，转念一想，这基本算是个组件了，就到此为止吧

------

## 资料

 - [总结vue知识体系之高级应用篇](https://juejin.im/post/5e702c4c51882549052f6054#heading-2)
 - [JS获取子节点、父节点和兄弟节点的方法实例总结](https://www.jb51.net/article/143286.htm)
 - [document.execCommand](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/execCommand)

