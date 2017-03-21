---
title: DOM笔记
date: 2017-01-08 22:56:31
tags:
---

## DOM和BOM
每个载入浏览器的HTML都会成为Document对象。
Document对象使我们可以从脚本中对HTML页面中的所有元素进行访问。
Document对象是Window对象的一部分。可通过Window.document对其进行访问。

<!-- more -->

### DOM节点关系
父节点：parentNode

兄弟节点：(兼容写法var nextE = E.nextElementSibling || E.nextSibling;)

- nextSibling           下一个兄弟节点 IE678认识
- nextElementSibling    下一个兄弟节点 其他浏览器认识
- previousSibling       上一个兄弟节点 IE678认识
- previousElementSibling上一个兄弟节点 其他浏览器认识

子节点：(兼容写法var lastE = E.lastElementChild || E.lastChild;)

- firstChild            第一个孩子 IE678认识
- firstElementChild     第一个孩子 其他浏览器认识
- lastChild             最后一个孩子 IE678认识
- lastElementChild      最后一个孩子 其他浏览器认识

孩子节点：childNodes选出全部的孩子
childNodes：它是标准属性，它返回指定元素的子元素集合，包括HTML节点，所有属性，文本节点。
```
//火狐、谷歌等高本版会把换行和注释也看做是子节点。所以下面的ul中将会有7个节点。
<ul id="ul">
    <li>A</li>
    <li>B</li>
    <li>C</li>
</ul>
//利用nodeType == 1 时候才是元素节点来获取元素节点。nodeType==2属性节点 nodeType == 3文本节点
-----------------------------------------------------
//ie 678  包含注释节点，在ie678下，ul将会有4个节点
<ul id="ul">
    <li>A</li>
    <li>B</li>
    <li>C</li>
    <!-- <li>C</li> -->
</ul>
```

```html
<body>
<div class="nodes">
    <div>1</div>
    <div>2</div>
    <div>3</div>
    <!-- <div>4</div> -->
</div>
<script type="text/javascript">
    window.onload = function() {
        var nodes = document.getElementsByClassName('nodes')[0].childNodes;//会获得9个node。3个div，5个text，一个注释
        for(var index in nodes) {
            if(nodes[index].nodeType == 1) {//获取标签节点
                console.log(nodes[index]);
            }
        }
    };
</script>
</body>
```

### DOM节点操作

- 创建节点：var div = document.createElement("li");

- 创建文本节点: var txt = document.createTextNode()

- 插入节点：

    + appendChild()  eg: demo.appendChild(newItem);添加节点，添加到demo节点所有孩子的最后面
    + insertBefore() eg: demo.insertBefore(newItem, existingItem);添加节点，添加到existingItem的前面。如果第二个参数为null则添加到最后。

- 移除节点：removeChild()
```javascript
var da = document.getElementById("test");
var demo.removeChild(da);//demo节点中移除id为“test”的节点。
```

- 克隆节点：cloneNode()

    + cloneNode() 方法创建节点的拷贝，并返回该副本。
    + cloneNode() 方法克隆所有属性以及它们的值。
    + 如果您需要克隆所有后代，请把 deep 参数设置 true，否则设置为 false。

    eg: var node = document.getElementById('test').lastChild.cloneNode(true);
        document.getElementById('demo').appendChild(node);

- 替换节点：
    
    + replaceChild()

- 获取节点属性：

    getAttribute("属性"); 通过这个方法可以得到某些元素的某些属性。
    eg: demo = getAttribute('title'); //获取属性title的内容。

- 设置节点属性：
    
    setAttribute("属性","值"); 通过这个方法可以设置指定属性的值
    eg: demo.setAttribute("class","test" );//给demo节点设置class为test的样式。

- 删除节点属性：
    removeAttribute("属性"); 删除某个节点的指定属性
    eg: demo.removeAttribute("title"); //把demo元素的title属性给删除了。

