---
date: 2020-01-06 18:00:00
layout: post
title: JS的this面试题
subtitle: JS的this面试题
description: JS的this面试题
image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1578046469146&di=24b211897ae2ce4b99f4c04c8cbfaced&imgtype=0&src=http%3A%2F%2Fattimg.dospy.com%2Fimg%2Fday_110923%2F20110923_0dd4df9e10e0aabdb8aaoGPSl0x9i9p6.jpg
optimized_image: http://pic1.win4000.com/wallpaper/f/544a04c831e05.jpg
category: javascript
tags:
  - this
  - JavaScript
author: fg411
---

## 1 . `this`

---
　　什么是 `this` ？在讨论 `this` 绑定前，我们得先搞清楚 `this` 代表什么
 - 1 . `this`是`JavaScript`的关键字之一。它是 对象自动生成的一个内部对象，只能在 对象 内部使用。随着函数使用场合的不同，`this` 的值会发生变化
 - 2 .  **`this`指向什么，完全取决于 什么地方以什么方式调用，而不是 创建时**。（比较多人误解的地方）（它非常语义化，`this`在英文中的含义就是 **这，这个** ，但这其实起到了一定的误导作用，因为`this`并不是一成不变的，并不一定一直指向当前 **这个**）

## 2 . `this` 绑定规则

---
　　掌握了下面介绍的4种绑定的规则，那么**你只要看到函数调用就可以判断 `this` 的指向了**

### 2.1 默认绑定

```
function foo(){
    var a = 1 ;
    console.log(this.a);    // 10
}
var a = 10;
foo();
```

　　这种就是典型的默认绑定，我们看看 foo 调用的位置，”光杆司令“，像 **这种直接使用而不带任何修饰的函数调用** ，就 **默认且只能** 应用 默认绑定。

　　那默认绑定到哪呢，一般是`window`上，严格模式下 是`undefined`

### 2.2 隐性绑定

```
function foo(){
    console.log(this.a);
}
var obj = {
    a : 10,
    foo : foo
}
foo();                // ?

obj.foo();            // ?
```

　　答案 : undefined 10

　　`foo()`的这个写法熟悉吗，就是我们刚刚写的默认绑定,等价于打印`window.a`,故输出`undefined` ,
下面`obj.foo()`这种大家应该经常写，这其实就是我们马上要讨论的 **隐性绑定** 

　　函数foo执行的时候有了**上下文对象**，即 `obj`。这种情况下，**函数里的this默认绑定为上下文对象**，等价于打印`obj.a`,故输出`10` 

　　如果是链性的关系，比如 `xx.yy.obj.foo();`, 上下文取函数的直接上级，即紧挨着的那个，或者说对象链的最后一个

### 2.3 显性绑定

#### 2.3.1 隐性绑定的限制

　　在我们刚刚的 **隐性绑定中有一个致命的限制，就是上下文必须包含我们的函数** ，例：`var obj = { foo : foo }`,如果上下文不包含我们的函数用隐性绑定明显是要出错的，**不可能每个对象都要加这个函数** ,那样的话扩展,维护性太差了，我们接下来聊的就是直接 **给函数强制性绑定`this`**

## 2.3.2 call apply bind

　　这里我们就要用到 `js` 给我们提供的函数 `call` 和 `apply`，**它们的作用都是改变函数的`this`指向**，**第一个参数都是 设置`this`对象**

　　两个函数的区别：
 - `call` 从第二个参数开始所有的参数都是 原函数的参数
 - `apply` 只接受两个参数，且第二个参数必须是数组，这个数组代表原函数的参数列表

```
function foo(a,b){
    console.log(a+b);
}
foo.call(null,'海洋','饼干');        // 海洋饼干  这里this指向不重要就写null了
foo.apply(null, ['海洋','饼干'] );     // 海洋饼干
```

　　除了 `call`，`apply` 函数以外，还有一个改变`this`的函数 `bind`，它和 `call`,`apply` 都不同

　　**bind 只有一个函数，且不会立刻执行，只是将一个值绑定到函数的this上,并将绑定好的函数返回**。例:

```
function foo(){
    console.log(this.a);
}
var obj = { a : 10 };

foo = foo.bind(obj);
foo();                    // 10
```

#### 2.3.3 显性绑定

　　开始入正题，上代码，就用上面隐性绑定的例子：
```
function foo(){
    console.log(this.a);
}
var obj = {
    a : 10            //去掉里面的foo
}
foo.call(obj);        // 10
```

　　我们将隐性绑定例子中的 上下文对象 里的函数去掉了，显然现在不能用 `上下文.函数` 这种形式来调用函数，大家看代码里的显性绑定代码`foo.call(obj)`，看起来很怪，和我们之前所了解的函数调用不一样。

