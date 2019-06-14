# 《Javascript权威指南第5版上》读书笔记2

[TOC]

## 第一部分 核心Javascript

### 第4章 变量

```js
//变量声明
var i;
var j;
var i,j;
var i=0,j=1,k=2;
var msg="hello";
var unVal;//undefined
for(var i=0;i<10;i++)document.write(i,"<br>");
for(var i=0,j=10;i<10;i++,j--)document.write(i*j,"<br>");
for(var i in obj)document.write(i,"<br>");

//变量的作用域1
var scope="global";
function checkscope(){
    var scope="local";
    document.write(scope);
}
checkscope();//"local"

//变量的作用域2
var scope="global";
function checkscope(){
    scope="local";
    console.log(scope);
}
checkscope();//"local"
console.log(scope)//"local"

//变量作用域3
var scope="global";
function checkscope(){
   var  scope="local";
    function nested(){
       var scope="nested";
        console.log(scope);
    }
    nested();
    
}
checkscope();//"nested"

//没有块级作用域1
function test(o){
    var i=0;
    if(typeof o =="object"){
        var j=0;
        for(var k=0;k<10;k++){
            console.log(k);//0~9
        }
        console.log(k)//10
    }
    console.log(j)//0
}

//没有块级作用域2
var scope="global";
function f(){
    console.log(scope);//undefined
    var scope="local";
    console.log(scope);//"local"
}
f();

//没有块级作用域3
function f(){
    var scope;
    console.log(scope);//undefined
    scope="local";
    console.log(scope);//"local"
}
f();

//未定义的变量和未赋值的变量
var x;//undefined
console.log(u)//undeclared
u=3;//3
```

**基本类型和引用类型**

基本类型：数值（8个字节），布尔值（1位），null和undefined

引用类型：对象，数组，函数

```js
//基本类型传值
var a=3.14;
var b=a;
a=4;
console.log(b)//3.14
//引用类型传址
var a=[1,2,3];
var b=a;
a[0]=99;
console.log(b)//[99, 2, 3]
```

**垃圾收集**

Javascript不需像C&C++那样内存必须手动释放，JavaScript的解释器可以检测到合适程序不在使用一个对象了，当他确定了一个对象是无用时（如程序中使用的变量在也无法引用这个对象），他就知道不在需要这个对象，可以把他所占用的内存释放掉

```js
var s="hello";
var u=s.toUpperCase();
s=u;
console.log(s);//"HELLO" 不能再获取原始字符串"hello",程序中没有变量在引用他，释放该空间
```



变量i和对象o属性i之间没有根本的区别，在Javascript中，变量基本数和对象的属性是一样的。

局部变量是一个对象的属性，这个对象成为调用对象，声明周期比全局对象的短，用途相同，在执行一个函数是，函数的参数和局部变量是作为调用对象的属性而存储的，用一个完全独立的对象来存储局部变量使Javascript可以防止局部变量覆盖同名的全局变量的值。

**深入理解变量作用域**

每个JavaScript执行环境都有一个与他关联的作用域链。这个作用域链是一个对象列表或对象链。当JavaScript代码需要查询变量x的值时（一个称为变量名解析的过程）它就开始查看该链的第一个对象。如果第一个对象有一个名为x的属性，那么就采用那个属性的值。如果没有该属性，JavaScript就会继续查询联众的第二个对象，是否有x属性，依此类推。

在JavaScript的顶层代码中（如，不属于任何函数定义的代码），作用域链只由一个对象构成，那就是全局对象。所有的变量都是在这个对象中查询的。如果一个变量并不存在，那么这个变量的值是未定义的。

在一个（非嵌套的）函数中，作用域链是由两个对象构成，第一个是函数的调用对象，第二个就是全局对象，当函数引用一个变量时，首先检查的是调用对象（局部作用域），其次才检查全局对象（全局作用域）。

在一个嵌套函数的作用域链中可以有三个或更多的对象。

| 词法作用域 | 作用域链 | 变量查找 |
|---|---|---|
|  | | 未定义 |
|  | | ↑否 |
| var x=1; | 全局变量  x:1 | 在此定义吗？ → 是 → 获得值 |
|  | ↑ | ↑否 |
| function f(){  var y=2; | 调用f()对象  y:2 | 在此定义吗？→ 是 →获得值 |
|  | ↑ | ↑否 |
| function g( ) { var z=3;} | 调用g()对象  z:3 | 在此定义吗？ → 是 →获得值 |
| } | | ↑ |
|  | | 开始 |

### 第5章  表达式和运算符

直接量表达式的值就是这个直接量本身，变量表达式的值则是该变量所存放或引用的值。