- offset获取对象自己的尺寸（这里的父级不仅仅指的是亲爸爸）
offsetWidth和offsetHeight获取元素自己的宽度和高度，和其他元素无关。
不用Element.style.width的原因是它只能获取行内样式的数值。
    + offsetWidth = width + border + padding
    + offsetHeight = height + border + padding
    + offsetLeft和offsetTop:返回距离上级盒子(最近的带有定位的)左边(顶部)的位置，如果上级盒子都没有定位就以body为准。
    + 从父级盒子边框的内侧开始算，就是子盒子外边框到父级盒子内边框的距离。
    + offsetParent返回该对象的父级(带有定位position为absolute、relative、fixed)的那个盒子，如果没有定位的父级就返回body。
        ```
        #fa {
            position: absolute;
        }
        ----------------------------------
        <div id="fa">
            <div id="son"></div>
        </div>
        ----------------------------------
        var son = document.getElementById("son");
        alert(son.offsetParent.tagName);  //弹出div。 tagName:标签的名字
        ```
    
    + offsetLeft 和style.left的区别
        * offsetLeft可以返回没有定位的元素距离左侧的距离，而style.left不可以。
        * offsetLeft返回的是数值(如：400)，而style.left返回的是字符串(如：400px)。
        * offsetLeft只读，而style.left可读写。
        * style.left只能得到行内样式，而offsetLeft随便。

- scroll 家族 
    + scrollTop   被卷去的头部，它就是当你滑动滚轮浏览网页的时候网页隐藏在屏幕上方的距离
    + window.onscroll = function() { 页面滚动语句  }
    + 兼容写法：var scrollTop = window.pageYOffset || document.documentElement.scrollTop || document.body.scrollTop || 0;
    + scrollWidth: 大小是内容的大小width+padding
    + scrollTo(x,y)  window.scrollTo(15,15); 法可把内容滚动到指定的坐标。

- client 家族 client可视区域
    + clientWidth: width + padding (不包含border)
    + clientX: 可视区域坐标。
    
- 检测屏幕宽度（可视区域，浏览器内部的大小）
    + ie9及其以上的版本 window.innerWidth
    + 标准模式 document.documentElement.clientWidth
    + 怪异模式 document.body.clientWidth
    + 需要根据不同模式做兼容，然后自己封装一个函数。

- 检测屏幕宽度（分辨率）
    clientWidth返回的是可是区域的大小 浏览器内部的大小
    window.screen.width返回的是我们的电脑的分辨率，跟浏览器没有关系。

### DOM事件对象
event就是事件的对象，这个对象中包含着所有与事件有关的信息。普通浏览器支持event,ie678支持window.event。兼容写法：var event = event || window.event; 

- event常见属性如下：
    ```
    data:       返回拖拽对象的url字符串 （dragDrop）
    width:      该窗口的宽度
    height:     该窗口的高度
    pageX:      鼠标相对该网页的水平距离（ie无）
    pageY:      鼠标相对该网页的垂直距离（ie无）
    screenX:    光标相对于该屏幕的水平距离
    screenY:    光标相对于该屏幕的垂直距离
    target:     该事件被传送到的对象
    type:       事件类型
    clientX:    鼠标相对于该网页的水平距离（当前可见区域）
    clientY:    鼠标相对于该网页的垂直距离
    ```

- pageX  clientX  screenX 区别
    + screen X   screenY   是以我们的电脑屏幕为基准点测量 
    + pageX  pageY   以我们的文档（绝对定位）的基准点对齐 ie678 不认识  
    + clientX   clientY 以可视区域为基准点类似于    固定定位  

- 指向常用事件的具体事件如下： 

    ```
    onclick                 //鼠标单击
    ondblclick              //鼠标双击
    onkeyup                 //按下并释放键盘上的一个键时触发 
    onchange                //文本内容或下拉菜单中的选项发生改变
    onfocus                 //获得焦点，表示文本框等获得鼠标光标。
    onblur                  //失去焦点，表示文本框等失去鼠标光标。
    onmouseover             //鼠标悬停，即鼠标停留在图片等的上方只会出发一次
    onmousemove             //鼠标移动，没移动一个像素都会触发
    onmouseout              //鼠标移出，即离开图片等所在的区域
    onmouseup               //当鼠标弹起  
    onmousedown             //当鼠标按下的时候 
    onload                  //网页文档加载事件
    onunload                //关闭网页时
    onsubmit                //表单提交事件
    onreset                 //重置表单时
    ```

- onmouseover和onmousemove的区别
    + 相同点：都是经过目标元素才会触发 
    + onmouseover只触发一次 
    + onmousemove每移动一像素，就会触发一次 

