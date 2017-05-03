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

##### 1. 链式操作风格

> 以一个实际项目代码为例，这是一个导航栏

```html
//省略其他代码
<div class="box">
	<ul class="menu">
		<li class="level1">
			<a href="#none">衬衫</a>
			<ul class="level2">
				<li><a href="#none">短袖衬衫</a></li>
				<li><a href="#none">长袖衬衫</a></li>
				<li><a href="#none">短袖T恤</a></li>
				<li><a href="#none">长袖T恤</a></li>
			</ul>
		<li>
		<li class="level1">
			<a href="#none">卫衣</a>
			<ul class="level2">
				<li><a href="#none">开襟卫衣</a></li>
				<li><a href="#none">套头卫衣</a></li>
				<li><a href="#none">运动卫衣</a></li>
				<li><a href="#none">童装卫衣</a></li>
			</ul>
		<li>
		<li class="level1">
			<a href="#none">裤子</a>
			<ul class="level2">
				<li><a href="#none">短裤</a></li>
				<li><a href="#none">休闲裤</a></li>
				<li><a href="#none">牛仔裤</a></li>
				<li><a href="#none">免烫卡其裤</a></li>
			</ul>
		<li>
	</ul>
</div>
```

项目需求是做一个导航栏，单击不同的商品名称丽娜姐，显示相应的内容，同时高亮当前选择的商品。使用jQuery来实现如下：

```javascript
$("level1 > a").click(function() {
	$(this).addClass("current").next().show().parent().siblings().children("a").removeClass("current").next().hide();
	return false;
});
```

这就是jQuery的强大的链式编程，一行代码就搞定了。
虽然jQuery做到了行为和内容的分离，但是jQuery代码本身也是应有良好的层次结构及规范，这样才能进一步改善代码的可读性和可维护性。因此，推荐一种带有适当的格式的代码风格。

上面代码改成如下格式：
```javascript
$(".level > a").click(function() {
	$(this).addClass("current")
	.next().show()
	.parent().siblings().children("a").removeClass("current")
	.next().hide();
	return false;
})
```

代码格式调整后，易读性好多了。对于格式的建议，这里有3种：

1. 对于同一个对象不超过3个操作的，可以直接写成一行，代码如下：
```javascript
$("li").show().unbind("click");
```

2. 对于同一个对象的较多操作，建议写成多行，每行写一个,代码如下
```javascript
$(this).removeClass("mouseout")
	.addClass("mouseover")
	.stop()
	.fadeTo("fast", 0.6)
	.fadeTo("fast", 1)
	.unbind("click")
	.click(function() {
		//...do something
	})
```

3.对于多个对象的少量操作，可以每个对象写一行，如果涉及子元素，可以考虑适当的缩进，代码如下：
```javascript
$(this).addClass("highlight")
		.children("li").show().end()
	.siblings().removeClass("highlight")
		.children("li").hide();
```

##### 为代码添加注释

### jQuery对象和DOM对象

#### 1.4.1 DOM对象和jQuery对象简介

##### 1. DOM对象
> DOM, 每一个DOM都可以表示成一棵树，下面来构建一个非常基本的网页，代码如下：

```html
//省略其他代码
<h3>例子</h3>
<p title="选择你喜欢的水果。">你最喜欢的水果是？</p>
<ul>
	<li>苹果</li>
	<li>橘子</li>
	<li>菠萝</li>
</ul>
//省略其他代码
```

可以把上面的HTMl看成一刻DOM树，`<h3>`、`<p>`、`<ul>`以及`<ul>`的3个`<li>`子节点都是DOM元素的节点。可以通过javascript的getElementsByTagName或者getElementById来获取元素节点。像这样得到的DOM元素就是DOm对象。DOM对象可以使用javascript中的方法，代码如下：

```javascript
var domObj = document.getElementById("id");//获取DOM对象
var ObjHTML = domObj.innerHTML;//使用javascript中的属性----innerHTML
```

##### 2. jQuery对象

> jQuery对象就是通过jQuery包装DOM对象后产生的对象。

