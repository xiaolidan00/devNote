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

**delete运算符**

delete运算符见删除运算数说制定的对象的属性、数组元素或变量。如果删除操作成功，返回true，否则返回false。并非所有的属性和变量都可以删除的，某些内部核心属性和客户端属性不能删除，用var声明的用户定义变量也不能删除。如果delete使用的运算数是一个不存在的属性，返回true。ECMAScript标准规定，当delete的运算数不是属性、数组元素或变量时返回true。

注意：删除属性、变量或数组元素不只是把他们的值设置为undefined，当删除一个属性后，该属性将不再存在。

delete所能影响的只是属性的值，并不能影响被这些属性引用的对象。

```js
var o={x:1,y:2};
delete o.x;//true
typeof o.x;//undefined
delete o.x;//true   o.x =》不存在
delete o;//不能删除已声明的变量，false
delete 1;//不能删除整数，true
x=1;
delete x;//true
console.log(x);//Runtime error:x is not defined
```



**void运算符**

void可以出现再任何类型的操作数之前，它总是舍弃运算数的值，然后返回undefined。

void的另一个用途是专门生成undefined值。



### 第6章 语句

```js
//表达式语句
s="Hello "+name;
i*=3;
counter++;
delete o.x;
alert("Welcome, "+name);
window.close();
Math.cos(x);
cx=Math.cos(x);
//复合语句
{
    x=Math.PI;
    cx=Math.cos(x);
    alert("cos("+x+")="+cx);
}
```

正式的Javascript语法规定每个复合语句可以包含一个子语句，那么使用语句块，就可以将任意数量的 语句放在这个子语句中。

Javascript解释器执行符合语句是，只是一句一句的按照编写顺序执行它的语句，通常JavaScript解释器会执行说有的语句，但再后写情况下，复合语句会意外终止。发生这种终止情况主要是由于复合语句含有break语句、continue语句，return语句或throw语句，或者它引发了错误，或者它调用的函数引发了不能捕捉的错误或抛出了不饿能捕捉的异常。

**if语句**

```js
//第一种形式 
if(username=="")
username="test";

if((address==null)||(address=="")){
address="undefined";
alert("Please specify a mailing address");
 }

//第二种形式

 if(username!=null)
     alert("Hello "+username+"\n Welcome to my blog.");
else{
    username=prompt("Welcome!\n What is your name?");
    alert("Hello "+username);
}
//第三种形式
if(n%3==0)
    alert("Num:"+n+" is  a multiple of 3");
else if(n%2==0)
    alert("Num:"+n+" is  a multiple of 2");
else 
    alert("Num:"+n+" is not a multiple of 3 or 2");
```

**switch语句**

```js
function convert(x){
    switch(typeof x){
        case "number":
            return x.toString(16);
        case "string":
            return '"'+x+'"';
        case "boolean":
            return x.toString().toUpperCase();
        default:
            return x.toString();
    }
}

//case 关键字后跟随的是数字和字符串的直接量，但ECMAscript v3标准允许跟随任意表达式
case 60*60*24:
case Math.PI:
case n+1:
case a[0]:
```

switch语句首先计算switch关键字后的表达式，然后按照出现的顺序计算case后的表达式，知道找到与switch表达式的值相匹配的case表达式位置。由于匹配case表达式时用===等同运算符判定的，而不是==相等运算符判定的，所以表达式必须时没有类型转换的情况下进行匹配。

**while语句**



```js
while(expression) 
    statement

var count=0;
while(count<10){
    console.log(count);//0~9
    count++;
}
console.log(count);//10
```

while先计算expression的值，如果是false，转而执行下一条语句，如果是true，执行循环体statement，重复计算expression判定是否执行循环体。

注意：while(true)会创建一个无线循环。

**do/while语句**

do/while循环和while循环相似，do/while是底部检测循环表达式，意味着循环体至少会被执行一次,while是循环顶部检测循环表达式。

