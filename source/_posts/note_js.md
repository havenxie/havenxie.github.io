---
title: JS笔记
date: 2017-01-02 21:41:39
tags:
---

## [JS部分]
js数据类型分为：Number、String、Boolean、Null、Undefined

内置对象：
数据封装类对象：Object、Array、Boolean、Number、String
其他对象：Function、Arguments、Math、Date、RegExp、Error

<!--more-->
### 布尔型
数值类型转换成布尔型

1. 利用!!
    console.log(typeof !!num);
2. 利用Boolean()
    console.log(Boolean(321));//log出 true

### 数值型
1. 数值前面带0表示八进制 var num = 020; `console.log(020)`=16
2. 数值前面带0x表示十六进制 var num = 0x16; `console.log(0x16)`=16
3. 转换为数值型： 
    - 利用 - * / 都可以: 
        + `console.log(021 - 0)`= 17  //二进制转成十进制
        + `console.log('021' - 0)`= 21//二进制字符串先转成十进制数值型再做算术运算
        + `console.log('21' - 0)`= 21 //十进制字符串直接转成数值型进行算术运算
        + `console.log(021 * 1)` = 17
        + `console.log('021' * 1)`= 21
        + `console.log('21' * 1)`= 17
        + `console.log(021 / 1)`= 17
        + `console.log('021' / 1)`= 21
        + `console.log('21' / 1)`= 21

    - 利用Number()函数。 
        + console.log(Number('123t'))这样会报错NaN，因为含有t
        + console.log(Number(021))//二进制转成十进制17
        + console.log(Number(0x16))//十六进制转成十进制22
        + console.log(Number('021'))//不会将二进制转成十进制，直接将021当做21，结果为21
        + console.log(Number('0x16'))//可以将字符串形式的十六进制转成十进制

    - parseInt(值,进制); paeseInt(110,2);//把110这个2进制转换成十进制
    - parseFloat(); 

### Null undefined

#### null和undefined的区别：
> null是一个表示“无”的对象，转换为数值为0；undefined是一个表示“无”的原始值，转换为数值是NaN

unll：

- 作为函数的参数，表示该函数的参数不是对象
- 作为对象原型链的终点

undefined：

- 变量被声明了，但是没有赋值就是undefined
- 函数调用时，应该提供的参数没有提供，该参数经济等于undefined
- 对象没有赋值的属性，该属性的值就是undefined
- 函数没有返回值时，默认返回的就是undefined

### arguments参数(注意是arguments而不是arguements)
1. arguments是存储了函数传送过来的实参(传给他什么他就有什么)
2. Javascript在创建函数的同时，会在函数内部创建一个arguments对象实例.
3. arguments对象只有函数开始时才可用。他只在正在使用的函数内使用。
4. 函数的 arguments 对象并不是一个数组而是一个对象，访问单个参数的方式与访问数组元素的方式相同
5. arguments对象的长度是由实参个数而不是形参个数决定的
```
function fun(a, b, c) {
    console.log(fun.length);//得到的是函数的形参的个数，不论传来多少参数，这里都会固定log出三个
    console.log(arguments.length);//这里拿到的是实参的个数，和形参没有关系
}
```
6. arguments.callee 返回的是正在执行的函数。也是在函数体内使用。在使用函数递归调用时建议使用arguments.callee代替函数名本身。
```
function fun() { console.log( arguments.callee ); }
fun();
// 这个callee就是 function fun() { console.log( arguments.callee ); }
```
### 数组
1. 添加数组元素
    - push()从数组的后面添加进去 eg: var arr = [1,3,5]; arr.push(7); 返回的是添加之后数组的长度。
    - unshift()从数组的前面添加进去 eg: var arr = [1,3,5]; arr.unshift(0); 返回的是添加之后数组的长度。
2. 删除数组元素
    - pop()从数组的后面删除元素 eg: var arr = [1,3,5]; arr.pop(); 返回的是被删除的那个元素
    - shift()从数组的前面删除元素 eg: var arr = [1,3,5]; arr.shift();返回的是删除的那个元素。
