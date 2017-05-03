---
title: CSS笔记
date: 2016-12-11 23:02:27
tags:
---

## [CSS部分]
> CSS具有继承性和层叠性
> CSS层叠式样式表  从审美的角度负责页面样式。
  层叠式含义：一个标签可以被多个css选择器选择，共同作用；

<!--more-->

### 清楚默认样式

```css
body,div,dl,dt,dd,ul,ol,li,h1,h2,h3,h4,h5,h6,pre,code,form,fieldset,legend,input,textarea,p,blockquote,th,td{
    margin:0;
    padding:0;
}
```

### 兼容性问题

#### IE版本和操作系统的关系

```javascript
windows xp    IE6
windows vista IE7
windows 7     IE8
windows 8     IE9
windows 8.1   IE10
windows 10    IE11/edge
```

#### IE6可以兼容的选择器：

```
p             标签选择器
#             id选择器
.spec         类选择器
div.box       交集选择器
div .box      后代选择器
div, .box     并集选择器
*             通配符选择器
```

#### IE7开始兼容的选择器

```
div > p       直接子代选择器
div + p       紧邻兄弟选择器
```

#### IE8开始兼容的选择器

```
div p:first-child  选择父元素的第一个子元素，并且这个子元素还要是p
div p:last-child   选择父元素的最后一个子元素，并且这个子元素还要是p
```

#### 12px bug，IE6不支持小于12px的盒子，任何小于12px的盒子在IE6中看都大。

> 解决办法很简单，就是将盒子的字号设置小于盒子的高度，比如0px。

```
height: 4px;
_font-size: 0px;//只要给css属性之前加上下划线，那么这个属性就是IE6认识的，其他浏览器不会认识。
```

#### IE6 支持overflow:hidden;但是不支持用它来清除浮动。 解决办法：追加一条_zoom:1;

```
overflow:hidden;
_zoom:1;//IE6不能用overflow:hidden;来清除浮动，可以使用_zoom:1;
```

#### IE6双倍margin bug
当出现连续浮动的元素，携带和浮动方向相同的margin时，队首的元素会有双倍margin的问题。
```
<ul>
    <li class="first"></li>
    <li></li>
    <li></li>
</ul>
```

解决办法：

1. 使浮动的方向和margin的方向相反。这也应该做为一个习惯
```
float: left;
margin-right: 40px;
```

2. 单独给队首元素写一个样式
```
ul li.first {
    _margin-left: 20px;   
}
```

### id选择器命名
- 只能有字符、数字、下划线
- 必须以字母开头
- 不能和标签同名
- 不能重名，同一个页面内id不能重复
- 严格区分大小写

### CSS选择器

#### 儿子选择器(直接子代选择器) >

```html
//IE7开始兼容，IE6不兼容。
div > p {
    color: red;
}
```

#### 序列选择器
> IE8开始兼容，IE6、7都不兼容。

```
ul li:first-child {/*选择父元素的第一个子元素，并且这个子元素还要是li*/
    color: red;
}
ul li:last-child {/*选择父元素的最后一个子元素，并且这个子元素还要是li*/
    color: blue;
}
```

#### 紧邻兄弟选择器
> +表示紧邻兄弟选择器
> IE7开始兼容，IE6不兼容。

```
h3 + p {/*选择h3后面紧挨着的那个p元素*/
    text-decoration: none;
}
```

### CSS的继承性和层叠性

#### 继承性

- 有一些属性，当给自己设置的时候，自己的后代都继承上了，这个就是继承性。
- color、text-开头的、line-开头的、font-开头的都会被继承。(a标签除外)
- 这些关于文字样式的都能够继承；所有关于盒子的、定位的、布局的属性都不能够继承。

#### 权重问题

- 如果不能直接选中某个元素，只是通过继承性影响的话，那么权重是0。
- 如果权重相同那么采用就近原则。
- 并集选择器（分组选择器）要拆开计算，不能合着计算。

