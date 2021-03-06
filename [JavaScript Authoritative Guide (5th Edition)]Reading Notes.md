# 《Javascript权威指南第5版上》读书笔记1

[TOC]

## 第1章 概述
Javascript是一种具有面向对象能力的，解释型的程序设计语言。
Javascript是一种松散类型语言，它的编写不必具有一个明确的类型，Javascript中的对象吧属性名映射为任意的属性值。
Javascript中的OO继承机制是基于原型的
Javascript的核心语言将数字、字符串和布尔值作为原始数据类型支持，它还内建支持数组、日期和正则表达式对象。
主流Web浏览器中都支持W3C DOM标准的核心部分，Microsoft Internet Explorer不支持W3C标准事件处理。
```html
<form name="myForm">
        <input type="text" name="user" placeholder="userName"><br>
        <input type="password" name="pwd" placeholder="password"><br>
        <input type="button" onclick="doLogin()" value="Login">
    </form>
    <script>
        function doLogin() {
            var user = document.myForm.user.value;
            var pwd = document.myForm.pwd.value;
            alert(user+','+ pwd);
        }
    </script>
```
## 第一部分 核心Javascript
### 第2章 词法结构
Javascript 是Unicode字符集编写的，大小写敏感，忽略程序中记号之间的空格、制表符和换行符，但对换行符有一些限制。

```js
//注释
/*注释*/

//直接量：
11212   1.2323  //number
"test"  'test'  //string
true  false    //boolean
/javascript/g   //正则表达式对象
null  undefined //空对象
[1,2,3,4,5]    //array
{a:1,b:2,c:'hello'}  //object

//标识符命名：不可与保留字同名
aaa
my_aaa_bbb
aaa123
_aaBB
$aa

//Javascript保留字：
break  case  catch  continue  default  delete
do  else  false  finally  for  function  
if  in  instanceof  new  null  null  return 
switch  this  throw  true  try  
typeof  var  void  while  with

//ECMA扩展保留字
abstract  boolean  byte  char  class  const  debugger
double  enum  export  extends  final  float  
goto  inplements  import  int  interface  long  
native  package  private  protected   public  short
static  super  synchronized   throws  transient  volatile
as  is  namespace  use

//避免使用标识符
arguments  Array  Boolean  Date  decodeURI  decodeURIComponent
encodeURI  Error  escape  eval  EvalError  Function  Infinity
isFinite  isNaN  Math  NaN  Number  Object  parseFloat  parseInt
RangeError  ReferenceError  RegExp  String  SyntaxError  TypeError
undefined  unescape  URIError

```

### 第3章 数据类型和值
**基本数据类型：**
数字、文本字符串、布尔值、null（空）、undefined（未定义）

**复合数据类型**
对象（已命名的有序集合：object，有编号值的有序集合：数组array）、函数function、类class（自定义类、非自定义类Date，RegExp，Error等）

**数字**
JavaScript中所有数字都是由浮点型表示，采用IEEE 754标准定义的64为浮点格式，最大值+-（1.7976931348623157*10^308），最小值+-（5*10^(-324)）
Javascript数字整数范围精确度(-2^53)~(2^53)，超过范围会失去尾数精确性。某些运算中范围是32为整数（-2^31）~(2^31)

十六进制直接量：以0x或0X开头，后跟十六进制数字符串直接量（0-f）
0xff  //15*16+15=255
0xCAFE911

八进制直接量：以0开头，后跟数字序列（0-7），ECMAScript不支持八进制直接量，Javascript支持
0377 //3*64+7*8+7=255

浮点型直接量：
3.14
123.45464645
.333333333
浮点型指数记数法：`[digits][.digits][(E|e)[(+|-)]digits]`
6.02e23   //6.02*10^23
1.4738223E-32   //1.4738223*10^(-32)


算数预算函数保存为Math对象属性，可直接调用
Math.sin(x)
Math.sqrt(x*x+y*y)

特殊数值
isNaN()检测是否非数字特殊值
isFinite()检测是否NaN,正无穷大或负无穷大
Infinity无穷大
NaN非数字值
Number.MAX_VALUE 最大数字
Number.MIN_VALUE 最小数字
Number.NaN非数字值
Number.POSITIVE_INFINITY 正无穷大
Number.NEGATIVE_INFINITY 负无穷大

