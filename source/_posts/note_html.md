---
title: HTML笔记
date: 2016-12-04 22:07:35
tags:
---

## [HTML部分]
### 1.文档头
- HTML是描述文档语义的语言，是英语HyperText Markup Language的缩写，超文本标记语言。

- <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"> 这是网页的声明头，即DocType fintion，简称DTD。网页的声明头可以告诉浏览器这是一个什么标准的页面，使用的是那种HTML或XHTML规范。

<!-- more -->
- HTML4.01是从IE6开始兼容的，HTML5是从IE9开始兼容的。

- HTML4.01规定了普通、XHTML这两大规范，每大种规范里面又各有3种小规范，所以总共6中规范（XHTML字母中的X表示“严格的”）：

    HTML4.01普通规范包括以下三种：

    + Strict         严格的，体现在一些标签不能使用，比如u、b、i(标签)
    + Transitional   普通的
    + Frameset       带有框架的页面
    
    XHTML1.0规范严格 体现在标签、属性必须是小写字母，标签必须封闭、必须使用引号……，包括一下三种：

    + Strict          严格的，体现在一些标签不能使用，比如u
    + Transitional    普通的（我们学习的版本）
    + Frameset      带有框架的页面
    
- HTML5中极大的简化了DTD，也就是说HTML5中就没有XHTML了。

- Doctype作用？

    告诉浏览器这是一个什么标准的文档应该以什么标准去执行及解析，如果Doctype不存在或者错误，浏览器将会以兼容模式去解析这个文档。

- 标准模式和兼容模式有什么区别？

    标准模式的排版和js执行都是以该浏览器支持的最高标准去执行，兼容模式情况下为了向后兼容将会采用该浏览器老式版本去排版和执行。

- HTML5 为什么只需要写 <!DOCTYPE html>？

    HTMl5不基于SGML(标准通用标记语言)，不需要对DTD进行引用，浏览器将会以对应格式进行排版和执行。
    HTML4.01基于SGML，需要引用DTD，才能让浏览器知道文档所使用的文档类型。

### 2.字符集

```html
<meta charset="UTF-8"> 
<meta http-equiv="Content-Type" content="text/html;charset=gb2312">
```

> 中文能够使用的字符集 UTF-8(国际通用字库)、gb2312(国标字库)、gbk。 注意：UTF-8里面存储一个汉字需要三个字节、gb2312中存储一个汉字需要两个字节。meta中写的和保存的要一致，否则浏览器会出现乱码。

### 3.meta关键字和页面描述

##### meta除了可以设置字符集，还可以设置关键字和页面描述。

```html
<meta name="Keywords" content="网易,邮箱,游戏,新闻,体育,娱乐,女性,亚运,论坛,短信" />
```

> 这些关键词，就是告诉搜索引擎，这个网页是干嘛的，能够提高搜索命中率。让别人能够找到你，搜索到你。

```html
<meta name="Description" content="网易是中国领先的互联网技术公司，为用户提供免费邮箱、游戏、搜索引擎服务，开设新闻、娱乐、体育等30多个内容频道，及博客、视频、论坛等互动交流，网聚人的力量。" />
```

> 只要设置的Description页面描述，那么搜索结果，就能够显示这些语句，这个技术叫做SEO，search engine optimization，搜索引擎优化。

### 4.title标签

`<title>网页的标题</title>`
> title也是有助于SEO搜索引擎优化的

### 5.favicon.ico图标

`<link rel="shortcut" href="favicon.ico" />`
> 链接站点的图标favicon.ico

### 5+.link标签

- link标签属于XHTML标签，除了加载css外，还可以加载less并且可以定义rel属性等作用。@import只能够加载css
- link在页面加载的时候同步被加载，@import会在html加载完毕之后才会加载对应的css文件

### 6.HTML基本语法特性

- HTML对换行不敏感，对tab不敏感，换不换行、tab不tab，都不影响页面的结构。

- 空白折叠现象。HTML中所有的文字之间，如果有空格、换行、tab都将被折叠为一个空格显示。

- 标签要严格封闭。

- HTML标签是分等级的，HTML将所有的标签分为两种：容器级、文本级。文本级的标签里面，只能放置文字、图片、表单元素（eg：`<p>xxx</p>`标签中只能放置文字、图片、表单元素。可以试着在p标签中放置h1标签，然后F2查看浏览器是如何处理的。）。

- `<img src="./demo.png" alt="图片加载失败时候显示的文字"/>`

- `<a href="./demo.html" title="鼠标悬停时候显示的文字" target="_blank">转到demo页面，并在新页面中打开</a>`

- a标签是一个文本类型标签，a标签的语义要小于p标签。如果一个段落中所有的文字都能够被点击应该是p包裹a：
    
    ```html
    <p>
        <a href="">落段落段落段落段落段落</a>
    </p>
    ```
    而不是a包裹p：
    ```html
    <a href="">
        <p>段落段落段落段落段落段落</p>
    </a>
    ```

- a标签无所不能，虽然它是文本类型标签但是它可放置容器级元素。但是不能包含a自己，也不能放置input等。

- 锚点用name属性来设置，一个a标签如果name属性（或者id属性），那么就是页面的一个锚点。`<a name="demo">我的作品</a>或者:<a id="demo">我的作品</a>`.

- 如果想跳转到某个锚点：`<a href="#demo">跳到“我的作品”锚点</a>` 

- href是hypertext reference“超文本地址”, 没有href的a标签不会有a标签默认的样式。作为锚点的a就不要加上href属性了。

<!-- #### h1标签的作用：给文本增加主标题的语义，有利于搜索引擎的搜索。 -->
### 7.HTML标签

#### 列表（无序列表、有序列表、定义列表）

###### 1.无序列表 `<ul>`：

