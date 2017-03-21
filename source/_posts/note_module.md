---
title: 模块化开发简介笔记
date: 2017-01-31 19:57:31
tags:
---

## 1. 概述

### 1.1 什么是模块化开发

- 将软件产品看作为一系列功能模块的组合
- 通过特定的方式实现软件所需模块的划分、管理、加载

### 1.2 为什么使用模块化开发

<!--more-->

- https://github.com/seajs/seajs/issues/547
- 协同开发
- 进度管理
- 单元测试
- 代码复用
- 解决问题
    + 大量的文件引入
    + 命名冲突
    + 文件依赖
        * 存在
        * 顺序


### 1.3 实现模块化的推演

#### step-01 全局函数写法

> 只要把不同的函数（以及记录状态的变量）简单的放在一起，就是一个模块

```javascript
function m1() {
  //...
}
function m2() {
  //...
}
```
上面的函数m1()和m2()，组成一个模块，使用的时候，直接调用就行了，

这种做法的缺点很明显：“污染”了全局变量，无法保证不与其他模块发生变量名冲突，而且模块成员之间看不出直接的关系。

#### step-02 封装对象写法

为了解决上面的缺点，可以把模块写成一个对象，所有的模块成员都放到这个对象里面去。

```javascript
var module1 = new Object({
  _count: 0,
  m1: function() {
    //...
  },
  m2: function() {
    //...
  }
});
```
上面的函数m1()和m2()都封装在module1对象里面，使用的时候，直接调用这个对象的属性和方法就好了。
```javascript
module1.m1();
```

但是这样的写法会暴露所有的模块成员，内部状态可以被外部改写，比如，外部代码可以直接改变内部的计数器的值。
```javascript
module1._count = 5;
```

#### step-03 立即执行函数写法
 
使用立即执行函数可以达到不暴露私有成员的目的
```javascript
var module1 = (function() {
  var _count = 0;
  var m1 = function() {
    //...
  };
  var m2 = function() {
    //...
  };

  return {
    m1: m1,
    m2: m2
  };

})();
```

使用上面的写法，外部代码无法读取内部的_count变量。
```javascript
console.info(module1._count);  //undefined
```

module1就是javascript模块的基本写法，下面，再对这种写法进行加工。

#### step-04 放大模式

如果一个模块很大，必须分成几个部分，或者一个模块需要继承另一个模块，这时就有必要采用“放大模式”

```javascript
var module1 = (function(mod) {
  mod.m3 = function() {
    //...
  };

  return mod;
})(module1);
```
上面的代码为module1添加了一个新的方法m3(),然后返回新的module1模块。

#### step-05 宽放大模式

在浏览器中，模块的各个部分都是从网上获取的，有时无法知道哪个模块会先加载，如果采用上一节的写法，第一个执行的部分有可能加载一个不存在的空对象，这时就要采用“宽放大模式”。

```javascript
var module1 = (function(mod) {
  var mod.m4 = function() {
    //...
  };

  return mod;
})(window.module1 || {});
```

#### step-06 输入全局变量

独立性是模块的重要特点，模块内部最好不与程序的其他部分直接交互。
为了在模块内部调用全局变量，必须显示的将其他变量输入模块。

```javascript
var module1 = (function($, YAHOO) {
  //...
})(jQuery, YAHOO);
```

上面的module1模块需要使用jQuery库和YUI库，就把这两个库（其实是两个模块）当做参数输入module1，这样做除了保证模块的独立性，还使得模块之间的依赖关系变得明显。

### 1.4 在什么场景下使用模块化开发
- 业务复杂
- 重用逻辑非常多
- 扩展性要求较高


*****


## 2. 实现规范

> 为什么模块很重要，因为有了模块，我们就可以更方便的使用别人的代码，想要什么功能就加载什么模块。
> 但是这样做有一个前提，那就是大家都必须以同样的方式编写模块，否则你有你的写法，我有我的写法，这样不是乱了套，考虑到javascript模块还没有官方规范，这一点就更重要了。
> 目前通行的javascript模块规范共有两种，CommonJS和AMD，CommonJS用于服务端，AMD基于CommonJS用于浏览器端。

### 2.1 CommonJS规范

> node.js的模块系统就是参照CommonJS规范实现的，在CommonJS中有一个全局性方法require()，用于加载模块，假定有一个数学模块math.js,就可以像下面这样加载。

```javascript
var math = require('math');
```

然后就可以调用模块提供的方法：

```javascript
var math = require('math');
math.add(2, 3);  //5
```

这里不做过多讨论，主要知道require()用于加载模块就行了。

### 2.2 AMD规范

#### AMD的由来
> 有了服务端模块以后，很自然地，大家就想要客户端模块，而且最好是两者都能兼容，一个模块不用修改，在服务器和浏览器都可以运行。

> 但是，由于一个重大的局限，使得CommonJS规范不适用于浏览器，还是上一个代码，如果在浏览器中运行，会有一个很大的问题

```javascript
var math = require('math');
math.add(2, 3);
```

第二行math.add(2, 3),在第一行require('math')之后运行，因此必须等math.js加载完成。也就是说，如果加载时间很长，整个应用就会停留在那里等。

这对浏览器不是问题，因为所有的模块都会放在本地硬盘，可以同步加载完成，等待时间取决于硬盘的读取速度。但是，对于浏览器，这是一个大问题，因为模块都放在服务器端，等待时间取决于网速的快慢，可能要等很长时间，浏览器处于“假死”状态。

因此浏览器端的模块不能采用“同步加载”，只能采用“异步加载”。这就是AMD规范诞生的背景。

#### AMD的简介

> AMD是“Asynchronous Module Definition”的缩写，意思就是“异步模块定义”。他采用异步方式加载模块，模块的加载不影响他后面语句的运行。所有依赖这个模块的语句，都定义在一个回调函数中，等到加载完成后，这个回调函数才会执行。

> AMD也采用require语句加载模块，但是不同于CommonJS，它要求两个参数：

```javascript
require([module], callback);
```

第一个参数[module], 是一个数组，里面的成员就是要加载的模块，第二个参数callback，则是加载成功之后的回调函数。如果将前面的代码改写成AMD形式，就是下面这个样子：

```javascript
require(['math'], function(math) {
  math.add(2, 3);
});
```

math.add()余math模块的加载不是同步的，浏览器不会发生假死，所以很显然，AMD比较适合浏览器环境。
目前，主要有两个Javascript库实现了AMD规范：require.js和curl.js.

### 2.3 CMD规范

> CMD是“Commond Module Definition”定义的缩写，由中国前端界大神“玉伯”和他的小伙伴搞出来的。CMD同AMD差别不大，都是干的一样的事情。CMD中一个模块就是一个文件，目前实现CMD规范的有sea.js

*****