```
#box1 div.spec2 p, #box1 #box2 p {//前面一个权重是1,1,2 后面一个权重是2,0,1
    color: blue;
}
```

#### !important标记

```
p {
    color: red !important;
}
#par {
    color: blue;
}
.spec {
    color: green;
}
```

1. k: e !important;来给一个属性提高权重。这个属性的权重就是无穷大。

2. 注意!important提升的是一个属性而不是一个选择器。如下三个选择器都选择到目标：

    ```
    p{
        color:red !important;   → 只写了这一个!important，所以只有颜色属性提升权重
        font-size: 100px ;  → 这条属性没有写!important，所以没有提升权重
    }
    #para1{
        color:blue;
        font-size: 50px;
    }
    .spec{
        color:green;
        font-size: 20px;
    }
    /*所以，综合来看，字体颜色是red（听important的）；字号是50px（听id的）*/
    ```

3. !important无法提升继承的权重，该是0的还是0.如下：
 
    ```
    <div>
        <p>这里是要显示的文字</p>
    </div>
    ---------------------------------------------------
    div {
        color: red !important;
    }
    p {
        color: blue;
    }
    /*虽然div的样式设置了!important并且p会继承div的样式，但是p的样式是实实在在的选中的（以p为准）。*/
    ```

4. !important不会影响就近原则

    ```
    <div>
        <ul>
            <li>这里该是什么颜色</li>
        </ul>
    </div>
    --------------------------------------------------
    div {
        color: red !important;
    }
    ul {
        color: blue;
    }
    /*这里的li颜色是继承来自ul，虽然div中加了!important,但是不会影响li的就近原则继承。*/
    ```

5. !important会让css样式很乱，所以不建议随便使用。

### 盒模型
 盒模型box model，什么是盒子？所有的标签都是盒子。无论div、span、a都是盒子。图片、表单一律看做文本。
 盒模型的组成成员包括：width、height、padding、border、margin。

#### 1.盒子中的区域

> 一个盒子中主要的属性就5个：width、height、padding、border、margin。
> width是“宽度”的意思，CSS中width指的是内容的宽度，而不是盒子的宽度。
> height是“高度”的意思，CSS中height指的是内容的高度，而不是盒子的高度。
> padding是“内边距”的意思,padding的区域有背景颜色padding: 20px 30px 40px;(上右下)。
> border是“边框”。
> margin是“外边距”。

#### 2.宽和高

#### 3.border
- 就是边框。边框有三个要素：粗细、线型、颜色。
- 颜色如果不写，默认是黑色。另外两个属性不写，显示不出来边框。
```
border: 1px dotted red;    /*点线  */
border: 1px dashed red;    /*虚线  */
border: 1px solid red;     /*实线  */
border: 1px double red;    /*双线  */
border: 1px groove red;    /*凹槽  */
border: 1px ridge red;     /*凸槽  */
border: 1px inset red;     /*内嵌型*/
border: 1px outset red;    /*外开型*/
/*用的比较多的是solid、dashed、dotted*/
```
- border属性能够被拆分开，有两大拆分方式：
    + 按三要素：border-width border-style border-color
        ```
        border-width:10px 20px;
        border-style:solid dashed dotted;
        border-color:red green blue yellow;
        ```
    + 按照方向；border-top border-right border-bottom border-left
        ```
        border-top:10px solid red;
        border-right:10px solid red;
        border-bottom:10px solid red;
        border-left:10px solid red;
        ```
    + 按方向还能再拆一层，就是把每个方向的，每个要素拆开，一共12条语句
        ```
        border-top-width:10px;
        border-top-style:solid;
        border-top-color:red;
        border-right-width:10px;
        border-right-style:solid;
        ```
- 其他
```
border: none;//border可以没有
border-left:none;//某一个边是没有的
border-left-width:0;//某一个边设置成0
```

