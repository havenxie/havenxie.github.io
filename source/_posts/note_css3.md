---
title: CSS3笔记
date: 2016-12-25 20:37:17
tags:
---

## CSS3手册使用

- []    表示全部可选项
- ||    表示或者
- |     表示多选一

<!-- more -->
- ?     表示0个或1个
- '*'   表示0个或多个
- {}    表示范围

## css3选择器
>  css3新增了许多灵活查找元素的方法，极大地提高了查找元素的效率和精准度。css3选择器与jQuery中所提供的绝大部分选择器兼容。

### 2.1.属性选择器

1. E[attr]          表示存在attr属性
2. E[attr = val]    表示属性值完全等于val
3. E[attr ~= val]   表示一个单独属性，这个属性是以空格分割的($中没有这个)
4. E[attr |= val]   表示要么一个单独的属性值，要么这个属性值是以"-"分隔得
5. E[attr *= val]   表示属性值里包含val字符并且在“任意”位置
6. E[attr ^= val]   表示属性值里包含val字符并且在“开始”位置
7. E[attr $= val]   表示属性值里包含val字符并且在“结束”位置

### 2.2.伪类选择器

>  nth-child(n) 对应根据E元素确定的父元素的所有子元素，nth-of-type(n) 的不同之处在于其对应的只是同级的E元素。
>  n遵循线性变化，其取值1、2、3、4、... 关于n的取值范围：
>  1、当n做为一个独立值时，n取值为n>=1，例如nth-child(n).
>  2、当n做一个系数时，例如nth-child(2n+1)、nth-child(-n+5)此处2n+1或者-n+5做为一个整体不能小于1；

