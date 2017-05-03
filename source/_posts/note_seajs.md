---
title: 模块化SeaJS笔记
date: 2017-02-19 22:09:43
tags:
---

> Sea.js 实现了CMD规范。在Sea.js中，所有的Javascript模块都遵循CMD模块定义规范。该规范明确了模块的基本书写格式和基本交互规则。

在CMD规范中，一个模块就是一个文件，代码的书写格式如下：

```javascript
define(factory);//factory工厂的意思
```

<!--more-->

### 1 使用步骤

1. 在页面中引入sea.js文件
2. 定义一个主模块文件，比如：main.js
3. 在主模块文件中通过define的方式定义一个模块，并导出公共成员
4. 在页面的行内脚本中通过seajs.use('path',fn)的方式使用模块
5. 回调函数的参数传过来的就是模块中导出的成员对象

*****

### 2 定义一个模块（define）

#### 2.1 define(factory)

> define是一个全局函数，用来定义模块。

> factory接受factory参数，factory可以是一个函数，也可以是一个对象或字符串。

factory为对象、字符串时，表示模块的借口就是该对象、字符串。比如可以定义如下一个JSON数据模块：

```javascript
define({"foo": "bar"});
```

也可以通过字符串定义模板模块：

```javascript
define('I am a template. My name is {{name}}');
``` 

factory为函数时，表示是模块的构造方法。执行该构造方法，可以得到该模块向外提供的借口。factory方法在执行时，默认会传入三个参数：require、exports、和module：

```javascript
define(function(require, exports, module) {
  exports.add = function(a, b) {
    return a + b;
  };
});
```

#### 2.2 define(id?, deps?, factory)

> define也可以接受两个以上参数，字符串id表示模块标识，数组deps是模块依赖。比如：

```javascript
define('Hello', ['jquery'], function(require, exports, module) {
	//模块代码
});
```

id和deps参数可以省略。省略时，可以通过构建工具自动生成。

注意：带id和deps参数的define用法不属于CMD规范，而属于Modules/Transport规范。

#### 2.3 define.cmd

> 一个空对象，可以用来判定当前页面是否有CMD模块加载：

```javascript
if(typeof define === 'function' && define.cmd) {
	//有Sea.js等CMD模块加载存在
}
```

*****

### 3 引用一个模块（require）

> require是factory函数的第一个参数

#### 3.1 require(id)

> require时一个方法，接受模块标识（id）作为唯一参数，用来获取其他模块提供的接口。

```javascript
define(function(require, exports) {
	//获取模块a的接口
	var a = require('./a');

	//调用模块a的方法
	a.dosomething();
});
```

注意：开发时，require的书写需要遵循一些简单约定。

#### 3.2 require.async(id, callback?)

> require.async 方法用来在模块内部异步加载模块，并且在加载完成后执行回调函数。callback参数可选。

```javascript
define(function(require, exports, module) {
	//异步加载一个模块，加载完成时，执行回调
	require.async('./b', function(b) {
		b.dosomething();
	});

	//异步加载多个模块，加载完成时，执行回调函数
	require.async(['./c', './d'], function(c, d) {
		c.dosomething();
		d.dosomething();
	});
});
```

注意： require是同步往下执行，require.async 则是异步回调执行。require.async 一般用来加载可延迟异步加载的模块。

#### 3.3 require.resolve(id)

> 使用模块系统内部的路径解析机制来解析并返回模块路径。该函数不会加载模块，只返回解析后的绝对路径。

```javascript
define(function(require, exports) {
	console.log(require.resolve('./b'));
	//===> http://example.com/path/to/b.js
});
```

这可以用来获取模块路径，一般用在插件环境或许动态拼接模块路径的场景下。

*****

### 4 向外输出模块接口（exports）

#### 4.1 exports

> exports是一个对象，用来向外提供模块接口

```javascript
define(function(require, exports) {
	//对外提供foo属性
	exports.foo = 'bar';

	//对外提供dosomething方法
	exports.dosomething = function() {};
});
```

除了给exports对象增加成员，还可以使用return直接向外提供接口

```javascript
define(function(require) {
	//通过return直接提供接口
	return {
		foo: 'bar',
		dosomething: function() {}
	};
});
```

如果return是function的唯一代码，还可以简化为：

```javascript
define({
	foo: 'bar',
	dosomething: function() {}
});
```

上面这种格式特别适合定义JSONP模块。

特别注意： 下面这种写法是错误的。

```javascript
define(function(require, exports) {
	//错误写法！
	exports = {
		foo: 'bar',
		dosomething: function() {}
	};
});
```

正取的写法是用return或者给module.exports赋值：

```javascript
define(function(require, exports, module) {
	//正确写法
	module.exports = {
		foo: 'bar',
		dosomething: function() {}
	};
});
```

提示： exports 仅仅是module.exports的一个引用。在factory内部给exports重新赋值时，并不会改变module.exports的值。因此给exports赋值是无效的，不能用来更改模块接口。

### 5 向外输出模块接口（module）

> module是一个对象，上面存储了与当前模块相关联的一些属性和方法

#### 5.1 module.id

> 模块的唯一标识

```javascript
define('id', [], function(require, exports, module) {
	//模块代码
});
```

上面代码中，define的第一个参数就是模块标识。

#### 5.2 module.uri

> 根据模块系统的路径解析规则得到模块的绝对路径

```javascript
define(function(require, exports, module) {
	console.log(module.uri);
	//===> http://example.com/path/to/this/file/js
});
```

一遍情况下，没有在define中手写id参数时，module.id的值就是module.uri, 两者完全相同。

#### 5.3 module.dependencies 

> dependencies 是一个数组，表示当前模块的依赖。

#### 5.4 module.exports 

> 当前模块对外提供的接口

传给factory构造方法的exports参数时module.exports对象的一个引用。只通过exports参数来提供接口，有时无法满足开发者的所有需求。比如模块的接口是某个类型的实例时，需要通过module.exports来实现：

```javascript
define(function(require, exports, module) {
	//exports是module.exports的一个引用
	 console.log(module.exports === exports);//true

	 //重新给module.exports赋值
	 module.exports = new SomeClass();

	 //exports不再等于module.exports
	 console.log(module.exports === exports);//false

});
```

注意：对module.exports的赋值需要同步进行，不能放在回调函数里。下面这样是不行的：

```javascript
//x.js
define(function(require, exports, module) {
	//错误用法
	setTimeout(function() {
		module.exports = {a: 'Hello'};
	}, 0);
});
```

在y.js里调用到上面的x.js

```javascript
y.js
define(function(require, exports, module) {
	var x = require('./x');

	//无法立即得到模块x的属性a
	console.log(x.a);// undefined
});
```

*****

### 6 主入口seajs.use

+ 一般用于入口模块
+ 一般只会使用一次
+ require和module.require用于模块与模块之间


### 7 使用第三方依赖（jQuery）

- 由于CMD是国产货，jquery默认不支持
- 改造

```javascript
// 适配CMD
if (typeof define === "function" && !define.amd) {
  // 当前有define函数，并且不是AMD的情况
  // jquery在新版本中如果使用AMD或CMD方式，不会去往全局挂载jquery对象
  define(function() {
    return jQuery.noConflict(true);
  });
}
```

### 8 Seajs配置

- [配置](https://github.com/seajs/seajs/issues/262)
- seajs.config
  + base
  + alias

### 9 使用案例

- 未完待续