**字符串**
```js
"You\'re right,it can\'t be a quote"   //You're right,it can't be a quote

//Javascript转义序列
\0   //NUL字符(\u0000)
\b   //退格符(\u0008)
\t   //水平制表(\u0009)
\n   //换行符(\u000A)
\v   //垂直制表符(\u000B)
\f   //换页符(\u000C)
\r   //回车符(\u000D)
\"   //双引号符(\u0022)
\'   //单引号符(\u0027)
\\   //反斜线符(\u005C)
\xXX   //由两位十六进制数值XX指定的Latin-1字符
\xXXXX   //由四位十六进制数值XXXX指定的Unicode字符
\XXX   //由一位到三位8禁止书（1到377）指定的Latin-1字符（ECMAScript v3不支持）

//字符串使用
var str1="Hello";
var str2="World"
var str3=str1+' '+str2;//Hello World
var last_str=str3.charAt(str3.length-1);//d
var ss=str3.substring(0,3);//Hell
var position_r=str3.indexOf("r");//8

//数字转字符串
111+''  //"111"
String(111)   //"111"
(111).toString()  //"111"

var val=17;
var valTo2=val.toString(2);//"10001" 二进制
var valTo8=val.toString(8);//"21" 八进制
var valTo16=val.toString(16);//"11" 十六进制

toFixed()//把一个数字转换为字符串，并且显示小数点后的指定的位数，不使用指数表示法
toExponential()//使用指数表示法把一个数字转换为字符串，小数点前一位数，小数点后有指定的位数
toPrecision()//使用指定的有意义的位数来显示一个数字，如果有意义的位数还不够显示数字的整个整数部分，他会使用指数表示法
//以上方法都会对结果字符串进行四舍五入

var n=123456.789;
n.toFixed(0);//"123456"
n.toFixed(2);//"123456.78"
n.toExponential(1);//"1.2e+5"
n.toExponential(3);//"1.235e+5"
n.toPrecision(4);//"1.235e+5"
n.toPrecision(7);//"123456.8"

//把字符串转换为数字
var nn="21"*"2";//42
var ss="33"-0;//33
var n=Number("99");//99
parseInt("3 meters");//3
parseFloat("3.14 meters");//3.14
parseInt("12.43");//12
parseInt("0xFF");//255
parseInt() parseFloat()//从字符串开始处转换和返回任何的数字，忽略或舍去非数字部分
parseInt()//只截取整数部分 可将0x或0X开头的字符串解释成一个十六进制的数字
parseFloat()//截取证书和浮点数
parseInt(str,[base])//base 2~36解析数字的基数
parseInt("11",2)//3 (1*2+1)
parseInt("ff",16)//255 (15*16+15)
parseInt("zz",36);//1295 （35*36+35）
parseInt("077",8);//63 (7*8+7)
parseInt("099",10);//99
parseInt()和parseFloat()不能把指定的字符串转为数字，会返回NaN
parseInt("eleven");//NaN
parseFloat("$12.33");//NaN
```

**布尔值**

true/false

```js
//布尔类型转换
数值环境下，true=>1  false=>0
字符串环境下，true=>"true" false=>"false"
0或NaN,"",null,undefined=>false

//转换为Boolean
var xToBoolean=Boolean(x)
var xToBoolean=!!x
```

**函数**

```js
function myFun(x){
    return x*x;
}
var a=myFun(10);//100

//函数直接量来定义函数
var myFun=function(x){
     return x*x;
}

//类来定义函数
var myFun=new Function("x","return x*x;");

```

**对象**

已命名的数据集合

```js
document.myForm.button
document.write("Hello world");
var image={
    height:100,
    width:200
}
image.height//100
image.width//200
image["height"]//100
image["width"]//200

//创建对象
//调用特殊的构造函数
var obj=new Object();
var date=new Date();
var pattern=new RegExp("\\sjava\\s","i");
date.getFullYear()//2019
obj.x=123;
obj.y=456;

//对象直接量
var point={x:123,y:234}
var rect={
    upperLeft:{x:2,y:4}
    lowerRight:{x:4,y:8}
};
var rect={
    "upperLeft":{x:2,y:4}
    'lowerRight':{x:4,y:8}
};

//对象转换
非空对象=>true
obj.toString()//对象转字符串
date.valueOf()//date.getTime()
```

**数组**

```js
//创建数组
var myArray=new Array();
a[0]=12.3;
a[1]="javascript";
a[2]=true;
a[3]={x:123,y:234};

var a=new Array(12.3,"javascript",true,{x:123,y:234});

var aa=new Array(10);//创建具有是个未定义元素的新数组

//数组直接量
var a=[12.3,"javascript",true,{x:123,y:234}];

var matrix=[[1,2,3],[4,5,6],[7,8,9]];

var base=1024;
var table[base,base+1,base+2,base+3];

var leftArray=[1,,,,5];//有3个未定义元素
```