3. 查找数组中的一个元素
    - indexOf() eg: var arr = [1,3,5]; arr.indexOf(3);查找数组中是否存在某个值，成功查找返回索引，否则返回-1
4. 数组遍历方法
    - forEach()
    ```
    var arr = [1, 3, 5];
    arr.forEach(function(item, index) {
        console.log(item);
        console.log(index);
    })
    ```
5. 连接两个数组concat()
    该方法用于连接两个或多个数组。它不会改变现有的数组，而仅仅会返回被连接数组的一个副本。
    ```
    var arr1 = [1,3,5]; var arr2 = ['a', 'b', 'c'];
    var newArr = arr1.concat(arr2);
    console.log(newArr);
    // 结果：  [1,3,5,“a”,”b”,”c”];
    ```

6. join() 把数组转换为字符串
    作用是将数组各个元素是通过指定的分隔符进行连接成为一个字符串。
    ```
    var arr = [1, 3, 5, 'a', 'b', 'c'];
    var newStr1 = arr.join();//join(符号) 要指定用什么符号分割，如果不指定则默认是用“逗号”
    var newStr2 = arr.join('-');
    console.log(newStr1);//得到 "1,3,5,a,b,c"
    console.log(newStr2);//得到 "1-3-5-a-b-c"
    ```

7. slice()方法对数组进行部分截取，并返回一个数组副本；参数start是开始截取的数组元素的索引，end是终止截取的数组元素的索引（不包含终止的这一个）（可选）
    ```
    //如果不传入参数二，那么将从参数一的索引位置开始截取，一直到数组尾
    var a=[1,2,3,4,5,6];
    var b=a.slice(0,3);  //[1,2,3]
    var c=a.slice(3);    //[4,5,6]

    //如果两个参数中的任何一个是负数，array.length会和它们相加，试图让它们成为非负数，举例说明：
    //当只传入一个参数，且是负数时，length会与参数相加，然后再截取
    var a=[1,2,3,4,5,6];
    var b=a.slice(-1);  //[6]

    //当只传入一个参数，是负数时,并且参数的绝对值大于数组length时，会截取整个数组
    var a=[1,2,3,4,5,6];
    var b=a.slice(-6);  //[1,2,3,4,5,6]
    var c=a.slice(-8);  //[1,2,3,4,5,6]

    //当传入两个参数一正一负时，length也会先于负数相加后，再截取
    var a=[1,2,3,4,5,6];
    var b=a.slice(2,-3);  //[3]

    //当传入一个参数，大于length时，将返回一个空数组
    var a=[1,2,3,4,5,6];
    var b=a.slice(6);　　//[]
    ```

8. splice()方法用于插入、删除或替换数组的元素。这是个超强的方法。用法：
    splice 的参数 ：splice (start, deleteCount, [item1[, item2[, . . . [,itemN]]]])
    + 数组从 start下标开始，删除deleteCount 个元素，并且可以在这个位置开始添加 n个元素
    + 当deleteCount 为0 的时候，就是在数组的start元素前面插入新的元素，并返回一个空数组。
    + 当参数只有 start，deleteCount 就是从start 下标开始删除deleteCount 个数组的元素，返回被删除的数组。
    + 当参数只有start参数时，就是删除 从start下标起至最后的元素。
    + 当参数为负的时 则该参数规定的是从数组元素的尾部开始算起的位置 （-1 指的是 数组中倒数第一个元素， -2 指的是，数组中倒数第二个元素。）

9. 保留小数位
    + console.log(str.substr(0,str.indexOf(".")+3));//122340.12345会得到122340.12
    + console.log(parseInt(Math.PI*100) /100);//先乘以100取整，然后再除以100
    + console.log(Math.PI.toFixed(2));//直接用toFixed(位数)来操作。

### 字符串

1. 字符串遇到+，+就变成了拼接。
    ```
    + “”       2+ “”  =  “2”    2+”ab”   =  “2ab”
    var num = 123;
    console.log(num + "");//得到"123"
    ```

2. String(num);转为字符串 eg: var num = 123; console.log(String(num));

3. toString(基数); 基数就是进制eg: var txt = 10; txt.toString(2);//得到1010