这里有个经典的滚动条拖拽的代码
```
<style>
    * {margin:0;padding:0;}
    .scroll {
        width: 400px;
        height: 8px;
        background-color: #ccc;
        margin: 100px;
        position: relative;
    }
    .bar {
        width: 10px;
        height: 22px;
        background-color: #369;
        position: absolute;
        top: -7px;
        left: 0;
        cursor: pointer;
    }
    .mask {
        width: 0;
        height: 100%;
        background-color: #369;
        position: absolute;
        top: 0;
        left: 0;
    }
</style>
-------------------------------------------------------------------------------
<div class="scroll" id="scrollBar">
    <div class="bar"></div> <!--拖动条-->
    <div class="mask"></div><!--遮罩层-->
</div>
<div id="demo"></div>
-------------------------------------------------------------------------------
<script>
    var scrollBar = document.getElementById("scrollBar");
    var bar  = scrollBar.children[0];
    var mask = scrollBar.children[1];
    var demo = document.getElementById("demo");
    bar.onmousedown = function(event) {
        var event = event || window.event;
        var leftVal = event.clientX - this.offsetLeft;
        var that = this;
        document.onmousemove = function(event) {
            console.log(that.offsetLeft);
            var event = event || window.event;
            that.style.left = event.clientX - leftVal  + 'px';
            var val = parseInt(that.style.left);
            if(val < 0) {
                that.style.left = 0;
            } else if(val > 390) {
                that.style.left = "390px";
            }
            mask.style.width = that.style.left;  // 遮罩盒子的宽度
            demo.innerHTML = "已经走了:"+ parseInt(parseInt(that.style.left) / 390 * 100) + "%";
            window.getSelection ? window.getSelection().removeAllRanges() : document.selection.empty();
        }
        document.onmouseup = function() {
            document.onmousemove = null;   // 弹起鼠标不做任何操作
        }
    }
</script>
```

### 冒泡

#### 事件冒泡
事件冒泡: 当一个元素上的事件被触发的时候，比如说鼠标点击了一个按钮，同样的事件将会在那个元素的所有祖先元素中被触发。这一过程被称为事件冒泡；这个事件从原始元素开始一直冒泡到DOM树的最上层。
- ie6.0 冒泡顺序：    div -> body -> html -> document
- 其他浏览器冒泡顺序：div -> body -> html -> document -> window
- 不是所有的事件都能冒泡。以下事件不冒泡：blur、focus、load、unload

#### 阻止事件冒泡
- 标准浏览器：event.stopPropagation()
- IE浏览器：  event.cancelBubble = true;
- 兼容写法：  
    ```
    if(event && event.stopPropagation) {
        event.stopPropagation();    //标准浏览器
    } else {
        event.cancelBubble = true;//ie浏览器、
    }
    ```

### 判断当前对象
- 标准浏览器event.target.id
- ie678 event.srcElement.id
- 兼容写法：var target = event.target ? event.target.id : event.srcElement.id;

### 获得用户选择的内容
- 标准浏览器window.getSelection()
- ie678 document.selection.createRange().text;
- 兼容写法：
    ```
    var txt = '';
    if(window.getSelection) {
        txt = window.getSelection();
    } else {
        txt = document.selection.createRange().text;
    }
    ```

### 得到(获取)css的样式
通过box.style.left、box.style.backgroundColor这样的style方式只能得到css的行内样式，但是我们使用最多的就是内联式和外链式，ie和opera常用obj.currentStyle 其他浏览器常用window.getComputedStyle("元素","伪类");这两个参数是必须的没有伪类就用null代替。

兼容写法：
```
//这种方法只能获取不能设置
var demo = document.getElementById('demo');
function getStyle(obj, attr) {
    if(obj.currentStyle) {
        return obj.currentStyle[attr];
    } else {
        return window.getComputedStyle(obj, null)[attr];
    }
}
console.log(getStyle(demo, "width"));
```

### 设置css样式

- 利用点语法： box.style.width = "100px";
- 利用 [] :box.style['width'] = '100px'; / box.style['backgroundColor'] = 'red';

### BOM操作

1. location

window.location.href = ”https://havenxie.github.io” ; 

2. window.onresize 改变窗口事件
3. window.onscroll 屏幕滚动事件