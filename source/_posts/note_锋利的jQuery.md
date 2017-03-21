---
title: 锋利的jQuery笔记
date: 2017-02-05 19:52:53
tags:
---

## 第1章 认识jQuery

### 1.1 Javascript和Javascriipt库

#### 1.1.1 javascript简介

#### 1.1.2 javascript库作用及对比
作用：

<!--more-->

- 简化javascript开发
- 封装了很多预定义的对象和使用函数
- 轻松构建web2.0特性的富客户端页面
- 兼容各大浏览器

流行javascript对比：

- Prototype(https://www.prototypejs.org)年代过早，对面向对象编程思维把我的不是很到位，导致结构松散
- Dojo(http://dojotoolkit.org)提供了离线存储API、生成图标组件、基于SVG、VML的矢量图形库和Comet支持等。但是它学习难度大，文档不全，api不稳定
- YUI(http://developer.yahoo.com/yui/)市场上使用的也相对较多
- Ext JS(http://www.extjs.com)可以用来开发富有华丽外观的富客户端应用。侧重于界面比较臃肿，并且不是完全免费
- MooTools(http://mootools.net)清凉、简介、模块化和面向对象，只有8K，是个不错的jsvascript库。
- jQuery(http://jquery.com)轻量、强大选择器、出色DOM、可靠事件处理、完善的兼容性、链式操作，最受欢迎的js库。

### 1.2 加入jQuery

#### 1.2.1 jQuery简介

#### 1.2.2 jQuery的优势
写得少做的多(write less, do more)

1. 轻量级，压缩后30k左右。
2. 强大的选择器
3. 出色的DOM操作的封装
4. 可靠的事件处理的机制
5. 完善的Ajax
6. 不污染停机变量
7. 出色的浏览器兼容
8. 链式操作方式
9. 隐式迭代，无需循环遍历每一个返回的元素，只需设置集合
10. 行为层和结构层分离
11. 丰富的插件支持
12. 完善的文档
13. 开源

### 1.3 jQuery代码的编写

#### 1.3.1 配置jQuery环境

1. 获取jQuery最新版版
2. jQuery库类型说明： jquery.js(开发版约229k) jquery.min.js(压缩版约30k)
3. jQuery环境配置：只需在html文档中引入该库即可
4. 页面中引入jQuery
```javascript
<!DOCTYPE html>
<html>
<head>
	<title>demo</title>
</head>
<body>
	<script type="text/javascript" src="jquery.js"></script>
</body>
</html>
```

#### 1.3.2 编写简单的jQuery代码
```javascript
<script type="text/javascript" src="jquery.js"></script>
<script type="text/javascript">
	$(document).ready(function() {
		console.log('Hello, jQuery!');
	});
</script>
```

说明：

- window.onload必须等待网页中所有内容加载完毕才会执行（包括图片）并且不能同时含有多个window.onload
- $(document).ready()是网页中DOM结构绘制完毕后就会执行，可能DOm元素关联的东西并未加载完毕。并且同时可以写多个ready事件。每个都会执行
- 可以简写成$(function() {});

#### 1.3.3 jQuery代码风格

1. 链式操作风格
> 以一个实际项目代码为例，这是一个导航栏

```html

```

## 未完待续 