4. charAt(index);获取指定位置的字符 eg: var txt = "abcdef"; txt.charAt(2);//得到c

5. charCodeAt(index);获取指定位置字符unicode编码。eg: var txt = "abc123"; console.log(txt.charCodeAt(3));//得到1的unicode编码是41。

6. indexOf('xxx');查找指定字符在字符串中的索引。只找第一个，找不到就返回-1。数组中也有这个方法，他们的使用方法一样。eg: var txt = "abc123"; console.log(txt.indexOf('c'));//返回2

7. lastIndexOf('xxx');从后往前查找指定字符在字符串中的索引。只找一个，找不到返回-1。数组中也有这个方法，他们的使用方法一样。eg: var txt = 'abc123'; console.log(txt.lastIndexOf('1'));//返回3

8. concat();字符串连接。数组中也有这个方法，他们的使用方法一样。 eg: var txt1 = '123'; var txt2 = 'abc'; console.log(txt1.concat(txt2));

9. slice();方法可提取字符串的某个部分，并以新的字符串返回被提取的部分。用法和参数均与数组相同。
    ```
    var a = 'i an a boy';
    var b = a.slice(0, 6);//得到'i an a'
    ```
    
10. split();分割 将字符串转换成数组
      stringObject.split(separator,howmany) 方法用于把一个字符串分割成字符串数组。参数 separator 可选。指定要使用的分隔符。如果省略该参数，则使用逗号作为分隔符。howmany 可选。该参数可指定返回的数组的最大长度。
     ```
     var str = "aa-bb-cc";
     console.log(str.split('-'));
     ```

11. substr();截取字符串
    substr(起始位置，[截取个数])不写取得个数，默认从开始位置一直取到最后。
    ```
    var txt = 'abcdefghijk';
    txt.substr(3, 4);//得到'defg'
    ```

12. substring();同slice()方法一样，但是substring()始终会把小的值放在起始位置大的值放在结束位置。例如substring(6,3);会自动转成 substring(3,6);
    ```
    var txt = 'abcdefghijk';
    var newStr = txt.substring(txt.substring(6,3));//等效txt.substring(6,3)
    console.log(newStr);//def
    ```

13. 大小写转换
    + 'abc'.toUpperCase();//转换为大写
        ```javascript
        function toUpperFirstLetter(str) {//将字符串的首字母转换成大写
            if(str && (String === typeof str)) {
                return str.slice(0, 1).toUpperCase() + str.slice(1);
            } else {
                return false;
            }
        }
        ```
    + 'ABC'.toLowerCase();//转换为小写

14. 网址编码与解码
    var url = 'https:havenxie.github.io';
    + 编码：encodeURIComponent(url)//把字符串作为URl组件进行编码
    + 解码：decodeURlComponent(url)//把字符串作为URI组件进行解码

15. xxx.toString().trim()//转换成字符串并去掉后面的空格

### 日期函数 Date() 

这个函数 （对象） 可以设置我们本地日期。年 月 日 时 分 秒 
var date = new Date();//dete = Mon Jan 02 2017 15:14:50 GMT+0800 (中国标准时间）
通过date.getTime()、+ newDate()或者date.valueOf();得到从1970年1月1日到现在的毫秒数。
//+ new Date();//将标准时间转换成Nubmer格式，通过“+”调用Date()内部的ToNumer()方法。
常用的日期函数：

- getDate();    //获取日1-31
- getDay();     //获取星期0-6
- getMonth();   //获取月份0-11
- getFullYear();//获取完整年份
- getHours();   //获取小时0-23
- getMinutes(); //获取分钟0-59
- getSeconds(); //获取秒0-59
- getMilliseconds();//获取毫秒
- getTimes();   //返回累计毫秒数(从1970/1/1午夜)

自己定义时间：

var myTime = new Date('2015/12/12 15:40:00');//myTime=Sat Doc 12 2015 15:40:00 GMT+0800 (中国标准时间)。

- 如果date 括号里面写日期  就是 自己定义的时间 。
- 如果 date括号里面不写日期 ， 就是当前时间 。