1. E:nth-child(n)    第n个元素，计算的方法是E元素的全部兄弟元素。
2. E:nth-last-child(n)    同E:nth-child(n) 计算顺序相反。
3. E:first-child     匹配父元素的第一个子元素E。E必须是父元素的第一个子元素。
4. E:last-child      匹配父元素的最后一个子元素E。E必须是父元素的最后一个元素。
5. E:only-child      匹配父元素仅有的一个子元素E。E必须是父元素的唯一子元素。
---------------------------------------------------------
6. E:nth-of-type(n)  同级中所有E元素中的第几个元素。
7. E:nth-last-of-type(n)  同E:nth-of-type(n)计算顺序相反
8. E:first-of-type   同级中所有E元素中的第一个。
9. E:last-of-type    同级中所有E元素中的最后一个。
10. E:only-of-type   同级中只有一个E类型元素。
---------------------------------------------------------
11. E:link           设置超链接a在未被访问前的样式。 
12. E:visited        设置超链接a在其链接地址已被访问过时的样式。
13. E:hover          设置元素在其鼠标悬停时的样式。
14. E:active         设置元素在被用户激活（在鼠标点击与释放之间发生的事件）
15. E:focus          设置对象在成为输入焦点(该对象的onfocus事件发生)时的样式。 
16. E:lang(fr)       匹配使用特殊语言的E元素。p:lang(en) {color: #090;}
17. E:not(s)         匹配不含有s选择符的元素E。
    ```css3
    .demo li:not(:last-child) {
        border-bottom: 1px solid #ddd;
    }
    /*给该列表中除最后一项外的所有列表项加一条底边线*/
    ```
18. E:empty          匹配没有任何子元素（包括text节点）的元素E
19. E:checked        匹配用户界面上处于选中状态的元素E。(用于input type为radio与checkbox时) 
20. E:enabled        匹配用户界面上处于可用状态的元素E。 
21. E:disabled       匹配用户界面上处于禁用状态的元素E。
22. E:target         :target选择器用于选取当前活动的目标元素。

### 2.3.伪元素选择器

1. E::selection      设置对象被选择时的样式.需要注意的是，::selection只能定义被选择时的background-color，color及text-shadow(IE11尚不支持定义该属性)。 
2. E::placeholder    设置对象文字占位符的样式。 用于控制表单输入框占位符的外观
3. E:before/E::before用来和content属性一起使用，并且必须定义content属性 
4. E:after/E::after  
5. E:first-letter/E::first-letter设置对象内的第一个字符的样式。 
>此伪对象仅作用于块对象。内联对象必须先将其设置为块级对象才能使用。常被用来配合font-size属性和float属性制作首字下沉效果。 
6. E:first-line/E::first-line设置对象内的第一行的样式。 
>此伪对象仅作用于块对象。内联对象要使用该伪对象，必须先将其设置为块级对象。 
IE6在使用该选择符时有个显式的BUG：选择符与包含规则的花括号之间不能紧挨着，需留有空格或换行

## 颜色

>  新增了RGBA、HSLA模式，其中的A 表示透明度通道，即可以设置颜色值的透明度，相较opacity，不具有继承性，即不会影响子元素的透明度。
>  R、G、B 取值范围0~255
>  H 取值范围0~360，0/360表示黑色、120表示绿色、240表示蓝色
>  S 取值范围0%~100%
>  L 取值范围0%~100%
>  A 取值范围0~1

关于透明度：
1. opacity子元素会继承父元素的透明度，在实际开发中会带来干扰；
2. transparent 设置透明度时完全类似于“玻璃”一样的透明；

## 文本

多行文本文字溢出处理，非标准属性，可应用于移动端，显示不完就会显示省略号(...)
```css
p {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 2;/*设置要显示的行数*/
    -webkit-box-orient: vertical;
    width: 80px;
}
<p>这里是要显示的内容，如果显示不完就会显示省略号</p>
```

文本基线对其方式：
vertical-align：baseline | sub | super | top | text-top | middle | bottom | text-bottom | `<percentage>` | `<length>`

- baseline： 把当前盒的基线与父级盒的基线对齐。如果该盒没有基线，就将底部外边距的边界和父级的基线对齐 
- sub： 把当前盒的基线降低到合适的位置作为父级盒的下标（该值不影响该元素文本的字体大小） 
- super： 把当前盒的基线提升到合适的位置作为父级盒的上标（该值不影响该元素文本的字体大小） 
- text-top： 把当前盒的top和父级的内容区的top对齐 
- text-bottom： 把当前盒的bottom和父级的内容区的bottom对齐 
- middle： 把当前盒的垂直中心和父级盒的基线加上父级的半x-height对齐 
- top： 把当前盒的top与行盒的top对齐 
- bottom： 把当前盒的bottom与行盒的bottom对齐 
- `<percentage>`： 把当前盒提升（正值）或者降低（负值）这个距离，百分比相对line-height计算。当值为0%时等同于baseline。 
- `<length>`： 把当前盒提升（正值）或者降低（负值）这个距离。当值为0时等同于baseline。（CSS2） 

### 其他的属性因为兼容性太差就算了

## 边框

>  其边框圆角、边框阴影属性，应用十分广泛，兼容性也相对较好，具有符合渐进增强原则的特征.

### 边框圆角

当圆角半径小于或等于边框宽度时，元素内角是直角

### 边框阴影

>  设置边框阴影不会影响盒子的布局，即不会影响其兄弟元素的布局 
>  spread可以与blur、h-shadow、v-shadow相互抵消，blur不可为负值
>  可以设置多重边框阴影，实现更好的效果，增强立体感。

`box-shadow：none | <shadow> [ , <shadow> ]*
<shadow> = inset? && <length>{2,4} && <color>?`

none： 无阴影 
水平偏移量：第1个长度值用来设置对象的阴影水平偏移值。可以为负值 
竖直偏移量：第2个长度值用来设置对象的阴影垂直偏移值。可以为负值 
模糊度：如果提供了第3个长度值则用来设置对象的阴影模糊值。不允许负值 
扩展：  如果提供了第4个长度值则用来设置对象的阴影外延值。可以为负值。和偏移量呈数学运算
颜色：  设置对象的阴影的颜色。 
inset： 设置对象的阴影类型为内阴影。该值为空时，则对象的阴影类型为外阴影 

盒子的阴影不会影响盒子的布局

### 边框图片

>  `border-image：<' border-image-source '> || <' border-image-slice '> [ / <' border-image-width '> | / <' border-image-width '>? / <' border-image-outset '> ]? || <' border-image-repeat '>`

取值：

- <' border-image-source '>：设置边框图像来源路径。 
- <' border-image-slice '>： 设置边框背景图的分割方式。 
- <' border-image-width '>： 设置边框厚度。 
- <' border-image-outset '>： 设置边框背景图的扩展。 
- <' border-image-repeat '>： 设置边框图像的平铺方式。
    + stretch: 用拉伸方式来填充边框背景图
    + repeat:用平铺方式来填充边框背景图。当图片碰到边界时，如果超过则会被截断。
    + round: 用平铺方式来填充边框背景图。会根据边框的尺寸动态调整图片的大小直至可以正好铺满整个边框。
    + space: 用平铺方式来填充边框背景图。会根据边框的尺寸动态调整图片之间的间距直至正好可以铺满整个边框。
    
## 盒模型

>  关于盒模型存在两种形式，分别是W3C标准盒模型和IE盒模型，如下图所示，其区别主要在于宽度和高度的计算方式，CSS3对盒模型做出了新的定义，即允许开发人员指定盒子宽度和高度的计算方式。

box-sizing: content-box | border-box | inherit;

- content-box: padding和border不被包含在定义的width和height之内。
- border-box:  padding和border被包含在定义的width和height之内。

## 背景

>  cover 会使“最大”边，进行缩放，另一边同比缩放，铺满容器，超出部分会溢出。
>  contain 会使“最小”边，进行缩放，另一边同比缩放，不一定铺满容器，会完整显示图片。
>  background-size会以background-clip设定的盒模型计算

background-clip : border-box | padding-box | content-box | text 就是设置图片真实显示出来的区域

- padding-box: 从padding区域(不包含padding)开始向外裁剪背景
- border-box:  从border区域(不包含border)开始向外裁剪背景
- content-box: 从content区域开始向外裁剪背景
- text: 从前景内容的形状(比如文字)作为裁剪区域向外裁剪，如此即可实现使用背景作为填充色之类的遮罩效果。

background-origin: border-box | padding-box | content-box就是图片在哪个区域放置

- padding-box: 从padding区域开始显示背景图像
- border-box： 从border区域开始显示背景图像
- content-box：从content区域开始显示背景图像

background-repeat: repeat-x | repeat-y | [repeat | no-repeat | space | round]{1,2} 就是设置图片平铺方式

- repeat-x: 背景图像在横向上平铺
- repeat-y: 背景图片在纵向上平铺
- repeat: 背景图片在横向和纵向上平铺
- no-repeat: 背景图片不平铺
- round: 背景图片自动缩放直到适应且填充整个容器
- space: 背景图片以相同的间距平铺且填充满整个容器或某个方向。

background-size: [length | percentage | auto]{1,2} | cover | contain

- length: 用长度指定背景图片的大小
- percentage: 用百分比指定背景图片的大小
- auto: 背景图像的实际大小
- cover： 将背景图片等比缩放到完全覆盖容器，背景图像有可能超出容器
- contain: 将背景图片等比缩放到宽度或高度与容器的宽度或高度相等，图片背景始终被包含在容器内。

## 渐变
>  线性渐变、径向渐变、重复渐变(包括重复线性渐变和重复径向渐变)。

### 线性渐变
>  linear-gradient()

- `<angle>`： 用角度值指定渐变的方向（或角度）。 
- to left： 设置渐变为从右到左。相当于: 270deg (向上为0度，角度按照顺时针旋转)
- to right： 设置渐变从左到右。相当于: 90deg 
- to top： 设置渐变从下到上。相当于: 0deg 
- to bottom： 设置渐变从上到下。相当于: 180deg。这是默认值，等同于留空不写。
- `<color-stop>`: 用于指定渐变的起止颜色：
- `<color>`： 指定颜色。 
- `<length>`： 用长度值指定起止色位置。不允许负值 
- `<percentage>`： 用百分比指定起止色位置。 

写法：
1. 标准写法：background-image: linear-gradient(yellow, green);//从黄色渐变到绿颜色（默认方向从上到下）
2. 多个颜色渐变：background-image: linear-gradient(yellow, green, red, blue);//默认方向从上到下
3. 用角度确定方向：background-image: linear-gradient(0, yellow, green);//方向从下到上
4. 用角度确定方向：background-image: linear-gradient(90deg, yellow, green);//方向从左到右
5. 用角度确定方向：background-image: linear-gradient(180deg, yellow, green);//方向从上到下
6. 用关键词确定方向：background-image: linear-gradient(to top, yellow, green);//方向从下向上
7. 用关键词确定方向: background-image: linear-gradient(to left top, yellow, green);//方向从右下到左上
8. 控制渐变：background-image: linear-gradient(to left, yellow 20%, green 40%, blue);//到百分之二十的时候是黄色，到百分之四十的时候是绿色 之后都是绿色（后面的值不能小于前面的值）

### 径向渐变

>  radial-gradient()

写法：
1. 普通写法：radial-gradient(yellow, green);//在中心，从黄色渐变到绿色
2. radial-gradient(120px at center center, yellow, green);//从中心点，渐变半径是120px，从黄色渐变到绿色。
3. radial-gradient(100px at 80px 80px,yellow, green);//在80px，80px处，渐变半径是100px，从黄色渐变到绿色。（坐标相对于左上角）
4. radial-gradient(120px 80px at center center, yellow, green);//在中心点，渐变范围是个椭圆，长轴是120px短轴是80px，从黄色渐变到绿色。
5. radial-gradient(circle at center center, yellow, green);//在中心处，渐变范围是一个圆，从黄色渐变到绿色。

### 重复线性渐变

> repeating-linear-gradient()

- 下述值用来表示渐变的方向，可以使用角度或者关键字来设置：
- `<angle>`： 用角度值指定渐变的方向（或角度）。 
- to left： 设置渐变为从右到左。相当于: 270deg 
- to right： 设置渐变从左到右。相当于: 90deg 
- to top： 设置渐变从下到上。相当于: 0deg 
- to bottom： 设置渐变从上到下。相当于: 180deg。这是默认值，等同于留空不写。 <- color-stop> 用于指定渐变的起止颜色：
- `<color>`： 指定颜色。 
- `<length>`： 用长度值指定起止色位置。不允许负值 
- `<percentage>`： 用百分比指定起止色位置。 

写法：
1. repeating-linear-gradient(#f00, #ff0 10%, #f00 15%);
2. repeating-linear-gradient(to bottom, #f00, #ff0 10%, #f00 15%);
3. repeating-linear-gradient(180deg, #f00, #f00 10%, #f00 15%);
4. repeating-linear-gradient(to top, #f00, #f00 10%, #f00 15%);

### 重复径向渐变

>  repeating-radial-gradient

写法：
1. background: repeating-radial-gradient(circle, #f00 0, #ff0 10%, #f00 15%);
2. background: repeating-radial-gradient(at top left, #f00, #ff0 10%, #080 15%, #ff0 20%, #f00 25%);
3. background: repeating-radial-gradient(circle closest-corner at 20px 50px, #f00, #ff0 10%, #080 20%, #ff0 30%, #f00 40%);

## 多列布局

> 多列布局常用在报纸或者新闻等网页

1. 设置分成几列：-webkit-column-count: 4;
2. 设置每列的宽度：-webkit-column-width: 400px;//当列数与列宽的乘积大于盒子总宽的时候会自动调整列数
3. 调整列之间的间隙宽度：-webkit-column-gap: 60px;
4. 设置列之间的分割线样式：-webkit-column-rule: 2px dashed #CCC;
5. 列数跨度，常用在文章标题：-webkit-column-span: all;//设置为all之后标题将会跨度所有的列。
6. 以上1~4条常作用在p标签，第五条常作用在h标签

## 伸缩布局

- flex: 复合属性，设置伸缩盒对象的子元素如何分配空间。
- flex-grow: 设置弹性盒的扩展比率
- flex-shrink：设置弹性盒的收缩比率
- flex-basis: 设置弹性盒伸缩基准值
- flex-flow: 复合属性，设置伸缩盒对象的子元素排列方式
- flex-direction: 设置伸缩盒对象的子元素的排列方向
- flex-wrap: 设置伸缩盒对象的子元素超出父容器时是否换行
- align-content: 设置伸缩盒堆叠伸缩行的对其方式
- align-items: 设置伸缩盒子元素在纵轴方向上的对齐方式
- align-self：设置弹性盒自身在纵轴方向上的对齐方式
- justify-content: 设置弹性盒子元素在主轴方向上的对齐方式
- order: 设置伸缩盒对象子元素出现的顺序。 

### flex：设置伸缩盒子元素如何分配剩余或者不够的空间
> flex：none | <' flex-grow '> <' flex-shrink >'? || <' flex-basis '>

- none： none关键字的计算值为: 0 0 auto 
- flex-grow : 用来指定扩展比率，即剩余空间是正值时此「flex子项」相对于「flex容器」里其他「flex子项」能分配到空间比例。(默认为1)
- 用来指定收缩比率，即剩余空间是负值时此「flex子项」相对于「flex容器」里其他「flex子项」能收缩的空间比例。在收缩的时候收缩比率会以伸缩基准值加权（默认值是1）
- flex-basis ： 用来指定伸缩基准值，即在根据伸缩比率计算出剩余空间的分布之前，「flex子项」长度的起始数值。如果自身的宽度没有定义，则长度取决于内容。 
    ```
    <ul class="flex">
        <li>a</li>
        <li>b</li>
        <li>c</li>
    </ul>
    .flex{display:flex;width:800px;margin:0;padding:0;list-style:none;}
    .flex :nth-child(1){flex:1 1 300px;}
    .flex :nth-child(2){flex:2 2 200px;}
    .flex :nth-child(3){flex:3 3 400px;}
    //父容器宽为800px,由于子元素设置了伸缩基准值flex-basis，相加300+200+400=900，那么子元素
    将会溢出900-800=100px；于同时设置了收缩因子，所以加权综合可得300*1+200*2+400*3=1900px；
    于是我们可以计算a,b,c将被移除的溢出量是多少：
    a被移除溢出量：(300*1/1900)*100，即约等于16px
    b被移除溢出量：(200*2/1900)*100，即约等于21px
    c被移除溢出量：(400*3/1900)*100，即约等于63px
    最后a,b,c的实际宽度分别为：300-16=284px, 200-21=179px, 400-63=337px
    ```

    ```
    <ul class="flex">
        <li>a</li>
        <li>b</li>
        <li>c</li>
    </ul>
    .flex{display:flex;width:1500px;margin:0;padding:0;list-style:none;}
    .flex :nth-child(1){flex:1 1 300px;}
    .flex :nth-child(2){flex:2 2 200px;}
    .flex :nth-child(3){flex:3 3 400px;}
    //父容器宽为1500px，由于子元素设置了伸缩基准值flex-basis，相加300+200+400=900，那么容器
    将有1500-900=600px的剩余宽度；于是我们可以计算a,b,c将被扩展量是多少：
    a的扩展量：(1/(1+2+3))*600，即约等于100px
    b的扩展量：(2/(1+2+3))*600，即约等于200px
    c的扩展量：(3/(1+2+3))*600，即约等于300px
    最后a,b,c的实际宽度分别为：300+100=400px, 200+200=400px, 400+300=700px

    ```

### flex-grow：用数据来定义扩展比率，不允许负值
> 根据弹性盒子元素所设置的扩展因子作为比率来分配剩余空间。默认为0。如果没有显示定义该属性，是不会拥有分配剩余空间权利的。


```
<ul class="flex">
    <li>a</li>
    <li>b</li>
    <li>c</li>
</ul>

.flex{display:flex;width:600px;margin:0;padding:0;list-style:none;}
.flex li:nth-child(1){width:200px;}
.flex li:nth-child(2){flex-grow:1;width:50px;}
.flex li:nth-child(3){flex-grow:3;width:50px;}
//例中b,c两项都显式的定义了flex-grow，flex容器的剩余空间分成了4份，其中b占1份，c占3分。
即1:3。flex容器的剩余空间长度为：600-200-50-50=300px，所以最终a,b,c的长度分别为：
a: 50+(300/4)=200px
b: 50+(300/4*1)=125px
a: 50+(300/4*3)=275px
```

### flex-shrink 用数值来定义收缩比率。不允许负值 
> 根据弹性盒子元素所设置的收缩因子作为比率来收缩空间。flex-shrink的默认值为1，如果没有显示定义该属性，将会自动按照默认值1在所有因子相加之后计算比率来进行空间收缩。

```
<ul class="flex">
    <li>a</li>
    <li>b</li>
    <li>c</li>
</ul>

.flex{display:flex;width:400px;margin:0;padding:0;list-style:none;}
.flex li{width:200px;}
.flex li:nth-child(3){flex-shrink:3;}
//例中c显式的定义了flex-shrink，a,b没有显式定义，但将根据默认值1来计算，可以看到总共将剩余
空间分成了5份，其中a占1份，b占1份，c占3分，即1:1:3。我们可以看到父容器定义为400px，子项被定
义为200px，相加之后即为600px，超出父容器200px。那么这么超出的200px需要被a,b,c消化通过收缩
因子，所以加权综合可得200*1+200*1+200*3=1000px；
于是我们可以计算a,b,c将被移除的溢出量是多少：
a被移除溢出量：(200*1/1000)*200，即约等于40px
b被移除溢出量：(200*1/1000)*200，即约等于40px
c被移除溢出量：(200*3/1000)*200，即约等于120px
最后a,b,c的实际宽度分别为：200-40=160px, 200-40=160px, 200-120=80px


```

### flex-basis
> 用长度或者百分比来自定义宽度，不允许出现负值。如果所有子元素的基准值之和大于剩余空间，则会根据每项设置的基准值，按比率伸缩剩余空间 

### flex-flow：<' flex-direction '> || <' flex-wrap '>
- <' flex-direction '>： 定义弹性盒子元素的排列方向。 
- <' flex-wrap '>： 控制flex容器是单行或者多行。 

#### flex-direction：row | row-reverse | column | column-reverse
- row： 主轴与行轴方向作为默认的书写模式。即横向从左到右排列（左对齐）。 
- row-reverse： 对齐方式与row相反。 
- column： 主轴与块轴方向作为默认的书写模式。即纵向从上往下排列（顶对齐）。 
- column-reverse： 对齐方式与column相反。 
```
<ul id="flex">
    <li>a</li>
    <li>b</li>
    <li>c</li>
</ul>

#box {
    display: flex;
    flex-direction: row-reverse;
    list-style: none;
}
```

#### flex-wrap：nowrap | wrap | wrap-reverse
> 该属性控制flex容器是单行或者多行，同时横轴的方向决定了新行堆叠的方向。 

- nowrap： flex容器为单行。该情况下flex子项可能会溢出容器 
- wrap： flex容器为多行。该情况下flex子项溢出的部分会被放置到新行，子项内部会发生断行 
- wrap-reverse： 反转 wrap 排列。

### align-content：flex-start | flex-end | center | space-between | space-around | stretch
> 当伸缩容器的侧轴还有多余空间时，本属性可以用来调准「伸缩行」在伸缩容器里的对齐方式，这与调准伸缩项目在主轴上对齐方式的 <' justify-content '> 属性类似。请注意本属性在只有一行的伸缩容器上没有效果。

### align-items：flex-start | flex-end | center | baseline | stretch
> 定义flex子项在flex容器的当前行的侧轴（纵轴）方向上的对齐方式。 

- flex-start： 弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴起始边界。 
- flex-end： 弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴结束边界。 
- center： 弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。 
- baseline： 如弹性盒子元素的行内轴与侧轴为同一条，则该值与'flex-start'等效。其它情况下，该值将参与基线对齐。 
- stretch： 如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制。 

### align-self：auto | flex-start | flex-end | center | baseline | stretch
> 定义flex子项单独在侧轴（纵轴）方向上的对齐方式。 

- auto： 如果'align-self'的值为'auto'，则其计算值为元素的父元素的'align-items'值，如果其没有父元素，则计算值为'stretch'。 
- flex-start： 弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴起始边界。 
- flex-end： 弹性盒子元素的侧轴（纵轴）起始位置的边界紧靠住该行的侧轴结束边界。 
- center： 弹性盒子元素在该行的侧轴（纵轴）上居中放置。（如果该行的尺寸小于弹性盒子元素的尺寸，则会向两个方向溢出相同的长度）。 
- baseline： 如弹性盒子元素的行内轴与侧轴为同一条，则该值与'flex-start'等效。其它情况下，该值将参与基线对齐。 
- stretch： 如果指定侧轴大小的属性值为'auto'，则其值会使项目的边距盒的尺寸尽可能接近所在行的尺寸，但同时会遵照'min/max-width/height'属性的限制

### justify-content：flex-start | flex-end | center | space-between | space-around
> 设置或检索弹性盒子元素在主轴（横轴）方向上的对齐方式。 