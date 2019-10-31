---
date: 2019-10-30 18:00:00
layout: post
title: 浅谈Javascript中的EventLoop机制
subtitle: 浅谈Javascript中的 EventLoop机制
description: 浅谈Javascript中的 EventLoop机制
image: https://res.cloudinary.com/dm7h7e8xj/image/upload/v1559825288/theme17_nlndhx.jpg
optimized_image: https://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_380/v1559825288/theme17_nlndhx.jpg
category: javascript
tags:
  - Event Loop机制
  - 事件循环机制
  - JavaScript
author: fg411
---

一直对JavaScript的运行机制不甚了解，看了一篇文章描述的很仔细，就弄了点干货下来

`javascript` 是一门单线程的非阻塞的脚本语言，`js` 任务也要按顺序一个一个顺序执行。那么问题来了，假如我们想浏览新闻，但是新闻包含的超清图片加载很慢，难道我们的网页要一直卡着直到图片完全显示出来？因此任务分为两类：同步任务 和 异步任务

![任务导图](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxNy8xMS8yMS8xNWZkZDg4OTk0MTQyMzQ3)

导图要表达的内容用文字来表述的话：

 - 同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入Event Table并注册函数。
 - 当指定的事情完成时，Event Table会将这个函数移入Event Queue。
 - 主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。
 - 上述过程会不断重复，也就是常说的Event Loop(事件循环)。

## 关于setTimeout

延迟执行方法 setTimeout 一般用于延迟执行，有时明明写的延时3秒，实际却5，6秒才执行函数，这又咋回事？
正常情况下是这样的：
```javascript
setTimeout(() => {
	task()
}, 3000)
console.log('执行console')

// 执行console
// task()
```
通常会遇到这样的：
```javascript
setTimeout(() => {
    task()
},3000)

sleep(10000000)
```
乍一看其实差不多嘛，但我们把这段代码在 chrome 执行一下，却发现控制台执行 `task()` 需要的时间远远超过3秒，说好的延时三秒呢？
这时候我们需要重新理解setTimeout的定义。我们先说上述代码是怎么执行的：

 - task()进入Event Table并注册,计时开始
 - 执行sleep函数，很慢，非常慢，计时仍在继续
 - 3秒到了，计时事件timeout完成，task()进入Event Queue，但是sleep也太慢了吧，还没执行完，只好等着
 - sleep终于执行完了，task()终于从Event Queue进入了主线程执行

上述的流程走完，我们知道 `setTimeout` 这个函数，是经过指定时间后，把要执行的任务(本例中为task())加入到 `Event Queue` 中，又因为是单线程任务要一个一个执行，如果前面的任务需要的时间太久，那么只能等着，导致真正的延迟时间远远大于3秒。
我们还经常遇到 `setTimeout(fn,0)` 这样的代码，0秒后执行又是什么意思呢？是不是可以立即执行呢？
答案是不会的，`setTimeout(fn, 0)`的含义是，指定某个任务在主线程最早可得的空闲时间执行，意思就是不用再等多少秒了，只要主线程执行栈内的同步任务全部执行完成，栈为空就马上执行。关于 `setTimeout`，即便主线程为空，0毫秒实际上也是达不到的。根据HTML的标准，最低是4毫秒

## 关于 `setInterval`

都已经涉及到 `setTimeout` 了，那也说说和 `setTimeout` 相似的 `setInterval` 吧！
他俩差不多，只不过 `setInterval` 是循环的执行。对于执行顺序来说，`setInterval` 会每隔指定的时间将注册的函数置入`Event Queue`，如果前面的任务耗时太久，那么同样需要等待
唯一需要注意的一点是，对于 `setInterval(fn, ms)` 来说，我们已经知道不是每过ms秒会执行一次 `fn`，而是每过ms秒，会有 `fn` 进入`Event Queue`。一旦 `setInterval` 的回调函数`fn`执行时间超过了延迟时间ms，那么就完全看不出来有时间间隔了

## 关于 `Promise` 与 `process.nextTick(callback)`

传统的定时器我们已经研究过了，接着我们探究`Promise`与`process.nextTick(callback)`的表现
`Promise`的定义和功能本文不再赘述，不了解的可以学习一下阮一峰老师的 `Promise`。而 `process.nextTick(callback)` 类似`node.js`版的 `setTimeout`，在事件循环的下一次循环中调用 callback 回调函数

我们进入正题，除了广义的同步任务和异步任务，我们对任务有更精细的定义：

 - macro-task(宏任务)：包括整体代码`script`，`setTimeout`，`setInterval`
 - micro-task(微任务)：`Promise`，`process.nextTick`

不同类型的任务会进入对应的`Event Queue`，比如`setTimeout`和`setInterval`会进入相同的`Event Queue`

事件循环的顺序，决定`js`代码的执行顺序。进入整体代码(宏任务)后，开始第一次循环。接着执行所有的微任务。然后再次从宏任务开始，找到其中一个任务队列执行完毕，再执行所有的微任务。我们用文章最开始的一段代码说明：