**null**

无值，对象类型的特殊值，无对象的值

```js
Boolean:null=>false

Number:null=>0

String:null=>"null"
```

**undefined**

使用一个未声明的变量，或已声明但未赋值变量，并不存在的对象属性=>undefined

```js
undefined==null  //true

undefined===null  //false

typeof(undefined) //"undefined"

typeof(null) //"object"

Boolean:undefined=>false

Number:undefined=>NaN

String:undefined=>"undefined"
```

和null不同，undefined不是JavaScript的保留字，ECMAScript v3标准规定了名为undefined的全职变量，他的初始值为undefined，故在JavaScript中把undefined作为关键词处理。

声明变量但不初始化（不赋值），值为undefined

void运算符提供了另一种获取undefined值的方法

**Date对象**

日期和时间的对象类

```js
var now=new Date();//当前时间日期
var xmas=new Date(2019,5,10);//2019-06-10

xmas.setFullYear(xmas.getFullYear()+1);//2020
xmas.getDay();//1 周一
xmas.toLocaleString();//"2019/6/10 上午12:00:00"
```

**正则表达式**

Javascript采用了Perl程序设计语言的语法表示正则表达式

```js
new RegExp(/^HTML/);
new RegExp(/[1-9][0-9]*/);
new RegExp(/\bjavascript\b/i);
```

**Error对象**

错误类，运行错误是会抛出某个类的对象，每个Error具有一个message属性，存放Javascript实现特定的错误消息。

预定义的错误对象类型有：

Error、EvalError、RangeError、ReferenceError\SyntaxError\TYpeError和URIError



**类型转换小结**（自动数据类型转换）

| 值           | 字符串         | 数字                       | 布尔  | 对象        |
| ------------ | -------------- | -------------------------- | ----- | ----------- |
| 未定义值     | “undefined”    | NaN                        | false | Error       |
| null         | "null"         | 0                          | false | Error       |
| 非空字符串   | 不变           | 字符串的数字值或NaN        | true  | String对象  |
| 空字符串     | 不变           | 0                          | false | String对象  |
| 0            | “0”            | 不变                       | false | Number对象  |
| NaN          | “NaN”          | 不变                       | false | Number对象  |
| 无穷         | “Infinity”     | 不变                       | true  | Number对象  |
| 负无穷       | “-Infinity”    | 不变                       | true  | Number对象  |
| 任意其他数字 | 数字的字符串值 | 不变                       | true  | Number对象  |
| true         | “true”         | 1                          | 不变  | Boolean对象 |
| false        | "false"        | 0                          | 不变  | Boolean对象 |
| 对象         | toString()     | valueOf(),toString(),或NaN | true  | 不变        |



**基本数据类型的包装对象**

为什么字符串的操作采用对象的表示法？

三种基本数据类型：数字，字符串，布尔值

Number,String,Boolean类是基本数据类型的包装，他们具有和基本类型一样的值，定义了用来运算数据的属性和方法。

Javascript可灵活地将一种类型的值转换为另一种类型，瞬态对象的转换。

当在对象环境下使用字符串（试图访问字符串的属性和方法）时，JavaScript会为这个字符串值内部地创建一个String包装对象，String对象就代替了原始字符串值，因对象具有属性和方法，故能使用简单的值。其他基本数据类型同理。注意：被创建的String对象是瞬间存在的，使得我们可以访问属性和方法，此后就没用了，故系统会将它丢弃，原始的字符串并不会改变。

显示使用String对象，非瞬态的对象

```js
var s=new String("Hello World");
typeof(new String("Hello World"));//"object"
typeof("Hello World");//"string"
var msg=s+"!";//Javascript自动将String对象转换为一个字符串，瞬态的基本字符串值被创建。
var num=Object(3);//使用Object()函数，任何数字、字符串或布尔值都可以转换为它对应的包装对象。
```

**对象到基本类型的转换**

```js
//以下对象在布尔环境下，都转换为true，因为它们是非空对象
new Boolean(false)
new Number(0)
new String("")
new Array()
```

自动数据类型转换是通过先调用对象的valueOf()方法来转换为数字对象，而默认的valueOf()不会返回一个基本类型的值，Javascript会调用对象的toString()方法将对象转换为一个数字：首先调用toString()，并且把结果字符串转换为一个数字。

