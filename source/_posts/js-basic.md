---
layout: post
title: "JavaScript核心-ECMAScript"
date: 2017-10-26
tags: [javascript]
comments: true
---

#### JavaScript的实现

    
    一个完整的JavaScript的实现由三部分组成：
    
    ECMAScript(核心) + DOM(文档对象模型) + BOM(浏览器对象模型)。

    注意：虽然JavaScript和ECMAScript经常表达的是一个意思，但是JavaScript包含ECMAScript。

##### ECMAScript

    ECMAScript与Web浏览器没有依赖关系，Web浏览器只是ECMAScript实现可能的宿主环境之一。
    
    其他宿主环境包括Node等。

##### DOM(文档对象模型)

    DOM是真对XML但经过扩展勇于HTML的应用编程接口(API)。
    
1. DOM 1级组成：

    (1) DOM核心：规定如何映射基于XML的文档结构，以便简化对文档中任意部分的访问和操作。

    (2) DOM HTML：在DOM核心的基础上加以扩展，添加了对HTML的对象和方法。

2. DOM 2级组成：

    (1) DOM视图：定义了跟踪不同文档视图的接口

    (2) DOM事件： 定义了事件和事件处理的接口

    (3) DOM样式：定义了基于css为元素应用样式的接口

    (4) DOM遍历和范围：定义了遍历和操作文档树的接口

3. DOM 3级扩展：

    (1) 引入了统一方式加载和保存文档的方法：在DOM加载和保存模块中定义

    (2) 新增了验证文档的方法：在DOM验证模块中定义

    (3) 对DOM核心进行了扩展

##### BOM(浏览器对象模型)

1. 弹出新浏览器窗口的功能

2. 移动、缩放和关闭浏览器窗口的功能

3. 提供浏览器详细信息的navigator对象

4. 提供浏览器所加载页面的详细信息的location对象

5. 提供用户显示器分辨率详细信息的screen对象

6. 对cookies的支持


#### 严格模式

严格模式是为JavaScript定义了一种不同的解析与执行模型。在代码顶部(或函数里面的顶部)中加入：“use strict”，这个字符串实际上是一个编译指示，用于告诉支持的JavaScript引擎切换到严格模式。

#### 数据类型
基本数据类型：undefined、null、boolean、number、string

复杂数据类型：object

使用typeof操作符可检测给定变量的数据类型，返回值为：undefined、boolean、string、number、object、function。

```javascript
typeof null;//object
//null是一个空对象指针，所以返回object

var message = "123112";

typeof message;//string

```
##### undefined
undefined类型只有一个值：undefined

变量使用var声明但是未复制，会出现undefined。

若变量没有声明就使用，则会报错，并非undefined。

##### null
unll类型只有一个值：null，null == undefined

##### boolean
boolean有两个值：true、false,需注意：区分大小写。

| 数据类型       |    转换成true     |  转换成false |
| --------   | -----:  | :----:  |
| Boolean     | true |   false     |
| String        |   非空字符串   |   空字符串   |
| Number        |    非零数字值    |  0 和 NAN  |
| Object        |    任何对象    |  null |
| undefined     |    不适用    |  undefined |

##### number

八进制：第一位必须是0，，后面是0-7，若超出范围，则以十进制解析。

十六进制：前两位必须是0x，后面是0-9和A-F，字母大小写不限制。

isFinite():判断一个数值是不是有穷的。

isNaN()：判断一个数是不是NaN。NAN与任何值都不相等，包括本身。

number():将任何数据类型转换成数值。

paeseInt()和parseFloat()：将字符串转换成数值。

```javascript
var num1 = Number("hello");//NaN
var num2 = Number("");//0
var num3 = Number("000011");//11
var num4 = Number(true);//1

var num1 = parseInt("1232hello");//1232
var num2 = parseInt("");//NaN
var num3 = parseInt("0xA");//10
var num4 = parseInt(22.5);//22
var num5 = parseInt("070",8);//56
var num6 = parseInt("70");//70

```