两者的区别：

（1）do循环要求必须使用关键字do来标识循环的开头，用关键字while来标志循环的结尾并引入循环条件。

（2）和while循环不同，do循环时用分号结尾的。这是因为do循环的结尾循环条件，而不是循环体的结束。

**for语句**

```js
for(initialize;test;increment)
    statement;
//等价于while循环
initialize;
while(test){
    statement;
    increment;
}

for(i=0,j=10;i<10;i++,j--)
    sum+=i*j;
```

**for/in语句**

```js
var obj={a:1,b:"hello",c:true};
for(var key in obj)
    console.log("obj["+key+"]="+obj[key]);


var a=new Array();
var i=0;
for(a[i++] in obj);//空循环体


console.log(i);//3
for(i in a)
    console.log(i,a[i]);
/*
0 "a"
1 "b"
2 "c"
*/
```

注意：for/in循环中的variable可以时任意的表达式，只要他的值适用于赋值表达式的赋值表达式即可。

**标签语句**

被标记的语句通常都是那些循环语句，即while，do/while、for和for/in语句。通过给循环命名，就可以使用break，continue退出循环或退出循环的某一次迭代。

```js
identifier:statement
//identifier可以时任何合法的JavaScript标识符，不可一世保留字
myAction:
while(token!=null){
    //statement
}

```

**break语句**

break会使程序立刻退出包含在最内层的循环或退出一个switch语句,break后可以跟标签名，退出对应标签名的循环。

```js
for(i=0;i<a.length;i++){
    if(a[i]==target){
        break;
    }
}

switch(n){
    case 1:
        console.log("n=1");
    case 2:
        console.log("n=2")
        break;
    default:
        console.log("n=?")
}
/*
当输入n=1时
结果:n=1,n=2
当输入n=2时
结果:n=2
当输入n=3时
结果:n=?
*/
var val=0;
myAction:
while(true){
    console.log(val);
    if(val==3){
       break myAction; 
    }    
    val++;
}
//0 1 2 3

outerloop:
for(var i=0;i<10;i++){
    innerloop:
    for(var j=0;j<10;j++){
        if(j>3)break;
        if(i==2)break innerloop;
        if(i==4)break outerloop;
        console.log(i,"+",j,"=",i+j);
    }
}
/*
0+0=0
0+1=1
0+2=2
0+3=3
1+0=1
1+1=2
1+2=3
1+3=4
//j>3 break innerloop
//i==2 break innerloop
3+0=3
3+1=4
3+2=5
3+3=6
//i==4 break outerloop
*/
```

**continue语句**

continue语句只能用在while、do/while、for、for/in的循环体之中，再其他地方使用它会引起语法错误。

执行continue时，封闭循环的当前跌掉会被终止，开始执行下次迭代。

- 在while循环体中，会再次检测循环开头的expression，如果他是true，见从头开始执行循环体。
- 在do/while循环体中，会跳到循环的底部，在顶部开始下次循环之前，会在此先检测循环条件。
- 在for循环中，先计算increment表达式，然后再检测test表达式以确定是否应该执行下次迭代。
- 在for/in循环中，将以下一个付给循环变量的属性名再次开始新的迭代。

```js
for(var i=0;i<10;i++){
    if(i==5){
        continue;
    }
    console.log(i);//1~4 6~9 跳过5
}

outerloop:
for(var i=0;i<4;i++){
    innerloop:
    for(var j=0;j<5;j++){
        if(j==1)continue;
        if(i==2)continue outerloop;
        if(j==3)continue innerloop;
        console.log(i,j)
    }
}
/*
0 0
// j==1 continue innerloop
0 2
//j==3 continue innerloop
0 4
1 0
// j==1 continue innerloop
1 2
//j==3 continue innerloop
1 4
// i==2 continue outerloop
3 0
// j==1 continue innerloop
3 2
//j==3 continue innerloop
3 4
*/
```