数组的toString()把数组元素转换为字符串，然后返回这些字符连接的结果，包括字符串之间的逗号，故一个没有元素的数组会转换为一个空字符串，从而转换为数字0。如果数组只有一个元素并且是数字n，则数组转换为一个表示n的字符串，最后转换为数字n自身。如果数组包含多个元素，或其中一个元素不是数字，数组会转换为NaN

在+运算和比较运算（>,>=和<,<=）中，大多数情况下，Javascript试图通过调用对象的valueOf()发方法来转换它，如果该方法返回一个基本类型（通常是一个数字），就使用这个值。然而，通常valueOf()方法只是返回一个为转换的对象，在这种情况下，JavaScript会尝试调用对象的toString()方法来把对象转换为一个字符串。

例外于此规则：当Date对象与+运算一起使用，转换会通过toString()执行，几乎总是需要执行字符串连接。当Date对象与比较运算一起使用，转换会通过valueOf()方法执行，几乎总是要执行时间数值之间的比较。原因是Date()既有toString()方法也有valueOf()方法。

大多数对象要么没有valueOf()方法，要么没有一个能够返回有用结果的valueOf()方法。当对象与+运算一起使用时，通常会得到字符串连接而非加法运算。当对象与比较运算符一起使用时，通常会得到字符串的比较而非数值的比较。

定制一个valueOf()方法的对象，定义一个返回数字的valueOf()方法，可对自己的对象使用算术运算符和其他运算符，但把对象和一个字符串相加看了能无法得到想要的结果：toString()方法不再被调用，并且表示valueOf()所返回的数字的字符串和该字符串的连接起来。

valueOf方法非toNumber（）,他的工作是把一个对象转换为一个合理的基本类型的值，故某些对象可能会返回字符串的valueOf()方法。



**传值和传址**

3种数据值操作方式：

（1）复制它，如把他赋值给一个新变量

（2）作为参数传给一个函数或方法

（3）把他与另外的一个值比较

|      | 传值                                                         | 传址                                                         |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 复制 | 实际复制的是值，存在两个不同的独立的拷贝                     | 赋值的只是对数值的引用，如果通过这个新的引用修改了数值，这个改变对最初的引用来说也是可见。 |
| 传递 | 传递给函数的是值的一个独立的拷贝，对他的改变在函数外部没影响 | 传递给函数的是对数值的一个引用，如果函数通过传递给他的引用改变了数值，这个改变在函数外部也可见。 |
| 比较 | 比较的是两个独立的值（通常组字节比较），以判断它们是否相同   | 比较的是两个引用，以判断他们引用的是否是同一数值。对两个不同数值的引用不相等，即使这两个数值是由相同的字节构成的。 |

**基础类型和引用类型**

基本数据类型通过传值来操作，引用类型通过传址来操作。

数字和布尔类型在JavaScript 中都是基本类型。对象、数组、函数是引用类型。字符串实际上并不能符合基本类型和引用类型的两种分类方法。

```js
var n=1;
var m=n;
funciton add_to_total(total,x){
    total=total+x;
}
add_to_total(n,m);
if(n==1)m=2;//n==1=>true,m=2


var xmas=new Date(2007,11,25);
var solstice=xmas;
solstice.setDate(21);
xmas.getDate();//21
(xmas==solstice)//true

var xmas1=new Date(2007,11,24);
var solstice_plus=new Date(2007,11,24);
(xmas1==solstice_plus)//false


function addNum(totals,x){//totals所有的引用都改变
    totals[0]=totals[0]+x;
    totals[1]=totals[1]+x;
    totals[2]=totals[2]+x;    
}

function addNum1(totals,x){//只有传进来的引用改变
    var newTotals=new Array(3);
    newTotals[0]=totals[0]+x;
    newTotals[1]=totals[1]+x;
    newTotals[2]=totals[2]+x; 
    totals=newTotals;
}

//比较字符串
var s1="hello";
var s2="hell"+"o";
(s1==s2)//true 推测字符串是通过传址来复制和传递的，而他们通过传值来比较
```

**传值和传址小结**

| 类型   | 复制   | 传递   | 比较 |
| ------ | ------ | ------ | ---- |
| 数字   | 传值   | 传值   | 传值 |
| 布尔   | 传值   | 传值   | 传值 |
| 字符串 | 不可变 | 不可变 | 传值 |
| 对象   | 传址   | 传址   | 传址 |

### 