```html
<ul>
    <li>北京</li>>
    <li>上海</li>>
    <li>广州</li>>
</ul>
```

li不能单独存在，必须包裹在ul或者ol里面，ul的直接子代元素不能是别的，只能是li，li是一个容器级标签里面什么都能放甚至可以再次嵌套ul，像下面这种是错误的：

```html
<ul>
    <h3>中国主要城市</h3>
    <li>北京</li>
    <li>上海</li>
    <li>深圳</li>
</ul>
```

###### 2.有序列表 `<ol>`：

```html
<ol>
    <li>算什么男人</li>
    <li>他不爱我</li>
    <li>明天的事情</li>
</ol>
```

浏览器会自动给ol中的li添加阿拉伯序号，ol里面只能是li，li必须被ol或者ul包裹。

###### 3.定义列表 `<dl>、<dt>、<dd>`：
定义列表也是一个标签组，出现三个标签：

- dl：表示definition list 定义列表
- dt：表示definition title 定义标题
- dd：表示definition description 定义描述词

dt、dd只能放在dl里面，dl里面只能有dt、dd; dt、dd都是容器级标签。

```html
<!-- 定义列表用法非常灵活，可以一个dt配很多dd;  -->
<dl>
    <dt>北京</dt>
    <dd>国家首都，政治、文化中心</dd>
    <dd>污染很严重，PM2.0天天报表</dd>
    <dt>上海</dt>
    <dd>国际化大都市、经济中心</dd>
    <dt>广州</dt>
    <dd>中国南大门、有珠江、小蛮腰</dd>
</dl>
---------------------------------------------------------
<!-- 还可以拆开，让每一个dl里面只有一个dt和dd，这样子感觉清晰一些。 -->
<dl>
    <dt>北京</dt>
    <dd>国家首都，政治文化中心</dd>
    <dd>污染很严重，PM2.0天天报表</dd>
</dl>
<dl>
    <dt>上海</dt>
    <dd>魔都，有外滩、东方明珠塔、黄浦江</dd>
</dl>
<dl>
    <dt>广州</dt>
    <dd>中国南大门、有珠江、小蛮腰</dd>
</dl>
```

#### div和span

- div是容器级标签，里面什么都能放置。

- span是文本级标签，里面只能放置文字、图片、表单元素。

- span里面不能放置h、ul、ol、div等这些容器级别标签，也不能放置p标签（文本级）

#### 表单 form
- 表单就是收集用户信息的，就是让用户填写的、选择的一些标签。

- 所有的表单内容，都要写在form标签里面。

- form标签里面有action属性和method属性。

    + action：表示表单提交到哪里
    + method：属性表示用什么HTTP方法提交，有get、post两种。

##### input

```html
文本输入框：<input type="text" name="" value="文本框里默认的值">
密码输入框：<input type="password" name="" id="" />
单选按钮：  <input type="radio" name="sex" id="" / checked="checked">男//默认被选上
            <input type="radio" name="sex" id="" />女
复选框：最好也是有相同的name（虽然他不需要互斥，但是也要有相同的name） 
    <p>
        请选择你的爱好：
        <input type="checkbox" name="hobby" id="" />吃饭
        <input type="checkbox" name="hobby" id="" />睡觉
        <input type="checkbox" name="hobby" id="" />打豆豆
    </p>
```

##### select、option

```html
select就是“选择”，option“选项”。select标签和ul、ol、dl一样，都是组标签。
<select>
    <option>北京</option>
    <option>河北</option>
    <option>河南</option>
</select>
```

##### textarea 多行文本框（文本域）
- cols属性表示columns“列”，
- rows属性表示rows“行”。
- 这个标签对里不需要写任何文字，如果写的话就是展现时候默认的内容。

```html
<textarea rows="10" cols="30"></textarea>

```

##### 按钮 button

- 普通按钮 `<input type="button" value="我是按钮上面显示的内容" />`

- 提交按钮 `<input type="submit" value="" />`点击之后表单就会被提交到form标签的action属性中的那个页面中去。

- 重置按钮 `<input type="reset" value="" />` 就是重置表单内容

##### label标签

```
//本质上讲，“男”、“女”这两个汉字，和input元素没有关系。
<input type="radio" name="sex" /> 男
<input type="radio" name="sex" /> 女
//label就是解决这个问题的。任何表单都可以和label配合使用
<input type="radio" name="sex" id="man" /><label for="man">男</label>
<input type="radio" name="sex" id="wom" /><label for="wom">女</label>
```

### html5
#### 语义化标签
> 用正确的标签做正确的事情

- 语义化标签让页面内容结构化
- 结构更清晰
- 便于浏览器、搜索引擎解析
- 便于屏幕阅读器读取，使页面更富文本化
- 有利于开发人员开发和维护人员维护

#### 新特性
> HTML5不再是SGML的子集， 主要增加了位置、图像、存储、多任务的等功能。

- 增加canvas
- 用于媒体的audio和video元素
- 本地离线存储localStorage可以长期保存
- sessionStorage的数据在浏览器关闭之后就自动消失
- 语义化标签header、footer、article、nav、section
- 表单控件calendar、date、time、email、url、search
- 新的技术webworker、websocket、Geolocation

#### 处理H5标签的兼容性
1. 使用document.createElement()方法产生新标签，但是需要为新标签增加默认样式
2. 最好的就是引用成熟框架html5shim
```html
<!--[if lt IE 9]>
<script type="text/javascript" src="html5shim.js"><
/script>
<![endif]-->
```

### 其他
#### 字符实体
+ `&lt;`表示<
+ `&gt;`表示>
+ `&copy;`表示版权符号©
+ `&nbsp;`表示空格，可以通过`&nbsp;&nbsp;&nbsp;`来解决空白折叠现象。