**function和return**   

```js
console.log(f(4));//16=>function f(x)
var f=0;
function f(x){return x*x;}
console.log(f)//0 =>var f

funciton showObj(obj){
    if(obj==null)return;//终止函数执行
    console.log(obj)
}
```

**throw和try/catch/finally**

```js
function factorial(x){
    if(x<0) throw new Error("x must not be negative");
    for(var f=1;x>1;f*=x,x--)/*empty*/;
        
        return f;
}
console.log(factorial(4));//24
console.log(factorial(-4));//Uncaught Error: x must not be negative

try{
    var n=prompt("please enter a positive integer:");
    var f=factorial(n);
    alert(n+"!="+f);
}catch(e){
    console.log(e);//Error: x must not be negative
}
//finally块中包括清楚代码，无论try从句中是否由break；continue，return，finally代码块都会被执行。
var i=0,total=0;
while(i<a.length){
    try{
        if(typeof a[i]!="number"||isNaN(a[i]))
            continue;
        total+=a[i];
    }finally{
        i++;//即使使用了continue，还是会执行i++
    }
}
```

**with语句**

with暂时修改作用域链

```js
with(object)
    statement;
```

将object添加到作用域链头部，然后执行statement，再把作用于恢复到原始状态。

```js
frames[1].document.forms[0].address.value;

with(frames[1].document.forms[0]){
    name.value="";
    address.value="";
    email.value="";
}
//等价于
var form=frames[1].document.forms[0];
form. name.value="";
form.address.value="";
form.email.value="";
```

**JavaScript语句小结**

| 语句     | 语法                                                         | 用途                                                    |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------- |
| break    | break；break label；                                         | 推出最内层循环或退出switch语句，或退出label指定的循环。 |
| case     | case expression：                                            | switch语句中标记一个语句                                |
| continue | continue;continue label;                                     | 重新开始最内层循环或重新开始label指定的循环；           |
| default  | defalut:                                                     | switch中标记默认语句                                    |
| do/while | do statement while(expression);                              | while循环的一种替代形式                                 |
| 空语句   | ;                                                            | 什么都不干                                              |
| for      | for(initialize;test;increment)statement                      | 一种易用的循环                                          |
| for/in   | for(variable in object)statement                             | 遍历一个对象的属性                                      |
| function | function funName(\[arg1,\[……,argn\]\]){statements}           | 声明一个函数                                            |
| if/else  | if(expression)statement1 \[else statement2\]                 | 有条件的执行代码                                        |
| label    | identifier:statement                                         | 给statement指定一个名字identifier                       |
| return   | return  \[expression\];                                      | 由函数返回或由函数返回expression的值                    |
| switch   | switch(expression){statements}                               | 用case或default语句标记的多分支语句                     |
| throw    | throw expression；                                           | 抛出一个异常                                            |
| try      | try{statements}catch(identifier){statements}finally{statements} | 捕捉一个异常                                            |
| var      | var name1\[=value1\]\[,……，nameN\[=valueN\]\]                | 声明或者初始化变量                                      |
| while    | while(expression)statement                                   | 一种基本循环结构                                        |
| with     | with(object){statement}                                      | 扩展作用域链（不推荐）                                  |

### 第7章对象和数组

对象是一种复合数据类型，它们将多个数据值集中在一个单元中，而且允许使用名字来存取这些值。

对象是一个无序的属性集合，每个属性都有自己的名字和值。存储在对象中的已命名的值既可以是数字和字符串这样的原始值，也可以是对象。