##### string

string类型由0或者多个16位Unicode字符组成的字符序列。由双引号或者单引号包含着，字符串一旦建立值就不可改变，除非销毁原来的值。

toString():将number、Boolean、object、string转换成字符串

默认情况下，toString()以十进制返回数值的字符串，参数可以为2、8、16表示二进制、八进制与十六进制。

String():将任何类型转成字符串。如果值是null返回null，值是undefined返回undefined。

```javascript
var num = 10;
num.toString();//"10"
num.toString(2);//"1010"
num.toString(8);//"12"
num.toString(16);//"a"

var val;
String(val);//"undefined"
String(10);//10
String(true);//"true"
String(null);//"null"

```

##### object

对象就是一组数据和功能的集合，可以通过执行new操作符来创建。object有属性和方法。

constructor：保存着用于当前对象的函数。

hasOwnProperty：检查给定属性是否存在于当前对象实例中。

isPrototypeOf：用于检查传入的对象是否是当前对象的原型。

propertyIsEnumerable：检查给定的属性是否能够for...in来循环遍历。

toLocalString：返回对象的字符串表示，与执行环境的地区相对应。

toString：返回对象的字符串表示。

valueOf：返回对象的字符串、数值或者布尔值表示。



#### 操作符

##### 一元操作符
1. 递增递减操作符

执行递增递减操作时，变量的值都是在语句被求值以前改变的(称为：副效应)。

```javascript
var myAge  = 23;
var hisAge = --myAge + 2;
console.log(myAge);//22
console.log(hisAge);//24

```

2. 一元加和减操作符

+放在数值前，对数值不产生影响。

-放在数值前，值变为负数。

对于不同的操作类型，会先转成数值类型，在进行操作
```javascript
var str1 = "23";
var str2 = "23.2";
var str3 = "abc";
var bool = false;
var num1 = 1.1;
var func = {
    valueOf:function(){
        return -1;
    }
}

console.log( +str1 );//23
console.log( +str2 );//23.2
console.log( +str3 );//NaN
console.log( +bool );//0
console.log( +num1 );//1.1
console.log( +func );//-1

console.log( -str1 );//-23
console.log( -str2 );//-23.2
console.log( -str3 );//NaN
console.log( -bool );//-0
console.log( -num1 );//-1.1
console.log( -func );//1

```
3. 位操作符

位操作符不直接操作64位的值，而是先转成32位进行操作，再将值转换为64位。

- ~ ：按位非(NOT)，结果返回数值的反码，与二进制有关，本质就是对原值取反-1。
```javascript
var num1 = 23;
var num2 = -23;
var str1 = "da";
console.log(~num1);//-24
console.log(~num2);//22
console.log(~str1);//-1

```
- &：按位与(AND)，当操作的两个数值都是1时，返回1，任何一位为0时，都返回0。

- |：按位或(OR)，当操作的两个数值任何一位是1时，返回1，都为0时，都返回0。

- ^：按位异或(XOR)：当操作的两个数值对应位上只有一个1时返回1，如果对应两位都是1或者0，则返回0。

```javascript
var num1 = 23 & 25;
var num2 = 23 | 25;
var num3 = 23 ^ 25;

console.log(num1);//17
console.log(num2);//31
console.log(num3);//14

//都是操作二进制的数值。

```
- <<：左移操作符

- \>\>：右移操作符

- \>>>：无符号右移

```javascript
var num = 23;
var num0 = -23;
var num1 = num << 5;
var num2 = num >> 5;
var num3 = num >>> 5;
var num4 = num0 >>> 5;

console.log(num1);//736
console.log(num2);//0
console.log(num3);//0
console.log(num4);//134217727

//又是操作二进制的数值。

```

##### 逻辑操作符
1. 逻辑非！

对任何数据类类型操作，先转换为布尔值，再对其求反。

2. 逻辑与&&