| P优先级 | A结合性 | 运算符                                       | 运算类型           | 执行操作                         |
| ------- | ------- | -------------------------------------------- | ------------------ | -------------------------------- |
| 15      | L       | .                                            | 对象，标识符       | 属性存取                         |
|         | L       | []                                           | 数组，整数         | 数组下标                         |
|         | L       | ()                                           | 函数，参数         | 函数调用                         |
|         | R       | new                                          | 构造函数调用       | 创建新对象                       |
| 14      | R       | ++                                           | lvalue             | 先得增或后递增运算（一元的）     |
|         | R       | --                                           | lvalue             | 先递减或后递减运算（一元的）     |
|         | R       | -                                            | 数字               | 一元减法(负)                     |
|         | R       | +                                            | 数字               | 一元加法                         |
|         | R       | ~                                            | 整数               | 按位取补码的操作（一元的）       |
|         | R       | ！                                           | 布尔值             | 取逻辑补码的操作（一元的）       |
|         | R       | delete                                       | lvalue             | 取消定义一个属性（一元的）       |
|         | R       | typeof                                       | 任意               | 返回数据类型（一元的）           |
|         | R       | void                                         | 任意               | 返回未定义的值（一元的）         |
| 13      | L       | *，/，%                                      | 数字               | 乘法，除法，取余运算             |
| 12      | L       | +，-                                         | 数字               | 加法、减法运算                   |
|         | L       | +                                            | 字符串             | 连接字符串                       |
| 11      | L       | \<\<                                         | 整数               | 左移                             |
|         | L       | \>\>                                         | 整数               | 带符号扩展的右移                 |
|         | L       | \>\>\>                                       | 整数               | 带领扩展的右移                   |
| 10      | L       | <,<=                                         | 数字或字符串       | 小于，小于等于                   |
|         | L       | \>,\>=                                       | 数字或字符串       | 大于，大于等于                   |
|         | L       | instanceof                                   | 对象，构造函数     | 检查对象类型                     |
|         | L       | in                                           | 字符串，对象       | 检查一个属性是否存在             |
| 9       | L       | ==                                           | 任意               | 测试相等性                       |
|         | L       | !=                                           | 任意               | 测试非相等性                     |
|         | L       | ===                                          | 任意               | 测试等同性                       |
|         | L       | !===                                         | 任意               | 测试非等同性                     |
| 8       | L       | &                                            | 整数               | 按位与操作                       |
| 7       | L       | ^                                            | 整数               | 按位异或操作                     |
| 6       | L       | \|                                           | 整数               | 按位或操作                       |
| 5       | L       | &&                                           | 布尔值             | 逻辑与操作                       |
| 4       | L       | \|\|                                         | 布尔值             | 逻辑或操作                       |
| 3       | R       | ?:                                           | 布尔值，任意，任意 | （由三个运算数构成的）条件运算符 |
| 2       | R       | =                                            | lvalue，任意       | 赋值运算                         |
|         | R       | *=,/=,%=,+=,-=,\<\<=,\>\>=,\>\>\>=,&=,^=,\|= | lvalue,任意        | 带操作的赋值运算                 |
| 1       | L       | ,                                            | 任意               | 多重计算的操作                   |



注意：lvalue指的是能够合法地出现在一个赋值表达式左边的表达式。在JavaScript中，变量，对象的属性和数组的元素都是lvalue型，ECMAscript标准允许内部函数返回lvalue类型的值。运算符返回的数据类型不能总是和他的运算数类型相同。

**运算符的结合性**

L表示结合性从左到右，R表示结合性从右到左。

```js
w=x+y+z;

w=((x+y)+z);

x=~~~y;
w=x=y=z;
q=a?b:c?d:e?f:g;

x=~(~(~y));
w=(x=(y=z));
q=a?b:(c?d:(e?f:g));
```

**相等算术运算符（==）和等同运算符（===）**

两者用来检测两个值是否相等，都接受任意类型的运算数，若两个运算数相等，则返回true，否则false。

===等同运算符，采用严格的同一性定义检测两个运算数是否完全等同。

==相等运算符，采用比较宽松的同一性定义（允许进行类型转换）检测两个运算数是否相等。

> ===比较两个值是否完全相等规则：

（1）如果两个值类型不同，它们就不相同。

（2）如果两个值的类型是数字，而且值相同，除非其中一个或两个是NaN（此情况不等同），否则他们等同。值NaN永远不与其他任何值等同，包括它自身。要检测一个值是否NaN，可以使用全局函数isNaN()

（3）如果两个值都是字符串，而且在传中同一位置上的字符完全相同，那么他们就完全等同，如果字符串长度或内容不同则不等同。

（4）如果两个值都是布尔值true，或两个值都是布尔值false，那么他们等同

（5）如果两个值引用的是同一个对象、数组或函数，那么他们完全等同，如果他们引用的是不同对象、数组或函数，他们就不等同，即使两个对象具有完全相同的属性或两个数组具有完全相同的元素。