#### 4.margin
- margin的塌陷现象，在标准文档流中，竖直方向上的margin不叠加，以较大的为准。
- 如果不在标准流中，比如盒子都浮动了，那么两个盒子之间没有塌陷现象。此时margin叠加。
- 盒子居中 margin: 0 auto;
    + 使用margin: 0 auto;的盒子，必须有明确的width。
    + 只有在标准文档流中，才能使用margin: 0 auto;居中。也就是说当一个盒子浮动、绝对定位、固定定位了之后就不能使用margin来居中了。
    + margin: 0 auto;是居中盒子而不是居中文本。文本的居中要使用text-align: center;
- 如果父亲没有border，那么儿子的margin实际上是作用在文档流上的而不是作用在父元素上的，这样父元素也会跟着移动。
    ```
    <div>
        <p></p>//这个p有一个margin-top踹父亲，试图将自己下移，最后父亲也跟着掉下来了。
    </div>
    ```
- IE6的双倍margin问题。解决方案：使浮动的方向和margin的方向，相反。

### 标准文档流

#### 块级元素和行内元素分类（p比较特殊：他是文本级却是块级）

- 块级元素
    + 霸占一行，不能与其他任何元素并列
    + 能接受宽、高
    + 如果不设置宽度，那么宽度将默认变为父亲的100%。
- 行内元素
    + 与其他行内元素并排
    + 不能设置宽、高。默认的宽度，就是文字的宽度。
- HTML中将元素分为文本级和容器级
    + 文本级：p、span、a、b、i、u、em
    + 容器级：div、h+、li、dt、dd
- CSS中分为块级和行内元素（文本级元素基本等于行内元素，只是p变了）
    + 行内元素：span、a、b、i、u、em
    + 块级元素：div、h+、li、dt、dd、p

#### 块级元素和行内元素的相互转换
块级元素可以设置为行内元素
行内元素可以设置为块级元素
```
div{
   display: inline;
   background-color: pink;
   width: 500px;
   height: 500px;
}
//那么，这个标签将立即变为行内元素。此时它和一个span无异：此时这个div不能设置宽度、高度；此时这个div可以和别人并排了。
----------------------------------------------------------------
span{
   display: block;
   width: 200px;
   height: 200px;
   background-color: pink;
}
//让标签变为块级元素。此时这个标签，和一个div无异：此时这个span能够设置宽度、高度此时这个span必须霸占一行了，别人无法和他并排；如果不设置宽度，将撑满父亲
```

行内块元素inline-block
兼容性写法：
    ```css
    display: inline-block;
    *display: inline;
    *zoom: 1;
    ```

标准流里面限制非常多，标签的性质恶心。比如，我们现在就要并排、并且就要设置宽高。所以，移民！脱离标准流！

### 脱离文档流
css中一共有三种手段，使一个元素脱离标准文档流：
1. 浮动 float: left;或者 float: right;
2. 绝对定位
3. 固定定位

#### 浮动

> 浮动宏观的看就是做“并排”用的。有几个性质：脱标、贴边、字围、收缩。

##### 1浮动的元素脱离标准文档流
浮动是css里面布局用的最多的属性。浮动的元素脱离文档流。也就是说，一个元素一旦浮动了就能脱离文档流了，此时就可以并排排列并且可以设置宽高，无论它原来是div、a还是span。已经浮动的元素就不需要再设置display：block;或者display:inline;了。
```
box1{
    float: left;
    width: 300px;
    height: 400px;
    background-color: yellowgreen;
}
.box2{
    float: left;
    width: 400px;
    height: 400px;
    background-color: skyblue;
}
```
这样box1和box2两个元素就可以并排排列了，并且两个元素都可以设置宽高。

##### 2浮动的元素互相贴靠

##### 3浮动的元素有“字围”效果
```
<div>
    <img src="./demo.jpg" alt="这是一张测试图片">
</div>
<p>这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。这里都是测试文字。</p>
```
让div浮动，p不浮动，div挡住了p，但是p中的文字不会被挡住，形成“字围”效果。