jQuery对象是jQuery独有的。如果一个对象是jQuery对象，那么就可以使用jQuery里的方法。例如：
```javascript
$("#foo").html();//获取id为foo的元素内的HTML代码。html()是jQuery里的方法。这段代码等同于：
document.getElementById("foo").innerHTML;
```
在jQuery对象中无法使用DOM对象的任何方法。例如$("#id").inerHTML和$("#id).checked之类的写法都是错误的，可以使用$("#id").html()和$("#id").attr("checked")之类的jQuery方法来代替。

同样，DOM对象也不能使用jQuery里的方法。例如document.getElementById(“id”).html()也会报错，只能用document.getElementById("id").innerHTML语句。


#### 1.4.2 jQuery对象和DOM对象的相互转换

> 在讨论jQuery对象和DOM对象的相互转换之前，先约定好定义变量的风格。如果获取的对象是jQuery对象，那么在变量前面加上$;如果获取的是DOM对象就还是之前的定义变量形式。

##### 1. jQuery对象转成DOM对象

> jQuery对象不能使用DOM中的方法，但是不得不使用DOM对象的时候，有以下两种方法可以处理。jQuery提供了两种方法将一个jQuery对象转换成DOm对象，即[index]和(index)

1. jQuery对象是一个类似数组的对象，可以通过[index]的方法得到相应的DOM对象。代码如下：
```javascript
var $cr = $("#cr");//jQuery对象
var cr = $cr[0];//DOM对象
alert(cr.checked)//检测这个checkbox是否被选中了
```

2. 另一种方法是jQuery本身提供的，通过get(index)方法得到相应的DOM对象。代码如下：
```javascript
var $cr = $("#cr");//jQuery对象
var cr = $cr.get(0);//DOM对象
alert(cr.checked);//检测这个checkbox是否被选中了
```

##### DOM对象转成jQuery对象
> 对于一个DOM对象，值需要用$()把DOM对象包装起来，就可以获得一个jQuery对象了。方式为$(DOM对象)。代码如下：

```javascript
var cr = document.getElementById("cr");//DOM对象
var $cr = $(cr); //jQuery对象
```

> 注意： 平时用到的jQuery对象都是通过$()函数制造出来的，$()函数就是一个jQuery对象的制造工厂。

### 1.5 解决jQuery和其他库的冲突

> 在jQuery库中，几乎所有的插件都被限制在他在命名空间里。通常，全局对象都被很好的存储在jQuery命名空间里，因此当把jQuery和其他Javascript库一起使用时，不会引起冲突。

#### 1. jQuery库在其他库之后引入

> 在其他库和jQuery库都被加载完毕后，可以在任何时候调用jQuery.noConflict()函数来将变量$的控制权移交给其他Javascript库。代码如下：
```javascript
//省略其他代码
<p id="pp">Test-prototype(将被隐藏)</p>
<p>Test-Jquery(将被绑定单击事件)</p>
<!--引入prototype-->
<script scr="lib/prototype.js" type="text/javascript"></script>
<script src="../../scripts/jquery.js" type="text/javascript"></script>
<script language="javascript">
	jQuery.noConflict();//将$的控制权交给prototype.js
	jQuery(function() {//使用jQuery 不适用$
		jQuery("p").click(function() {
			alert(jQuery(this).text());
		});
	});
	$("pp").style.display = 'none';//使用prototype.js隐藏元素
</script>
//省略其他代码。。。
```
交出了$的控制权之后就可以在程序中将jQuery()函数作为jQuery对象的制造工厂了。

如果想确保jQuery不会与其他库冲突，但又想自定义一个快捷方式，可以进行如下操作：
```javascript
//省略其他代码
var $j = jQuery.noConflict();//自定义一个快捷方式
$j(function() {
	$j("p").click(function() {
		alert($j(this).text());
	});
	$("pp").style.display = "none";//使用prototype.js隐藏元素
});
```
可以自定义备用名称，例如jq、$j、awesomequery等。

如果不想给jQuery自定义这些备份名称，还想使用$而不管其他库的$()方法，同时又不想与其他库冲突，那么可以使用一下两种解决方法。
```javascript
//方法1
jQuery.noConflict();//将变量$的控制权交出来
jQuery(function($) {//使用jQuery设定页面加载时执行的函数
	$("p").click(function() {//在函数内部继续使用$()方法
		alert($(this).text());
	});
});
$("pp").style.display = "none";

//方法2 自执行函数方式 这是我们所推荐的方式
jQuery.noConflict();
(function($) {
	$(function() {
		$("p").click(function() {
			alert($(this).text());
		});
	})
})(jQuery);
$("pp").style.display = 'none';
```

#### 2. jQuery在其他库之前导入

> 如果jQuery库在其他库之前就导入了，那么可以直接使用“jQuery”来做一些jQuery的工作。同时，也可以使用$()方法作为其他库的快捷方式。这里无需使用jQuery.noConflict()函数。代码如下：

```javascript
<p id="pp">Test-prototype</p>
<p>Test-jQuery(将被绑定的单击事件)</p>
<!--先导入jQuery-->
<script src="../../scripts/jquery.js" type="text/javascript"></script>
<script src="lib/prototype.js" type="text/javascript"></script>
<script language="javascript">
	jQuery(function() {//直接使用jQuery无需使用“jQuery.noConflict()”函数
		jQuery("p").click(function() {
			alert(jQuery(text).text());
		});
	});
	$("pp").style.display = "none";
<script>
```

### 1.6 jQuery开发工具和插件

*****

## 第2章 jQuery选择器

### 2.1 jQuery选择器是什么

#### 1.CSS选择器

#### 2.jQuery选择器

> jQuery中的选择器完全继承了CSS的风格。利用jQuery选择器，可以非常便捷和快速的找出特定的DOM元素，然后为他们添加相应的行为，而无需担心浏览器是否支持这个选择器。来看看代码：

```javascript
<script>
	function demo() {
		alert("Javascript demo");
	}
</script>
<p onclick="demo()">点击我</p>
```
这段代码为p元素添加一个onclick事件，当单机此元素的时候，会弹出一个对话框。这种方式没有将网页和内容分离，比较混乱。建议代码如下：
```javascript
<p class="demo">jQuery Demo</p>
<script type="text/javascript">
	$(".demo").click(function() {
		alert("jQuery demo!");
	});
</script>
```

jQuery选择器的写法和CSS的写法十分相似。

### 2.2 jQuery选择器的优势

#### 1.简洁的写法

#### 2.支持CSS1到CSS3选择器

#### 3.完善的处理机制

> 简单的说就是不会随便报错，即使没有找到元素也不会报错。
> 需要注意的是：$("#tt")获取的永远是对象，即使网页上没有此元素。因此当要用jQuery检查某个元素在网页上是否存在时，不能使用以下代码：

```javascript
if($("#tt")) {
	//do something
}
```
而应该根据获取到元素的长度来判断，代码如下
```javascript
if($("#tt").length > 0) {
	//do something
}
```
或者转成DOM来判断, 代码如下：
```javascript
if($("#tt")[0]) {
	//do something
}
```

### 2.3 jQuery选择器

> jQuery选择器分为基本选择器、层次选择器、过滤选择器和表单选择器。

#### 2.3.1 基本选择器

> 基本选择器是jQuery中最常用的选择器，也是最简单的选择器，它通过元素的id、class和标签名等来查找DOM元素。在网页中，每个id名称只能使用一次，class允许使用多次。基本选择器如下：

| 选择器 | 描述 | 返回 | 示例 |
|:--------:|:--------:|:--------:|:--------:|
| #id	   | 根据给定的id匹配一个元素 | 单个元素 | `$("#test")`选取id为test的元素 |
| .class   | 根据给定的类名匹配元素   | 集合元素 | `$(".test")`选取所有class为test的元素
| element  | 根据给定的元素名匹配元素 | 集合元素 | `$("p")`选取所有的`<p>`元素 |
| * 	   | 匹配所有元素			   | 集合元素 | `$("*")`选取所有的元素 |
| selector1, selector2,...,selectorN| 将每一个选择器匹配到的元素合并后一起返回 | 集合元素 | `$("div, span, p.myClass")`选取所有`<div>,<span>`和拥有class为myClass的`<p>`标签的一组元素

#### 2.3.2 层次选择器

> 如果想通过DOM元素之间的层次关系来获取特定元素，例如后代元素、子元素、相邻元素和同辈元素等，那么层次选取器是一个非常好的选择。层次选择器介绍如下：

| 选择器 | 描述 | 返回 | 示例 |
|:--------:|:--------:|:--------:|:--------:|
| `$("ancestor descendant")` | 选取ancestor元素里的所有descendant（后代）元素 | 集合元素 | `$("div span")`选取`<div>`里所有的`<span>`元素 |
| `$("parent > child")` | 选取parent元素下的child（子）元素 | 集合元素 | `$("div > span")`选取`<div>`元素下面元素名是`<span>`的直接子代元素 |
| `$("prev + next")` | 选取紧接在prev元素后的next元素 | 集合元素 | `$(".one + div")`选取class为one的下一个`<div>`同辈元素 |
| `$("prev~siblings")` | 选取prev元素之后的所有siblings元素 | 集合元素 | `$("#two~div")`选取id为two的元素后面的所有`<div>`同辈元素 |

等价代换关系：

> `$("prev + next")`选择器与`next()`方法的等价关系

 	$(".one + div"); == $("one").next("div");


> `nextAll()`方法来代替`$("prev~siblings")`选择器

	$("#prev~div"); == $("#prev").nextAll("div");

> 比较`siblings()`方法和`$('prev~siblings')`选择器

	$("prev~siblings")只能选择"prev"元素后面的同辈的<div>元素。而siblings()方法与前后位置无关，只要是同辈节点就能匹配得到。

#### 2.3.3 过滤选择器

> 过滤选择器主要是通过特定的过滤规则来筛选出所需的DOM元素，过滤规则和CSS中的伪类选择器语法相同，即选择器都是以一个冒号(:)开头。按照不同的过滤规则，过滤选择器可以分为基本过滤、内容过滤、可见性过滤、属性过滤、子元素过滤和表单对象属相过滤选择器。

##### 1 基本过滤选择器

| 选择器 | 描述 | 返回 | 示例 |
|:----:|:----:|:----:|:----:|
| :first | 选择第一个元素 | 单个元素 | `$("div:first")`选取所有`<div>`元素中的第1个`<div>`元素 |
| :last  | 选取最后一个元素 | 单个元素 | `$("div:last")`选取所有`<div>`元素中最后一个`<div>`元素 |
| :not(selector) | 去除所有与给定选择器匹配的元素 | 集合元素 | `$("input:not(.myClass)")`选取class不是myClass的`<input>`元素 |
| :even  | 选取索引是偶数的所有元素，索引从0开始 | 集合元素 | `$("input:even")`选取索引是偶数的`<input>`元素 |
| :odd   | 选取索引是奇数的所有元素，索引从0开始 | 集合元素 | `$("input:odd")`选取索引是奇数的`<input>`元素 |
| :eq(index) | 选取索引等于index的元素，索引从0开始 | 集合元素 | `$("input:eq(1)")`选取索引等于1的input元素 |
| :gt(index) | 选取索引大于index的元素，索引从0开始 | 集合元素 | `$("input:gt(1)")`选取索引大于1的`<input>`的元素 |
| :lt(index) | 选取索引小于index的元素，索引从0开始 | 集合元素 | `$("input:lt(1)")`选取索引小于index的`<input>`元素 |
| :header | 选取所有的标题元素，例如h1，h2，h3等等 | 集合元素 | `$(":header")`选取网页中所有的`<h1>, <h2>, <h3>......` |
| :animated | 选取当前正在执行动画的所有元素 | 集合元素 | `$("div:animated")`选取正在执行动画的`<div>`元素 |
| :focus | 选取当前获取焦点的元素 | 集合元素 | `$(":focus")`选取当前获取焦点的元素 |

##### 2.内容过滤选择器

> 内容过滤选取器的过滤规则主要体现在他所包含的子元素或文本内容上。介绍如下：

| 选取器 | 描述 | 返回 | 示例 |
|:----:|:----:|:----:|:----:|
| :contains(text) | 选取含有文本内容为text的元素 | 集合元素 | `$("div:contains('我')")选取含有文本“我”的<div>元素` |
| :empty | 选取不包含子元素或者文本的空元素 | 集合元素 | `$("div:empty")选取不包含子元素（包括文本元素）的<div>空元素` |
| :has(selector) | 选取含有选择器所匹配的元素的元素 | 集合元素 | `$("div:has(p)")选取含有<p>元素的<div>元素` |
| :parent | 选取含有子元素或者文本的元素，它和;empty相反 | 集合元素 | `$("div:parent")选取拥有子元素（包括文本元素）的<div>元素` |
|

##### 3. 可见性过滤选择器

> 可见性过滤选择器是根据元素的可见和不可见状态来选择相应的元素。如下：

| 选择器 | 描述 | 返回 | 示例 |
|:----:|:----:|:----:|:----:|
| :hidden | 选取所有不可见的元素 | 集合元素 | `$(":hidden")选取所有不可见的元素。包括<input type="hidden">, <div style="dispaly:none;">和<div style="visibility: hidden;">等元素。如果只想选取<input>元素，可以使用$("input:hidden")` |
| :visible | 选取所有可见的元素 | 集合元素 | `$("div:visible")选取所有可见的<div>元素` | 
|

##### 4. 属性过滤选择器

> 属性过滤选择器的过滤规则是通过元素的属性来获取相应的元素。介绍如下：

| 选择器 | 描述 | 返回 | 示例 |
|:----:|:----:|:----:|:----:|
| [attribute] | 选取拥有此属性的元素 | 集合元素 | `$("div[id]")选取拥有属性id的元素` |
| [attribute=value] | 选取属性的值为value的元素 | 集合元素 | `$("div[title=test]")选取属性title为“test”的<div>元素` | 
| [attribute!=value] | 选取属性的值不等于value的元素 | 集合元素 | `$("div[title!=test]")选取属性title不等于“test”的<div>元素（注意：没有属性title的<div>元素也会被选取）` |
| [attribute^=value] | 选取属性的值以value开始的元素 | 集合元素 | `$("div[title^=test]")选取属性title以“test”开始的<div>元素` |
| [attribute$=value] | 选取属性值以value结束的元素 | 集合元素 | `$("div[title$=test]")选取属性title以“test”结束的<div>元素` |
| [attribute*=value] | 选取属性值含有value的元素 | 集合元素 | `$("div[title*=value]")选取属性title含有“test”的<div>元素` |
| [attribute`|`=value] | 选择属性等于给定字符串或以该字符串为前缀（该字符串后跟一个连字符“-”）的元素 | 集合元素 | `$('div[title|="en"]')选取属性等于en或以en为前缀（该字符串后跟一个连字符“-”）的元素` |
| [attribute~=value] | 选择属性用空格分割的值中包含一个给定的元素 | 集合元素 | `$('div[title~="uk"]')选取属性title用空格分隔的值中包含字符uk的元素` |
| [attribute1][attribute2][attributeN] | 用属性选择器合成一个符合属性选择器，满足多个条件。每选择一次，缩小一次范围 | 集合元素 | `$("div[id][title$='test']")选取拥有属性id并且属性title以“test”结束的<div>元素` |

##### 5. 子元素过滤选择器

> 子元素过滤选择器的过滤规则相对于其他的选择器稍微有些复杂，不过没有关系，只要将元素的父元素和子元素区分清楚，那么使用起来也非常简单。还要注意他和图通的过滤选择器的区别。

| 选择器 | 描述 | 返回 | 示例 |
|:----:|:----:|:----:|:----:|
| :nth-child(index/even/odd/equation) | 选取每个父元素下的第index个子元素或者奇偶元素。(index从1算起) | 集合元素 | :eq(index)只匹配一个元素，而:nth-child将为每一个父元素匹配子元素，并且:nth-child(index)的index是从1开始的，而：eq(index)是从0算起的。|
| :first-child | 选取每个父元素的第一个子元素 | 集合元素 | `:first只返回单个元素，而:first-child选择符将为每个父元素匹配第一个子元素。例如$("ul li:first-child");选取每个<ul>中的第一个<li>元素` |
| :last-child | 选取每个父元素的最后一个子元素 | 集合元素 | `同样， :last只返回单个元素，而:last-child选择符将为每个父元素匹配最后一个子元素。例如$("ul li:last-child");选择每个<ul>中最后一个<li>` |
| :only-child | 如果某个元素时它父元素中唯一的子元素，那么将会被匹配。如果父元素中含有其他元素，则不会被匹配 | 集合元素 | `$("ul li:only-child")在<ul>中选取是唯一子元素的<li>元素` |
|

> :nth-child()选择器是很常用的子元素过滤器，详细如下：

- :nth-child(even)能选取每个父元素下的索引值是偶数的元素
- :nth-child(odd)能选取每个父元素下的索引值是奇数的元素
- :nth-child(2)能选取每个父元素下的索引值等于2的元素
- :nth-child(3n)能选取每个父元素下的索引值是3的倍数的元素（n从1开始）
- :nth-child(3n+1)能选取每个元素下的索引值是(3n+1)的元素。(n从1开始)

> eq(index)只能匹配一个元素，而:nth-child将为每一个符合条件的父元素匹配子元素。同时应该注意到nth-child(index)的index是从1开始的，而:eq(index)是从0开始的。同理：:first和:first-child, :last和:last-child也类似。

##### 6.表单对象属性过滤选择器

> 此选择器主要是对所选择的表单元素进行过滤，例如选择被选中的下拉框，多选框等元素。介绍如下：

| 选择器 | 描述 | 返回 | 示例 |
|:----:|:----:|:----:|:----:|
| :enabled | 选取所有可用元素 | 集合元素 | `$(“#form1:enabled”);选取id为form1的表单内所有可用元素` |
| :disabled | 选取所有不可用元素 | 集合元素 | `$("#form2:disabled");选取id为“form2”的表单内的所用不可用元素` |
| :checked | 选取所有被选中的元素（单选框，复选框）| 集合元素 | `$("input:checked");选取所有被选中的<input>元素` |
| :selected | 选取所有被选中的选项元素（下拉列表） | 集合元素 | `$("select option:selected");选取所有被选中的选项元素` |
|

#### 2.3.4 表单选择器

> 为了使用户能够更灵活的操作表单，jQuery中专门假如了表单选择器。利用这个选择器，能极其方便地获取到表单的某个或某类型的元素。

| 选择器 | 描述 | 返回 | 示例 |
|:----:|:----:|:----:|:----:|
| :input | 选取所有`<input>、<textarea>、<select>和<button>`元素 | 集合元素 | `$(":input")选取所有<input>、<textarea>、<select>和<button>元素` |
| :text | 选取所有的单行文本框 | 集合元素 | `$(":text")选取所有的单行文本框` |
| :password | 选取所有的密码框 | 集合元素 | `$(":password")选取所有的密码框` |
| :radio | 选取所有的单选框 | 集合元素 | `$(":radio")选取所有的单选框` |
| :checkbox | 选取所有的多选框 | 集合元素 | `$(":checkbox")选取所有的复选框` |
| :submit | 选取所有的提交按钮 | 集合元素 | `$(":submit")选取所有的提交按钮` |
| :image | 选取所有的图像按钮 | 集合元素 | `$(":image")选取所有的图像按钮` |
| :reset | 选取所有的重置按钮 | 集合元素 | `$(":reset")选取所有的重置按钮` |
| :button | 选取所有的按钮 | 集合元素 | `$(":button")选取所有的按钮` |
| :file | 选取所有的上传区域 | 集合元素 | `$(":file")选取所有的上传域` |
| :hidden | 选取所有不可见元素 | 集合元素 | `$(":hidden")选取所有不可见元素（已经在不可见性过滤选择器中讲解过）` |

### 2.4 应用jQuery改写示例

- 例子1： 给网页中所有的`<p>`元素添加onclick事件。
- 例子2： 使一个特定的表格隔行变色
- 例子3： 对多选框进行操作，输出选中的多选框的个数。

>　下面我们用刚学会的jQuery选择器以及隐式迭代的特性来重写这3个例子：

使用jQuery选择器写例子1：
```javascript
$("p").click(function() {//获取一个p元素 ，给每一个p元素添加单击事件
	//doing something
});
```

使用jQuery选择器写例子2：
```javascript
$("#tb tbody tr:even").css("backgroundColor", "#888");
/*
获取id为tb的元素，然后寻找它下面的tbody标签，再寻找tbody下索引值是偶数的tr元素，改变它的背景色。
*/
```

使用jQuery选择器写例子3：
```javascript
$("#btn").click(function() {
	//先使用属性选择器，然后用表单对象属性过滤，最后获取jQuery对象的长度
	var items = $("input[name='check']:checked");
	alert("选中的个数为：" + items.length);
});
```

### 2.5 选择器中的一些注意事项

#### 2.5.1 选择器中含有特殊符号的注意事项

1. 选择器中含有“.”、“#”、“(”或“]”等特殊字符

	根据W3C的规定，属性值中是不能含有这些特殊字符的，但在实际项目中偶尔会遇到表达式中含有“#”和“.”等特殊字符，如果按照普通的方式处理出来的话就会出错。解决此类问题的方法是使用转义符转义。

	 ```html
	 <div id="id#b">bb</div>
	 <div id="id[1]">cc</div>
	 ```
	 如果按照普通的方式来获取，例如：
	 ```javascript
	 $("#id#b");
	 $("#id[1]");
	 ```
	 以上代码不能正确获取到元素，正确的写法如下：
	 ```javascript
	 $("#id\\#b");//转义特殊字符"#"
	 $("#id\\[1\\]");//转义特殊字符"[ ]"
	 ```
	
2. 属性选择器的@符号问题

	在jQuery升级版本过程中，jQuery在1.3.1版本中彻底放弃了1.1.0版本遗留下的@符号，假如你使用1.3.1以上的版本，那么你不需要在属性前添加@符号：比如：
	```javascript
	$("div[@title='test']");
	```
	正确的书写时去掉@符号，比如：
	```javascript
	$("div[title='test']");
	```

#### 2.5.2 选择器中含有空格的注意事项

> 选择器中的空格也是不容忽略的，多一个空格或少一个空格也会得到截然不同的结果。如下代码：

```html
<div class="test">
	<div style="display:none;">aa</div>
	<div style="display:none;">bb</div>
	<div style="display:none;">cc</div>
	<div class="test" style="display:none;">dd</div>
</div>
<div class="test" style="display:none;">ee</div>
<div class="test" style="display:none;">ff</div>
```
如果使用以下jQuery选择器分别获取他们。
```javascript
var $t_a = $('.test :hidden');//带空格的jQuery选择器
var $t_b = $('.test:hidden');//不带空格的jQuery选择器
var len_a = $t_a.length;
var len_b = $t_b.length;
alert("$('.test :hidden') = " + len_a);//输出4
alert("$('.test:hidden') = " + len_b);//输出3
```
之所以出现上面的情况是因为过滤选择器和后代选择器不一样。

### 2.6 案例研究----某网站品牌列表效果

### 2.7 其他选择器

#### 2.7.1 jQuery提供的选择器的扩展

> 虽然jQuery提供的许多选择器，但还是有可能不能满足各种多变的业务需求，不过jQuery选择器是可以扩展的。

1. MoreSelectors for jQuery

	http://plugin.jquery.com/project/moreSelectors

2. Basic XPath

	http://plugins.jquery.com/project/xpath

#### 2.7.2 其他使用CSS选择器的方法

### 2.8 小结

## 第3章 jQuery中的DOM操作

### 3.1 DOM操作分类

> 一般来说，DOM操作分为3个方面，即DOM Core（核心）、HTML-DOM、和CSS-DOM。



## 未完待续 