可以返回null、undefined、NaN、布尔值等。

3. 逻辑或||

##### 乘性操作符
1. \* 乘以

2. /除以

##### 加性操作符

1. +：若有字符串，则是连接作用。

2. -：若有字符串，先转换成数值，在进行减法操作。

##### 关系操作符

\>、<、= 、<= 、>=

##### 条件操作符(三目运算符)

 var max = (num1 > num2) ? num1 : num2;

#### 函数
函数使用function 关键词来声明，通过函数可以封装任意多条语句，而且可以在任何地方任何时候调用执行。

return：函数在执行完return语句之后退出，所以return之后的语句不会执行。

函数的参数：ECMAScript中的参数在内部是一个参数数组来表示的，函数接收到的永远都是这个数组，而不关心数组中有没有参数。实际上，在函数体内可以使用arguments对象来访问参数数组。

arguments：与数组类似，因为可以使用[]访问他的每一个元素，也可以用length来确定他有多少个参数，函数内部通过arguments可以改变函数参数（严格模式不可以）。


#### object类型

##### 2种创建方式

1. 使用new操作符后跟Object构造函数。

2. 对象字面量表示

##### 2种访问对象属性的方法

1. 点表示法(推荐使用)

2. []表示法(需用字符串形式)

- 使用[]的优点 

(1) 可以通过变量来访问属性

(2) 属性名中包含关键字或者保留字

(3) 属性中含有非字母非数字等导致语法错误的字符。


#### Array类型

##### 2种创建方式

1. 使用new操作符后跟Array构造函数(new 操作符可省略)。

2. 对象字面量表示


##### 检测数组

1. instanceOf 

2. Array.isArray()

##### 数组方法

 | 方法   |   参数  | 作用  | 返回值  |
 | :----:  | :----:  | :----:  | :----:  |
 | toLocalString() <br/>toString()<br/>valueOf() |    | 将数组转换成字符串   |   返回数组的字符串格式    |
 | push() <br/>pop() | 任意数量的参数<br/>无需传参    |  将参数添加到数组末尾<br/>移除数组的最后一项  |   返回新数组的长度<br/>被移除的那一项    |
| unshift() <br/>shift() | 任意数量的参数<br/>无需传参    |  将参数添加到数组最前端<br/>移除数组的第一项  |   返回新数组的长度<br/>被移除的那一项    |
| reverse() <br/>sort() | 无需传参<br/>比较函数    |  反转数组的顺序<br/>按照一定规则给数组排序  |   返回排序后的数组  |
| concat() <br/>slice() | 值或数组<br/>两或三个参数    |  基于当前数组的所有项创建一个新数组<br/>删除(起始位置,结束位置)、插入(起始位置,删除个数,插入项)、替换(起始位置,删除个数,插入项)  |   返回操作后的新数组  |
 | indexOf() <br/>lastIndexOf() | 查找的内容,查找开始的位置    |  从前往后找<br/>从后往前找 |  找不到返回-1，返回查找项在数组中的位置  |
| every() <br/>filter() <br/>forEach() <br/>map() <br/>some() | 给定函数(数组的每一项都执行该函数)  |  该函数对数组每一项返回true，则返回true<br/>返回值为true的数组<br/>运行给定函数<br/>函数调用后的结果组成的数组<br/>任一项返回true，则返回true |  true<br/>数组<br/>无返回值<br/>数组<br/>true |
 | reduce() <br/>reduceRight() | 给定函数(函数参数：prev,cur,index,arr)，归并基础的初始值  |  从头开始遍历<br/>从尾部开始遍历 |  迭代数组所有项，构建一个最终返回的值 |


#### Date类型

##### 创建方式 
使用new操作符加Date构造函数：

var now =  new Date()

##### Date方法
1. Date.parse()：参数为字符串格式的日期(格式因地区而异)，返回值为对应日期的毫秒数。如果参数不规范，则返回NaN。

2. Date.UTC()：参数格式全部为数字，其中年月必须，返回值为对应日期的毫秒数。