### Math函数 

- Math.pow(m, n);//乘方，表示m的n次方
- Math.sqrt(81); //开方，就是开根号运算
- Math.ceil();   //向上取整
- Math.floor();  //向下取整
- Math.round();  //四舍五入

### in运算符

- in运算符也是一个二元运算符，但是对运算符左右两个操作数的要求比较严格。
- in运算符要求第1个（左边的）操作数必须是字符串类型或可以转换为字符串类型的其他类型
- 而第2个（右边的）操作数必须是数组或对象。
- 只有第1个操作数的值是第2个操作数的属性名(注意不是值)，才会返回true，否则返回false

```
//in经常用来判断json中有没有某个属性
var json = {name: 'haven', age: 25};
if('haven' in json) {
    console.log('yes');
} else {
    console.log('no');
}
if('name' in json) {
    console.log('yes');
} else {
    console.log('no');
}
// 将会log出 no yes
```

### this对象

> this总是指向事件的直接调用者(而非间接调用者)
> 如果有new关键字，this总是指new出来的那个对象
> 在事件中this指向触发这个事件的对象，特殊的是，IE中的attachEvent中的this总是指向全局对象window

### new操作符
new操作符具体干的事：
- 创建一个空对象，并且this引用这个空对象，同时继承了该函数的原型
- 属性和方法被加入到了this引用的对象中去
- 新创建的对象由this引用，并且隐式的返回这个this 

### eval

> 它的功能是把对应的字符串转换成js代码并进行执行
> 应该避免使用eval，不安全并且消耗性能（一次解析成js代码一次执行）
> 由JSON字符串转换成JSON对象的时候可以使用eval。`var obj = eval('('+ str +')')`;

### JSON

> JSON是一种采用键值对方式的轻量级的数据交换格式

#### JSON转换方法
- JSON.parse(xxx)//将json字符串装换成JSON对象
- JSON.stringify(xxx)//将json对象转换成JSON字符串

### apply()和call()

- apply()函数有两个参数，第一个参数是上下文，如果上下文是null，则使用全局对象代替；第二个参数是参数组成的数组。
```javascript
function.apply(this, [1, 2, 3]);
```

- call()函数第一个参数是上下文，后续的是传入的参数序列
 ```javascript
function.call(this, 1, 2, 3);
 ```

#### for in 遍历JSON
for in关键字
for(变量 in 对象) { 执行代码 }
```javascript
var json = {width: 200, height: 300, left: 50};
for(var key in json) {
    console.log(key);//key是遍历json的变量，可以得到json的“键”
    console.log(json[key]);//json[key]得到的是key对应的属性值。
}
```

### 定时器

- setInterval(fun, 5000);
    setInterval是排队执行的
    假如 间隔时间是1秒， 而执行的程序的时间是2秒    上次还没执行完的代码会排队, 上一次执行完下一次的就立即执行, 这样实际执行的间隔时间为2秒

- setTimeout(fun, 5000);
    setTimeout延迟时间为1秒执行, 要执行的代码需要2秒来执行,那这段代码上一次与下一次的执行时间为3秒。


### UA(浏览器标识)

> 浏览器标识(UA)可以使得服务器能够识别客户使用的操作系统及版本、CPU类型、浏览器版本、浏览器渲染引擎、浏览器语言、浏览器插件、从而判断用户是使用电脑浏览器还是手机浏览器，让页面做出自动的适应。

```javascript
function whatBrowser() {
    document.Browser.Name.value = navigator.appName;
    document.Browser.Version.value = navigator.appVersion;
    document.Browser.Code.value = navigator.appCodeName;
    document.Browser.Agent.value = navigator.userAgent;
}
```


### 闭包
我们可以用一个函数去访问另外一个函数的内部变量的方式就是闭包。
优点：不产生全局变量，实现属性私有化。
缺点：闭包中的数据会常驻内存，在不用的时候要删掉否则会导致内存溢出。

闭包写法：
```javascript
function outFun() {//闭包写法
    var num = 0;
    function innerFun() {
        num ++;
        console.log(num);
    }
    return innerFun;
}

var demo = outFun();//闭包使用
demo();//输出1
demo();//输出2
demo();//输出3
var demo2 = outFun();
demo2();//输出1
demo2();//输出2
```