##### 4浮动的元素有收缩的特性
一个浮动的元素，如果没有设置width，那么将自动收缩为文字的宽度（这点非常像行内元素）。eg:
```
div{
    float: left;
    background-color: gold;
}
//这个div浮动了，且没有设置宽度，那么将自动缩紧为内容的宽度：
```

#### 清除浮动
清除浮动的方法分为： 父元素加高、clear:both;、隔墙法、父元素加overflow:hidden;
现在有两个div，div身上没有任何属性。每个div中都有li，这些li都是浮动的。我们本以为这些li，会分为两排，但是，第二组中的第1个li，去贴靠(贴边)第一组中的最后一个li了。原因就是因为div没有高度，不能给自己浮动的孩子们，一个容器。
```
    <div>
        <ul>
            <li>HTML</li>
            <li>CSS</li>
            <li>JS</li>
            <li>HTML5</li>
            <li>设计模式</li>
        </ul>
    </div>
    <div>
        <ul>
            <li>学习方法</li>
            <li>英语水平</li>
            <li>面试技巧</li>
        </ul>
    </div>
```
   
##### 清除浮动方法1.给浮动的元素的祖先元素加高度
如果一个元素要浮动，那么他的祖先元素一定要有高度。有高度的盒子才能关住浮动。只要浮动在一个有高度的盒子中，那么这个浮动就不会影响后面的浮动元素。所以就是清除浮动带来的影响了。
##### 清除浮动方法2.clear:both;
实际的网页制作中高度都是被内容撑开的，很少主动去设定高度。也就是说方法一很少使用。clear:both;就是清除左右浮动，使自己内部的元素不受其他盒子影响。但是会导致margin失效。
```
   <div>
    <ul>
        <li>HTML</li>
        <li>CSS</li>
        <li>JS</li>
        <li>HTML5</li>
        <li>设计模式</li>
    </ul>
</div>
<div class="box2">  → 这个div写一个clear:both;属性 
    <ul>
        <li>学习方法</li>
        <li>英语水平</li>
        <li>面试技巧</li>
    </ul>
</div>
```
##### 清除浮动方法3.隔强法
1. 外墙法：隔墙法就是用一个元素（如：div）隔开两部分的浮动，使两部分的浮动相互不会影响。当然了，这个墙的margin就失效了。

    ```
    <div class="box1">
        <ul>
            <li>HTML</li>
            <li>CSS</li>
            <li>JS</li>
            <li>HTML5</li>
            <li>设计模式</li>
        </ul>
    </div>
    <div class="cl h16"></div>  ->在这里设置一道墙从而阻隔上下两部分浮动。
    <div class="box2">
        <ul>
            <li>学习方法</li>
            <li>英语水平</li>
            <li>面试技巧</li>
        </ul>
    </div>
    -------------------------------------------------------
    .cl {
        clear: both;
    }
    .h16 {
        height: 16px;
    }
    ```
2. 内墙法：当两个p元素都浮动，所以div不能被撑出高度，而在家里修一堵墙能够让div被儿子撑出高度。

    ```
    <div>
        <p></p>
        <p></p>
        <div class="cl"></div> ->在家里修一堵墙撑出父亲的高度。
    </div>
    ```

##### 方法4.overflow:hidden;
 overflow:hidden;本来是设置溢出部分隐藏的，但是却发现它有一个特效：一个父元素不能被它浮动的儿子撑出高度，但是只要给父元素设置overflow:hidden;之后就能被儿子撑出高度了。
    ```
    div {//父元素
        width: 400px;
        border: 10px solid black;
        overflow:hidden;-->设置这个属性之后就可以被撑出高度了。
        _zoom:1;//兼容IE6，IE6支持overflow:hidden;但是不支持overflow:hidden的这个hack
    }
    ```

#### 清除浮动总结：
1. 父级div加高
2. 结尾处加空div标签clear: both
3. 父级div定义伪类:after和zoom
4. 父级div定义overflow: hidden
5. 父级div定义overflow: auto
6. 父级div也浮动，需要定义快读
7. 父级div定义display: table
8. 结尾处加br标签clear:both