3. Date.now()：返回表示调用这个方法时的日期和事件的毫秒数。可使用+操作符来获取Date对象的时间戳。

4. toLocalString()：返回与浏览器设置的地区相适应的格式返回日期与时间。

5. toString()：返回带有时区信息的日期与时间。

6. valueOf()：返回日期的毫秒表示，可以用来比较日期大小。

##### 常用方法
1. getTime():获取日期毫秒数

2. getFullYear(): 获取四位数的年份

3. getMonth()：获取月份

4. getDate()：获取月份中的天数

5. getDay()：获取星期几(0：星期日，6：星期六)

6. getHours()：获取小时(0-23)

7. getMinutes()：获取分钟(0-59)

8. getSeconds()：获取秒数(0-59)

- 注意：将get改为set，则可以设置对应的时间。


#### RegExp类型

正则暂时不研究了，遇见就百度吧。


#### function类型

函数是对象，函数名是只想函数对象的指针。

##### 创建方式

1. 使用函数声明语法：function Sum (){   }。可提前调用。

2. 使用函数表达式定义函数：var sum = function(){    }。不可提前调用。

3. 使用Function构造函数：var sum = new Function()。

##### 函数内部属性

1. arguments：类数组对象，包含传入函数的所有参数。

- arguments有一个属性： callee，callee是 一个指向拥有arguments对象函数的指针，在函数内部中使用本函数时，可以使用arguments.callee()来代替。

2. caller：函数对象的属性，保存着调用当前函数的函数的引用。A函数中调用B函数，则A.caller指向B函数。

3. length：函数的length表示函数希望接受的命名参数的个数。

4. prototype：保存所有的实例方法，且不可枚举，即不能使用for...in来遍历。

##### 函数方法

1. apply()：在特定的作用域中调用函数，实际上等于设置函数体内this对象的值。参数有两个：(函数的作用域,数组)，第二个参数也可以是arguments。

2. call()：与apply作用想同，改变this指向。有多个参数：(this,参数1，参数2...)。

- apply()与call()真正作用：扩大函数赖以运行的作用域，好处就是对象不需要与方法有任何耦合关系。

3. bind()：创建一个函数的实例。


#### String类型

##### 字符串方法

 | 方法   |   参数  | 作用  | 返回值  |
  | :------:  | :----:  | :----:  | :----:  |
| charAt() <br/>charCodeAt() |  字符位置  | 用于访问字符串中特定的字符  |   返回给定位置的字符<br/> 返回给定位置的字符的字符编码     |
  | concat() | 字符串 |  拼接任意个数的字符串 |   返回新拼接的字符串    |
   | slice()<br/>substr()<br/>substring() | (开始位置,结束位置)<br/>(开始位置，字符个数)<br/>(开始位置,结束位置) |  基于子字符串创建新的字符串 |   返回新的字符串 且不改变原字符串   |
   | indexOf()<br/>lastIndexOf() | (给定子字符串，开始查找位置) |  查找子字符串 |   返回-1或者子字符串第一次出现的位置    |
  | trim() | 字符串 |  删除字符串前置以及后缀的所有空格 |   返回字符串副本 |
  | toLowerCase()<br/>toLocalLowerCase()<br/>toUpperCase()<br/>toLocalUpperCase() | 无 |  实现字符串大小写转换 |   返回转换后的字符串 |
  | match()<br/>search()<br/>replace()<br/>split() | 正则<br/>正则<br/>(正则或字符串,字符串或函数)<br/>(分隔符,数组大小) |  字符串中匹配模式的方法 |   返回数组<br/>返回字符串中第一个匹配项的索引<br/>返回新的字符串<br/>返回数组   |
   | localCompare() | 需比较的字符串之一 |  按字母表顺序比较两个字符串 |  返回值-1、0、1   |
  | fromCharCode() | 字符编码 |  字符编码 |  返回编码对应的字符串   |