　　其实`call` 是 `foo` 上的一个函数,在改变`this`指向的同时执行这个函数

### 2.4 `new`绑定

---------

#### 2.4.1 什么是 `new`

　　学过面向对象的小伙伴对new肯定不陌生，js的new和传统的面向对象语言的new的作用都是创建一个新的对象，但是他们的机制完全不同

　　创建一个新对象少不了一个概念，那就是`构造函数`，传统的面向对象 构造函数 是类里的一种特殊函数，要创建对象时使用`new 类名()`的形式去调用类中的构造函数，而js中就不一样了

　　**js中的只要用new修饰的 函数就是'构造函数'**，准确来说是 **函数的`构造调用`**，因为在js中并不存在所谓的'构造函数'

　　那么用new 做到函数的`构造调用`后，js帮我们做了什么工作呢:

 - 1.  创建一个新对象
 - 2.  把这个新对象的`__proto__`属性指向 原函数的`prototype`属性。(即继承原函数的原型)
 - 3.  **将这个新对象绑定到 此函数的this上 **
 - 4.  返回新对象，如果这个函数没有返回其他**对象**

　　第三条就是我们下面要聊的new绑定：

## 2.4.2 `new`绑定

```
function foo(){
    this.a = 10;
    console.log(this);
}
foo();                    // window对象
console.log(window.a);    // 10   默认绑定

var obj = new foo();      // foo{ a : 10 }  创建的新对象的默认名为函数名
                          // 然后等价于 foo { a : 10 };  var obj = foo;
console.log(obj.a);       // 10    new绑定
```

　　**使用new调用函数后，函数会 _以自己的名字 命名 和 创建_ 一个新的对象，并返回。**

　　特别注意 : 如果原函数返回一个对象类型，那么将无法返回新对象,你将丢失绑定this的新对象，例:

```
function foo(){
    this.a = 10;
    return new String("捣蛋鬼");
}
var obj = new foo();
console.log(obj.a);       // undefined
console.log(obj);         // "捣蛋鬼"
```

### 2.5 `this`绑定优先级

```
new 绑定 > 显示绑定 > 隐式绑定 > 默认绑定
```

## 3 . 总结

　　1. 如果函数被`new` 修饰

    ```
    this绑定的是新创建的对象，例:var bar = new foo(); 函数 foo 中的 this 就是一个叫foo的新创建的对象 , 然后将这个对象赋给bar , 这样的绑定方式叫 new绑定 .
    ```

　　2. 如果函数是使用`call,apply,bind`来调用的

    ```
    this绑定的是 call,apply,bind 的第一个参数.例: foo.call(obj); , foo 中的 this 就是 obj , 这样的绑定方式叫 显性绑定 .
    ```

　　3. 如果函数是在某个 上下文对象 下被调用

    ```
    this绑定的是那个上下文对象，例 : var obj = { foo : foo }; obj.foo(); foo 中的 this 就是 obj . 这样的绑定方式叫 隐性绑定 .
    ```

　　4. 如果都不是，即使用默认绑定

    ```
       例:function foo(){...} foo() ,foo 中的 this 就是 window.(严格模式下默认绑定到undefined).
       这样的绑定方式叫 默认绑定 .
    ```

## 4 . 箭头函数的`this`绑定

---------

　　箭头函数，一种特殊的函数，不使用`function`关键字，而是使用`=>`,它和普通函数的区别：

 - 1.  箭头函数不使用我们上面介绍的四种绑定，而是**完全根据外部作用域来决定this**。(它的父级是使用我们的规则的哦)
 - 2.  箭头函数的this绑定无法被修改 (这个特性非常爽)

```
function foo(){
    return ()=>{
        console.log(this.a);
    }
}
foo.a = 10;

// 1. 箭头函数关联父级作用域this

var bar = foo();            // foo默认绑定
bar();                      // undefined 哈哈，是不是有小伙伴想当然了

var baz = foo.call(foo);    // foo 显性绑定
baz();                      // 10 

// 2. 箭头函数this不可修改
//这里我们使用上面的已经绑定了foo 的 baz
var obj = {
    a : 999
}
baz.call(obj);              // 10
```

　　还记得我们之前第一个例子吗，将它改成箭头函数的形式(可以彻底解决恶心的this绑定问题)：