### 行高和字号
#### 行高
1. CSS中所有的行都有行高，盒模型的padding绝不是作用在文字上的，而是作用在“行”上的。
2. 单行文本垂直居中：使行高==盒子高`p{height: 60px; line-height: 60px;}`
3. 行高=盒子高的小技巧只能适用于单行文本垂直居中，不适用于多行文本。如果想要设置多行文本垂直居中需要设置盒子的上下padding，把文本挤到中间。

#### 字号font
1. 使用font属性，能够将字号、行高、字体一起设置。
2. 格式font: 14px/28px "宋体";
等价于下面三行语句：
    font-size: 14px;
    line-height: 28px;
    font-family: "宋体";
3. 网页中我们使用最多的就是 微软雅黑、宋体、黑体、Arial、Times New Roman。
4. 为了防止用户电脑里面，没有微软雅黑这个字体。就要用英语的逗号，隔开备选字体。font-family: "微软雅黑","宋体";
5. 我们要将英语字体，放在最前面，这样所有的中文，就不能匹配英语字体，就自动的变为后面的中文字体：font-family: "Times New Roman","微软雅黑","宋体";
6. 所有的中文字体，都有英语别名：font-family: "Microsoft YaHei"; font-family: "SimSun";
7. font属性能将font-size、font-weight、font-family三者合为一体：font:12px/24px "TimesNewRoman","Microsoft YaHei","SimSun";
8. 行高可以用百分比，表示字号的百分之多少。一般来说都是大于100%，因为行高要大于字体。
    font:12px/200% "宋体";
等价font:12px/24px "宋体";

### 超级链接的美化
#### 伪类
a标签有4种伪类:
```
//注意：这四种伪类必须要按照固定的顺序书写，如果不按照固定顺序就会失效。"love hate"记忆法
a:link{    //用户没有点击过这个链接的样式。
    color:red;
}
a:visited{ //用户访问过了这个链接的样式。
    color:orange;
}
a:hover{   //用户鼠标悬停的时候链接的样式。
    color:green;
}
a:active{  //用户用鼠标点击这个链接，但是不松手，此刻的样式。
    color:black;
}
```
#### 超级链接的美化
一定要将a标签写在前面，:link、:visited、:hover、:active这些伪类写在后面。
a标签中，描述盒子； 伪类中描述文字的样式、背景。
```
nav ul li a{
    display: block;
    width: 120px;
    height: 40px;
}
.nav ul li a:link ,.nav ul li a:visited{
    text-decoration: none;
    background-color: yellowgreen;
    color:white;
}
.nav ul li a:hover{
    background-color: purple;
    font-weight: bold;
    color:yellow;
}
```
记住，所有的a不继承text、font这些东西。因为a自己有一个伪类的权重。

a:link、a:visited都是可以省略的，简写在a标签里面。也就是说，a标签涵盖了link、visited的状态。
```
nav ul li a{
    display: block;
    width: 120px;
    height: 50px;
    text-decoration: none;
    background-color: purple;
    color:white;
}
.nav ul li a:hover{
    background-color: orange;
}
```


### background系列属性

#### 1.background-color属性

#### 2.background-image属性
1. 用于给盒子加上背景图片：background-image: url("images/demo.jpg");
2. 背景默认是会被平铺满的。如果不想平铺请使用background-repeat属性。

#### 3.background-repeat属性
设置背景图是否重复的，重复方式的。也就是说，background-repeat属性，有三种值：
```
    background-repeat: no-repeat; //不重复
    background-repeat: repeat-x;  //横向重复
    background-repeat: repeat-y;  //纵向重复
```

#### 4.background-position属性
背景定位属性。
1. background-position: 向右移动量 向下移动量 eg: background-position: 20px -98px;
2. background-position: left top;//用单词来表述位置(left right top right center) 

#### 5.background-attachment属性
背景是否固定
background-accachment: fixed;//设置背景固定，不会被滚动条拖走。