```js
var empty={};
var point={x:0,y:0};
var circle={x:point.x,y:point.y+1,radius:2};
var homer={
    "name":"Homer Simpson",
    "age":34,
    "married":true,
    "occupation":"plant operator",
    "email":"homer@example.com"
};

var a=new Array();
var d=new Date();
var r=new RegExp("javascript","i");

var book={};
book.title="JavaScript";
book.chapter1=new Object();
book.chapter1.title="Introduction to JavaScript";
book.chapter1.pages=11;
book.chapter2={title:"Lexical Structure",pages:6}

alert("Outline:"+book.title+"\n"+
     "Chapter 1"+book.chapter1.title+"\n"+
     "Chapter 2"+book.chapter2.title);
book.title="My JavaScript";

//属性枚举
function displayPropsName(obj){
    var names="";
    for(var name in obj)name+=name+"\n";
    console.log(name);
}
//for/in循环列出的属性并没有特定的顺序，虽能美剧出所有用户定义的属性，但不能美剧出某些预定义的属性和方法。

//检查属性的存在性
if("x" in point) console.log("there is attribute x in object point");
if(point.x!==undefined)console.log("there is attribute x in object point");
if(point.drawCircle)point.drawCircle();

//删除属性
delete book.chapter2;//删除属性不仅是把属性设置为undefined，实际上是从对象一处了属性，在删除后，for/in将不会枚举该属性，并且in运算符也不会检测到该属性。

//作为关联数组的对象
object.property
object["property"]

var addr="";
for(i=0;i<4;i++){
    addr+=customer["address"+i]+"\n"
}


var value=0;
for(stock in portfolio){
    value+=get_share_value(stock)*portfolio(stock);
}
```

**通用的Object属性和方法**

```js
//constructor属性
var d=new Date();
d.constructor==Date;//true

if((typeof o=="object")&&(o.constructor==Date)){//确认未知值的类型为Date
    
}

if((typeof o=="object")&&(o instanceof Date)){//确认未知值的类型为Date
    
}

//toString()方法
//使用+把字符串与对象连接或者alert()传递一个对象时对象会调用toString()
var s={x:1,y:2}.toString();//"[object Object]"默认toString()并不能显示太多信息。

//hasOwnProperty()方法
//如果对象用一个单独的字符串参数指定的名字来本地定义一个非继承的属性，hasOwnProperty方法就返回true,否则false
var o={};
o.hasOwnProperty("undef");//false
o.hasOwnProperty("toString");//false
Math.hasOwnProperty("cos");//true

//propertyIsEnumerable()方法
//如果对象用一个单独的字符串参数所指定的名字来定义一个非继承的属性，并且如果这个属性可以在for/in循环中枚举，propertyIsEnumerable()返回true,否则false
//注意，一个对象的所有的用户定义的属性都是可以枚举的，不能枚举的属性通常都是继承的属性。这个方法几乎总会和hasOwnProperty()返回相同结果
var o={x:1};
o.propertyIsEnumerable("x");//true
o.propertyIsEnumerable("y");//false
o.propertyIsEnumerable("valueOf");//false

//isPrototypeOf()方法
//如果isPrototypeOf()方法所属于的对象时参数的原型对象，那么返回true,否则false
var o={};
Object.prototype.isPrototypeOf(o);//true o.constructor==Object
Object.isPrototypeOf(o);//false
o.isPrototypeOf(Object.prototype);//false
Function.prototype.isPrototypeOf(Object);//true Object.constructor=Function
```



**数组**

数组时一个有序的、值的集合，每个值叫做一个元素，每个元素在数组中都有一个数字化位置，叫做下标。一个数组的元素可以具有任意的数据类型，同意数组的不同元素可以具有不同的类型。数组可以包含其他数组。

