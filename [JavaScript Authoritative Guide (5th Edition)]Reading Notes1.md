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