```javascript
setTimeout(function() {
    console.log('setTimeout');
})

new Promise(function(resolve) {
    console.log('promise');
}).then(function() {
    console.log('then')
})
console.log('console')
```

 - 这段代码作为宏任务，进入主线程。
 - 先遇到`setTimeout`，那么将其回调函数注册后分发到宏任务`Event Queue`。(注册过程与上同，下文不再描述)
 - 接下来遇到了`Promise`，`new Promise`立即执行，`then函数`分发到微任务`Event Queue`
 - 遇到`console.log()`，立即执行
 - 好啦，整体代码`script`作为第一个宏任务执行结束，看看有哪些微任务？我们发现了then在微任务Event Queue里面，执行。
 - ok，第一轮事件循环结束了，我们开始第二轮循环，当然要从宏任务Event Queue开始。我们发现了宏任务Event Queue中`setTimeout`对应的回调函数，立即执行。
 - 结束。

事件循环，宏任务/微任务的关系如图所示：
![宏任务/微任务 导图](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxNy8xMS8yMS8xNWZkY2VhMTMzNjFhMWVj)

例子1：

```
console.log('1');

setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
process.nextTick(function() {
    console.log('6');
})
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    console.log('8')
})

setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})
```

第一轮事件循环流程分析如下：

 - 整体script作为第一个宏任务进入主线程，遇到console.log，输出1。
 - 遇到`setTimeout`，其回调函数被分发到宏任务Event Queue中。我们暂且记为`setTimeout1`
 - 遇到`process.nextTick()`，其回调函数被分发到微任务Event Queue中。我们记为`process1`
 - 遇到`Promise`，new Promise直接执行，输出7。then被分发到微任务Event Queue中。我们记为`then1`
 - 又遇到了`setTimeout`，其回调函数被分发到宏任务Event Queue中，我们记为`setTimeout2`

| 宏任务Event Queue | 微任务Event Queue |
|--|--|
| setTimeout1 | process1 |
| setTimeout2 | then1 |

 - 上表是第一轮事件循环宏任务结束时各Event Queue的情况，此时已经输出了1和7。
 - 我们发现了`process1`和`then1`两个微任务。
 - 执行`process1`,输出6。
 - 执行`then1`，输出8。

好了，第一轮事件循环正式结束，这一轮的结果是输出1，7，6，8。那么第二轮时间循环从`setTimeout1`宏任务开始：

 - 首先输出2。接下来遇到了`process.nextTick()`，同样将其分发到微任务Event Queue中，记为`process2`。new Promise立即执行输出4，then也分发到微任务Event Queue中，记为`then2`

| 宏任务Event Queue | 微任务Event Queue |
|--|--|
| setTimeout2 | process2 |
|  | then2 |

 - 第二轮事件循环宏任务结束，我们发现有`process2`和`then2`两个微任务可以执行
 - 输出3
 - 输出5
 - 第二轮事件循环结束，第二轮输出2，4，3，5
 - 第三轮事件循环开始，此时只剩`setTimeout2`了，执行
 - 直接输出9
 - 将`process.nextTick()`分发到微任务Event Queue中。记为`process3`
 - 直接执行new Promise，输出11
 - 将then分发到微任务Event Queue中，记为`then3`


| 宏任务Event Queue  | 微任务Event Queue  |
|--|--|
|  | process3 |
|  | then3|

 - 第三轮事件循环宏任务执行结束，执行两个微任务`process3`和`then3`
 - 输出10
 - 输出12
 - 第三轮事件循环结束，第三轮输出9，11，10，12

整段代码，共进行了三次事件循环，完整的输出为1，7，6，8，2，4，3，5，9，11，10，12。(node环境下的事件监听依赖libuv与前端环境不完全相同，输出顺序可能会有误差)

例子2：

```javascript
const first = () => (new Promise((resovle,reject)=>{
    console.log(3);
    let p = new Promise((resovle, reject)=>{
         console.log(7);
        setTimeout(()=>{
           console.log(5);
           resovle(6); 
        },0)
        resovle(1);
    }); 
    resovle(2);
    p.then((arg)=>{
        console.log(arg);
    });

}));

first().then((arg)=>{
    console.log(arg);
});
console.log(4);
```
**第一轮事件循环**

 - 先执行宏任务，主script ，new Promise立即执行，输出【3】
 - 执行p这个new Promise 操作，输出【7】
 - 发现`setTimeout`，将回调放入下一轮任务队列（Event Queue），p的then，姑且叫做`then1`，放入微任务队列，发现first的then，叫`then2`，放入微任务队列
 - 执行`console.log(4)`，输出【4】
 - 宏任务执行结束，再执行微任务
 - 执行`then1`，输出【1】
 - 执行`then2`，输出【2】
 - 到此为止，第一轮事件循环结束。开始执行第二轮。

**第二轮事件循环**

 - 先执行宏任务里面的，也就是`setTimeout`的回调，输出【5】。
 - `resovle`不会生效，因为p这个Promise的状态一旦改变就不会在改变了。 所以最终的输出顺序是3、7、4、1、2、5

## 原文链接：

* [这一次，彻底弄懂Javascript执行机制](https://juejin.im/post/59e85eebf265da430d571f89)
* [一个Promise面试题](https://juejin.im/post/5af800fe518825429c594f92?utm_source=gold_browser_extension)