```js
var primes=[2,3,4,5];
var music=[1.2,"abc",false];
var base=1024;
var table=[base,base+1,base+2,base+3];
var b=[[1,{x:1,y:2}],[2,{x:3,y:4}]];
var count=[1,,3];//3个元素，中间的为未定义
var undefs=[,,,,];//五个未定义元素

//创建数组的另一种方式使用Array()构造函数
//无参数
var a=new Array();//无元素的空数组，等价于[]
//显式地指定数组前n个怨怒是的值
var a=new Array(1,2,3.3,"test");//数组赋值时时从元素0开始
//传递给它一个数字参数，指定数组的长度
var a=new Array(10);//十个undefined元素的数组

//数组元素的读写
value=a[0];
a[1]=3.14;
i=2;
a[i]=3;
a[i+1]="hello";
a[a[i]]=a[0];
my['abc']*=2;//my.abc=my.abc*2;
a[-1.23]=true;//=>a["-1.23"]=true


//添加数组新元素 JavaScript数组时习俗的，意味着数组的下标不许连续的数字范围
a[10]=1;
a[0]=111;
a[10000]="test";
//数组元素也可以被添加到对象中
var c=new Circle(1,2,3);
c[0]="hahaha";//定义名为"0"的对象属性

//删除数组元素
delete a[0];
Array.shift();//删除数组的第一个元素
Array.pop();//删除数组的最后一个元素
Array.splice();//删除数组中连续范围的元素


//数组的长度
var a=new Array();//a.length==0
a=new Array(10);////a.length==10 10个未定义元素
a=new Array(1,2,3);//a.length==3
a=[4,5];//a.length==2
a[5]=-1;//a.length==6 3个未定义元素  [4, 5, empty × 3, -1]
a[49]=0;//a.length==50 46个未定义元素 [4, 5, empty × 3, -1, empty × 43, 0]

//遍历数组
var fruits=["mango","banana","cherry","pear"];
for(var i=0;i<fruits.length;i++){
    console.log(fruits[i]);
}

for(var i=0;i<fruits.length;i++){
    if(fruits[i])console.log(fruits[i]);
}

var lookup_table=new Array(1024);
for(var i=0;i<lookup_table.length;i++)
    lookup_table[i]=parseInt(Math.random());

//截断或增长数组
var a=[1,2,3];
a.length=2;
console.log(a);//[1, 2]
a.length=5;
console.log(a);//[1, 2, empty × 3]

//多维数组
var table=new Array(10);
for(var i=0;i<table.length;i++)
    table[i]=new Array[10];
for(var row=0;row<table.length;row++){
    for(var col=0;col<table[row].length;col++){
        table[row][col]=row*col;
    }
}
console.log(table[5][7]);//5*7=35
```

**数组的方法**