```
var people = {
    Name: "海洋饼干",
    getName : function(){
        console.log(this.Name);
    }
};
var bar = people.getName;

bar();    // undefined
```
============ 修改后 ============
```
var people = {
    Name: "海洋饼干",
    getName : function(){
        return ()=>{
            console.log(this.Name);
        }
    }
};
var bar = people.getName(); //获得一个永远指向people的函数，不用想this了,岂不是美滋滋？

bar();    // 海洋饼干 
```
　　可能会有人不解为什么在箭头函数外面再套一层，直接写不就行了吗，搞这么麻烦干嘛，其实这**也是箭头函数很多人用不好的地方**

```
var obj= {
    that : this,
    bar : function(){
        return ()=>{
            console.log(this);
        }
    },
    baz : ()=>{
        console.log(this);
    }
}
console.log(obj.that);  // window
obj.bar()();            // obj
obj.baz();              // window
```
 - 1.  我们先要搞清楚一点，obj的当前作用域是window,如 obj.that === window。
 - 2.  如果不用function（function有自己的函数作用域）将其包裹起来，那么默认绑定的父级作用域就是window。
 - 3.  用function包裹的目的就是将箭头函数绑定到当前的对象上。函数的作用域是当前这个对象，然后箭头函数会自动绑定函数所在作用域的this，即obj

## 5 . 面试题解析
---------

1. 
```
var x = 10;
var obj = {
    x: 20,
    f: function(){
        console.log(this.x);        // ?
        var foo = function(){ 
            console.log(this.x);    
            }
        foo();                      // ?
    }
};
obj.f();
```
------------------答案-------------------

答案 ： 20 10

解析 ：考点 **1.** this默认绑定 **2.** this隐性绑定

```
var x = 10;
var obj = {
    x: 20,
    f: function(){
        console.log(this.x);    // 20
                                // 典型的隐性绑定,这里 f 的this指向上下文 obj ,即输出 20
        function foo(){ 
            console.log(this.x); 
            }
        foo();       // 10
                     //有些人在这个地方就想当然的觉得 foo 在函数 f 里,也在 f 里执行，
                     //那 this 肯定是指向obj 啊 , 仔细看看我们说的this绑定规则 , 对应一下很容易
                     //发现这种'光杆司令'，是我们一开始就示范的默认绑定,这里this绑定的是window
    }
};
obj.f(); 
```

2 .
```
function foo(arg){
    this.a = arg;
    return this
};

var a = foo(1);
var b = foo(10);

console.log(a.a);    // ?
console.log(b.a);    // ?
```
-----------------------答案---------------------

答案 ： undefined 10

解析 ：考点 **1.** 全局污染 **2.** this默认绑定

　　这道题很有意思，问题基本上都集中在第一`undefined`上，这其实是题目的小陷阱，但是追栈的过程绝对精彩
让我们一步步分析这里发生了什么：

1.  foo(1)执行，应该不难看出是默认绑定吧 , this指向了window，函数里等价于 window**.**a = 1,return window;
2.  var a = foo(1) 等价于 window**.**a = window , 很多人都忽略了**var a 就是window.a **，将刚刚赋值的 1 替换掉了。
3.  所以这里的 a 的值是 window , a**.**a 也是window ， 即window**.**a = window ; window**.**a**.**a = window;
4.  foo(10) 和第一次一样，都是默认绑定，这个时候，**将window.a 赋值成 10** ，注意这里是关键，原来window.a = window ,现在被赋值成了10，变成了值类型，所以现在 a.a = undefined。(验证这一点只需要将var b = foo(10);删掉，这里的 a.a 还是window)
5.  var b = foo(10); 等价于 window.b = window;

本题中所有变量的值，a = window.a = 10 , a.a = undefined , b = window , b.a = window.a = 10;

3 .
```
var x = 10;
var obj = {
    x: 20,
    f: function(){ console.log(this.x); }
};
var bar = obj.f;
var obj2 = {
    x: 30,
    f: obj.f
}
obj.f();
bar();
obj2.f();
```
-----------------------答案---------------------

答案：20 10 30

解析：传说中的送分题，考点，辨别`this`绑定

```
var x = 10;
var obj = {
    x: 20,
    f: function(){ console.log(this.x); }
};
var bar = obj.f;
var obj2 = {
    x: 30,
    f: obj.f
}
obj.f();    // 20
            //有上下文，this为obj，隐性绑定
bar();      // 10
            //'光杆司令' 默认绑定  （ obj.f 只是普通的赋值操作 ）
obj2.f();   //30
            //不管 f 函数怎么折腾，this只和 执行位置和方式有关，即我们所说的绑定规则
```

4 .
```
function foo() {
    getName = function () { console.log (1); };
    return this;
}
foo.getName = function () { console.log(2);};
foo.prototype.getName = function () { console.log(3);};
var getName = function () { console.log(4);};
function getName () { console.log(5);}

foo.getName ();                // ?
getName ();                    // ?
foo().getName ();              // ?
getName ();                    // ?
new foo.getName ();            // ?
new foo().getName ();          // ?
new new foo().getName ();      // ?
```
-----------------------答案---------------------