#### 6.background综合属性
```
background: red url("images/demo.jpg") no-repeat 100px 100px fixed;
//等价于：
background-color: red;
background-image: url("images/demo.jpg");
background-repeat: no-repeat;
background-position: 100px 100px;
background-attachment: fixed;
//其实也可以任意省略部分
```

### 定位
定位有三种，相对定位position:relative;、绝对定位position:absolate;、固定定位position: fixed;

#### 相对定位
相对定位就是微调元素的位置，让元素相对于自己原来的位置进行位置调整。

1. 相对定位的盒子不会脱离标准流，本质上讲，相对定位的盒子仍然位于原来的位置，相对定位后的盒子只是一个影子。

2. 相对定位有坑，一般只用于微调元素、作为绝对定位的参考。

3. 定位值：
```
    position: relative;
    top: 10px;  //向下移动
    left: 40px; //向右移动
-------------------------------------
    position: relative;
    right: 100px; //向左移动
    top：100px;   //向下移动
-------------------------------------
    position: relative;
    right: 100px; //向左移动
    bottom: 100px;//向上移动
-------------------------------------
    position: relative;
    top: -200px;  //向上移动
    right: -200px;//向右移动
------------------------------------
    position: relative;
    right: -300px;//向右移动
    bottom: 300px;//向上移动

```


#### 绝对定位

1. 决定定位的盒子是脱离标准流的，所有标准流的性质，决定定位之后都不遵循了。

2. 决定定位之后，标签就不区分所谓的块级元素、行内元素了，不需要display:block;就可以设置宽高了。

    ```
        span {
            position: absolute;
            top: 100px;
            left: 100px;
            width: 100px;
            height: 100px;
            background-color: pink;
        }
    ```

3. 决定定位的参考点（用top描述）就是页面的左上角，而不是浏览器的左上角。

4. 绝对定位的参考点（用bottom描述）就是浏览器的首屏幕的左下角，而不是页面的左下角。

5. 以盒子为参考点
    - 一个绝对定位的元素，如果父辈元素中也出现了定位(无论是相对定位、绝对定位还是固定定位)的元素，那么将以这个父辈元素为参考点。
    - 要听最近的已经定位的祖先元素的，不一定是父亲，可能是爷爷：  
        ```
        <div class="box1">   →  相对定位
            <div class="box2">   →  没有定位
                <p></p>   → 绝对定位，将以box1为参考，因为box2没有定位，box1就是最近的父辈元素
            </div>
        </div>
        ```
    - 不一定是相对定位，任何定位，都可以作为参考点
        ```
        <div>  → 绝对定位
            <p></p>  → 绝对定位，将以div作为参考点。因为父亲定位了。
        </div>
        ```
    - 绝对定位的盒子，无视参考的那个盒子的padding。
    - 绝对定位的盒子居中：绝对定位的盒子脱离标准文档流，所以margin: 0 auto;不再有效，应该使用position: absolute; left: 50%; margin-left: '宽度的一半'。
        ```
        position: absolute;
        left: 50%;
        top: 0;
        margin-left: -300px;   → 宽度的一半
        ```

#### 固定定位

固定定位就是相对浏览器窗口的定位，无论页面如何滚动，这个盒子显示的位置都不会改变。

固定定位脱离标准文档流。

IE6不兼容

#### z-index (定位了的元素才可以使用，别的元素和浮动的元素不能使用)
z-index: 999; //没有单位

- z-index值表示谁压着谁。数值大的压盖住数值小的。
- 只有定位了的元素才能使用z-index，也就是说，不管是相对定位、固定定位、绝对定位都可以使用z-index。但是浮动的元素不能使用z-index。
- z-index没有单位，默认是0
- 如果大家都没有z-index，那么谁写在后面，谁在上面就能压住别人。定位了的元素能够压住没有定位的元素。
- 从父现象，如果父亲怂了，儿子再牛逼也没有用。