```js
//join()方法 把数组的所有元素转换成字符串，然后连接起来
var a=[1,2,3];
var s=a.join();//"1,2,3"
var s1=a.join(",");//"1,2,3"

//split()方法，通过一个字符串分割成几个片段来创建数组
var ss="a=1&b=3&c=5";
var aa=ss.split("&");//["a=1", "b=3", "c=5"]
var aaa=aa[0].split("=");//["a", "1"]

//reverse()方法 将颠倒数组元素的顺序并返回颠倒后的数组。
var a=new Array(1,2,3);//[1,2,3]
a.reverse();//[3,2,1]

//sort()方法 在原数组上对数组元素进行排序，返回排序后的数组。默认按数字字母顺序对数组元素进行排序
var a=new Array("banana","cherry","apple");
a.sort();//["apple", "banana", "cherry"]
var b=[33,4,111,22];
b.sort();//[111, 22, 33, 4]
b.sort(function(a,b){
    return a-b;
});//[4, 22, 33, 111]

//concat()方法 创建并返回一个数组，原始数组元素跟随concat()参数,可以是元素或数组
var a=[1,2,3];//以下操作基于a=[1,2,3]
a.concat(4,5);//[1,2,3,4,5];
a.concat([4,5]);//[1,2,3,4,5];
a.concat([4,5],[6,7]);//[1,2,3,4,5,6,7];
a.concat(4,[5,[6,7]]);//[1,2,3,4,5,[6,7]];

//slice()方法 返回指定数组片段，或者子数组 第一个参数是截取的开始位置，第二个参数是截取结束的位置
var a=[1,2,3,4,5];
a.slice(0,3);//[1,2,3]
a.slice(3);//[4,5] 第二个参数未定义默认是到最后一个元素
a.slice(1,-1);//[2,3,4]//第二个参数负数是相对与最后一个元素而言,-1数组的倒数第一个元素=》4
a.slice(-3,-2);//[3] -3倒数第三个元素=》2，-2倒数第二个元素=》3

//splice()方法 删除或插入数组元素，直接在原数组上修改，不同slice()和concat()那样创建新数组。第一个参数是开始删除位置，第二个参数是删除元素个数，第三个以后参数为插入的元素
var  a=[1,2,3,4,5,6,7,8];
a.splice(4);//return [5,6,7,8];  a=[1,2,3,4];第二个参数不指定,默认从开始位置到结尾的所有元素删除
a.splice(1,2);//return [2,3];a=[1,4];
a.splice(1,1);//return [4];a=[1];
var b=[1,2,3,4,5];
a.splice(2,0,'a','b');//return [];a=[1,2,'a','b',3,4,5];
a.splice(2,2,[1,2],3);//return ['a','b'];a=[1,2,[1,2],3,3,4,5];
//与concat()不同，splice()不将插入的数组参数展开，插入还是数组本身。

//push()方法和pop()方法 直接在原数组上修改。
//push()将一个或多个新元素附加到数组的尾部，然后返回数组的新长度。
//pop()将删除的数组的最后一个元素，减少数组的长度，返回它删除的值。
var stack=[];
stack.push(1,22);//return 2;stack=[1,22] 
stack.pop();//return 22 ;stack=[1]
stack.push(3);//return 2;stack=[1,3]
stack.pop();//return 3;stack=[1];
stack.push([4,5]);//return 2; stack=[1,[4,5]]
stack.pop();//return [4,5];stack=[1];
stack.pop();//return 1;stack=[]

//unshift()方法和shift()方法
//unshift()将一个或多个元素添加到数组的头部，然后把以由的元素移动到下标较大的位置以腾出空间，它返回的是数组的新长度。
//shift()删除并返回数组的第一个元素，然后将后面的所有元素都向前移动以填补第一个元素留下的空白。
var a=[];
a.unshift(1);//return 1;a=[1]
a.unshift(22);//return 2;a=[22,1]
a.shift();//return 22;a=[1]
a.unshift(3,[4,5]);//return 3;a=[3,[4,5],1]
a.shift();//return 3;a=[[4,5],1]
a.shift();//return [4,5];a=[1]
a.shift();//return 1;a=[]

//toString()方法和toLocaleString()方法
//注意 toString()返回和无参数的join()方法返回的字符串相同
//toLocaleString()是toString()局部或的版本，它将调用每个元素的toLocaleString()把数组元素转换成字符串，然后把生成的字符串用特定的分隔符字符串连接起来
[1,2,3].toString();//"1,2,3"
[1,[2,"c"]].toString();//"1,2,c"


//indexOf()方法和lastIndexOf()快速查找特定值
[true,"a",1,55].indexOf(1)；//2
[true,"a",1,55].lastIndexOf(1)；//2
[true,"a",1,55].lastIndexOf(true)；//0

//forEach()每个元素调用一个指定函数
var aa=[true,"a",1,55];
aa.forEach(function(a,b){console.log(a,b)})
/*
true 0
"a" 1
1  2
55  3
undefined
*/

//map()返回数组中的每个元素传递给指定函数所获取结果的数组
var aa=[true,"a",1,55];
aa.map(function(a,b){return a+":"+b;})
/*
["true:0", "a:1", "1:2", "55:3"]
*/

//filter()返回一个给定的断言函数返回tru的元素组成一个数组
var aa=[true,"a",1,55];
aa.filter(function(a){return a>0});//[true, 1, 55]
```