答案：2 4 1 1 2 3 3

解析：考点 1. new绑定 2. 隐性绑定 3. 默认绑定 4. 变量污染

```
function foo() {
    getName = function () { console.log (1); }; 
            //这里的getName 将创建到全局window上
    return this;
}
foo.getName = function () { console.log(2);};   
        //这个getName和上面的不同，是直接添加到foo上的
foo.prototype.getName = function () { console.log(3);}; 
        // 这个getName直接添加到foo的原型上，在用new创建新对象时将直接添加到新对象上 
var getName = function () { console.log(4);}; 
        // 和foo函数里的getName一样, 将创建到全局window上
function getName () { console.log(5);}    
        // 同上，但是这个函数不会被使用，因为函数声明的提升优先级最高，所以上面的函数表达式将永远替换
        // 这个同名函数，除非在函数表达式赋值前去调用getName()，但是在本题中，函数调用都在函数表达式
        // 之后，所以这个函数可以忽略了

        // 通过上面对 getName的分析基本上答案已经出来了

foo.getName ();                // 2
                               // 下面为了方便，我就使用输出值来简称每个getName函数
                               // 这里有小伙伴疑惑是在 2 和 3 之间，觉得应该是3 , 但其实直接设置
                               // foo.prototype上的属性，对当前这个对象的属性是没有影响的,如果要使
                               // 用的话，可以foo.prototype.getName() 这样调用 ，这里需要知道的是
                               // 3 并不会覆盖 2，两者不冲突 ( 当你使用new 创建对象时，这里的
                               // Prototype 将自动绑定到新对象上，即用new 构造调用的第二个作用)

getName ();                    // 4 
                               // 这里涉及到函数提升的问题，不知道的小伙伴只需要知道 5 会被 4 覆盖，
                               // 虽然 5 在 4 的下面，其实 js 并不是完全的自上而下，想要深入了解的
                               // 小伙伴可以看文章最后的链接

foo().getName ();              // 1 
                               // 这里的foo函数执行完成了两件事, 1. 将window.getName设置为1,
                               // 2. 返回window , 故等价于 window.getName(); 输出 1
getName ();                    // 1
                               // 刚刚上面的函数刚把window.getName设置为1,故同上 输出 1

new foo.getName ();            // 2
                               // new 对一个函数进行构造调用 , 即 foo.getName ,构造调用也是调用啊
                               // 该执行还是执行，然后返回一个新对象，输出 2 (虽然这里没有接收新
                               // 创建的对象但是我们可以猜到，是一个函数名为 foo.getName 的对象
                               // 且__proto__属性里有一个getName函数，是上面设置的 3 函数)

new foo().getName ();          // 3
                               // 这里特别的地方就来了,new 是对一个函数进行构造调用,它直接找到了离它
                               // 最近的函数,foo(),并返回了应该新对象,等价于 var obj = new foo();
                               // obj.getName(); 这样就很清晰了,输出的是之前绑定到prototype上的
                               // 那个getName  3 ,因为使用new后会将函数的prototype继承给 新对象

new new foo().getName ();      // 3
                               // 哈哈，这个看上去很吓人，让我们来分解一下：
                               // var obj = new foo();
                               // var obj1 = new obj.getName();
                               // 好了，仔细看看, 这不就是上两题的合体吗,obj 有getName 3, 即输出3
                               // obj 是一个函数名为 foo的对象,obj1是一个函数名为obj.getName的对象
```

5 .
```javascript
var baz = 0;
let foo = {
    bar:function() {
        console.log(this,this.baz); 
        return this.baz;
    },
    baz:1
};
let foo2 = {
    baz:2
};

let a = foo.bar();  // 作为对象的方法调用，this指向调用函数的对象，即foo
let b = foo.bar.call(foo2); // 使用call方法将this绑定到第一个参数对象向，此时，this指向foo2
let fn = foo.bar;
console.log(fn);
let c = fn(); // let fn创建的对象，此时，fn = function(){...}，此时函数执行时，默认指向全局window对象
let d;
(function test(){
  d = arguments[0]()
})(foo.bar);   // arguments.callee包括了一个函数的引用去创建一个arguments对象，它能让一个匿名函数很方便的指向本身，即此时this指向arguments类数组对象
console.log(a,b,c,d);
```

---

### 參考
 - [MDN this](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)
 - [深入理解 js this 绑定](https://segmentfault.com/a/1190000011194676)