闭包传递参数：
```javascript
function fun(x) {
    return function(y) {
        console.log(x + y);
        return x + y;
    }
}
var demo = fun(5);
demo(2);//输出7
demo(6);//输出11
```

使用案例1：
```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style>
        div {
            position: absolute;
            left: 0;
            background-color: pink;
            width: 100px;
            height: 100px;
        }
    </style>
</head>
<body>
    <button id="btn1">右走</button>
    <button id="btn2">左走</button>
    <div id="box"></div>
</body>
</html>
<script>
    var btn1 = document.getElementById("btn1");
    var btn2 = document.getElementById("btn2");
    var box = document.getElementById("box");
    function move(speed) {
        return function() {
            box.style.left = box.offsetLeft + speed + "px";
        }
    }
    btn1.onclick = move(5);
    btn2.onclick = move(-5);
</script>
```

使用案例2：
```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style>
        *{margin:0;padding:0;}
        ul{list-style:none;}
        .box {
            width: 350px;
            height: 300px;
            border:1px solid #ccc;
            margin: 100px auto;
            overflow: hidden;
        }
        .mt span {
            display: inline-block;
            width: 80px;
            height: 30px;
            background-color: pink;
            text-align: center;
            line-height: 30px;
            cursor: pointer;
        }
        .mt span.current {
            background-color: purple;
        }
        .mb li {
            width: 100%;
            height: 270px;
            background-color: purple;
            display: none;
        }
        .mb li.show {
            display: block;
        }
    </style>
    <script>
        window.onload = function(){
            function tab(obj){
                var target = document.getElementById(obj);
                var spans = target.getElementsByTagName("span");
                var lis = target.getElementsByTagName("li");
                for(var i=0;i<spans.length;i++)
                {
                    spans[i].onmouseover =  function (num) {
                        return function(){
                            for(var j=0; j<spans.length;j++)
                            {
                                spans[j].className = "";
                                lis[j].className = "";
                            }
                            spans[num].className = "current";
                            lis[num].className = "show";
                        }
                    }(i);
                }
            }
            tab("one");
            tab("two");
            tab("three");
         }
    </script>
</head>
<body>
<div class="box" id="one">
    <div class="mt">
        <span class="current">新闻</span>
        <span>体育</span>
        <span>娱乐</span>
        <span>八卦</span>
    </div>
    <div class="mb">
        <ul>
            <li class="show">新闻模块</li>
            <li>体育模块</li>
            <li>娱乐模块</li>
            <li>八卦模块</li>
        </ul>
    </div>
</div>
<div class="box" id="two">
    <div class="mt">
        <span class="current">新闻</span>
        <span>体育</span>
        <span>娱乐</span>
        <span>八卦</span>
    </div>
    <div class="mb">
        <ul>
            <li class="show">新闻模块</li>
            <li>体育模块</li>
            <li>娱乐模块</li>
            <li>八卦模块</li>
        </ul>
    </div>
</div>
<div class="box" id="three">
    <div class="mt">
        <span class="current">新闻</span>
        <span>体育</span>
        <span>娱乐</span>
        <span>八卦</span>
    </div>
    <div class="mb">
        <ul>
            <li class="show">新闻模块</li>
            <li>体育模块</li>
            <li>娱乐模块</li>
            <li>八卦模块</li>
        </ul>
    </div>
</div>
</body>
</html>
```

-------------------------------------------------------------------------------
console.log('xxx');     //向控制台输出log日志
console.warn('xxx');    //向控制台输出xxx警告提示
console.error('xxx');     //向控制台输出xxx错误提示
console.time('id1');
console.timeEnd('id1'); //控制台输出从time('id1')执行到timeEnd('id1')经历的时间
-------------------------------------------------------------------------------
var res = parseFloat('3.1415');//将字符串转换成浮点数
var res = prompt("请输入一个数字");//弹窗用来接收用户输入的字符，返回的是字符型
-------------------------------------------------------------------------------