（6）如果两个值都是null或都是undefined，他们完全相同。

> ==比较两个值是否相等规则：

（1）如果两个值具有相同的类型，那么就检测他们的等同性。如果两个值网球相同，他们就相等，否在不相等。

（2）如果两个值的类型不相同，它们仍然可能相等，使用以下规则和类型转换来检测它们的相等性：

- 如果一个值是null，另一个之是undefined，它们相等
- 如果一个值是数字，另一个值是字符串，把字符串转换为数字，在用转换后的值进行比较。
- 如果一个值是true，把他转化为1，再进行比较。如果一个值是false，把它转化为0，再进行比较。
- 如果一个值是对象，另一个之是数字或字符串，把对象转换为原始类型的值，再进行比较。可以使用对象的toString()方法或valueOf()方法把对象转化成原始类型的值。Javascript合性语言的内部类通常尝试valueOf()转换，再尝试toString()转换，但对于Date类，则先执行toString()转换。不属于Javascript合性语言的对象可以采用Javascript实现定义的方式把自身转换成原始值。
- 其他的数值组合是不相等。

```js
"1"==true//true "1"=>1  true=>1
```

**比较运算符的比较和转换规则**

比较运算符的运算数可以是任意类型。但是比较运算只能再数字和字符串上执行，当不是数字或字符串是将被转换为数字或字符串。

（1）如果两个运算数都是数字，或者被转换成了数字，那么将采取数字比较。

（2）如果两个运算数都是字符串，或者都被转成字符串，那么将作为字符串比较。

（3）如果一个运算数是字符串，或者被转换称类字符串，而另一个运算数是数字，或者被转换成了数字，那么运算符会将字符串转换为数字，然后执行数字比较。如果字符串不代表数字，将会被转换成NaN，比较结果为false

（4）如果对象可以被转换成数字或者字符，JavaScript将执行数字转换。如可以从数字的角度比较Date对象，即，比较两个日期，以判断那个日期早于另一个日期是有意义的。

（5）如果运算数都不能被成功地转换为数字或字符串，比较运算返回false

（6）如果某个运算数是NaN，或者被转换成NaN，比较运算返回false

**in 运算符**

in运算符要求其左边的运算数是一个字符串，或者可以被转换成字符串，右边的运算数是一个对象或数组。如果该运算符左边的值是其右边对象的一个属性名，则返回true；

```js
var point={x:1,y:2};
var hasX="x" in point;//true
var hasZ="z" in point;//false
var ts="toString" in point;//true
```

**instanceof运算符**

instanceof运算符要求其左边的运算数是一个对象，右边的运算数是对象类的名字。如果该运算符左边的对象是右边类的一个实例，则返回true，否则返回false。

在Javascript中，对象类是由用来初始化它们的构造函数定义的，因此instanceof运算符右边的运算数应该是一个构造函数的名字，注意所有对象都是Object类的实例。若instanceof的左运算数不是对象或者右边的运算数是一个对象，而非一个构造函数，则会返回false。若右边运算数根本就不是对象，则返回运行错误

```js
var d=new Date();
d instanceof Date;//true
d instanceof Object;//true
d instanceof Number;//false

var a=[1,2,3];
a instanceof Array;//true
a instanceof Object;//true
a instanceof RegExp;//false
```



**字符串运算符**

```js
1+2//3
"1"+2//"12"
"1"+"2"//"12"
11<3//false
"11"<"3"//true 
"11"<3//false  "11"=>11
"one"<3//false  "one"=>NaN
s=1+2+" hello";//"3 hello"
t="hello "+1+2;//"hello 12"
```

**赋值运算符**

| 运算符  | 示例      | 等价等式   |
| ------- | --------- | ---------- |
| +=      | a+=b      | a=a+b      |
| -=      | a-=b      | a=a-b      |
| *=      | a*=b      | a=a*b      |
| /=      | a/=b      | a=a/b      |
| %=      | a%=b      | a=a%b      |
| \<\<=   | a\<\<=b   | a=a\<\<b   |
| \>\>=   | a\>\>=b   | a=a\>\>b   |
| \>\>\>= | a\>\>\>=b | a=a\>\>\>b |
| &=      | a&=b      | a=a&b      |
| \|=     | a\|=b     | a=a\|b     |
| ^=      | a^=b      | a=a^b      |

**typeof运算符**

一元运算符，放在运算数之前，运算数可以是任何类型，返回一个字符串说明该运算数类型。

```js
typeof 123;//number
typeof 'abc';//string
typeof true;//boolean
typeof {x:1,y:2};//object
typeof [1,2,3];//object
typeof null;//object
function f(x){return x*x;}
typeof f;//function
var unValue;
typeof unValue;//undefined
typeof new Number(123);//object
typeof new String("abc");//object
typeof new Boolean(true);//object
typeof new Date();//object
typeof /java/;//object
```

