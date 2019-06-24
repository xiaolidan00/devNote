# 《Javascript权威指南第5版上》读书笔记3

[TOC]

## 第一部分 核心Javascript

### 第8章 函数

```js
function print(msg){
    document.write(msg,"<br>")
}
function distance(x1,y1,x2,y2){
    var dx=x2-x1;
    var dy=y2-y1;
    return Math.sqrt(dx*dx+dy*dy);
}
function factorial(x){
    if(x<=1) return 1;
    return x*factorial(x-1);
}
print("hello");//不包含return 返回undefined
factorial(5);//5*4*3*2*1=120
distance(0,0,2,2);//2倍根号2
distance(0,0,2);//NaN 因为y2为undefined

//嵌套的函数
function outerFun(){
    function innerFun(x){
        return x*x;
    }
    return Math.sqrt(innerFun(a)+innerFun(b));
}

function f(x){return x*x;}
var f=function(x){return x*x;}

var ff= function fact(x){
    if(x<=1)return 1;
    else return x*fact(x-1);
}//把函数的引用存储到ff变量中，并未将函数引用存储到fact变量中，只是允许函数体用这个名字来引用自身。

f[0]=function(x){return x*x;};
a.sort(function(a,b){return a-b;});
var tensquared=(function(x){return x*x;})(10);//10*10=100,定义并调用函数

//可选参数
function copyPropertyNamesToArray(o,/*optional*/a){
    if(!a)a=[];
    //a=a||[];
    for(var property in o)a.push(property);
    return a;
}
var a=copyPropertyNamesToArray(o);
copyPropertyNamesToArray(p,a);
```

**可变长度的参数列表对象:Argument对象**

```js
//Arguments对象允许完全地存储那些实际参数值，即使某些或全部参数名还没被命名。
function f(x,y,z){
    if(arguments.length!=3){
        throw new Error("function f called with "+arguments.length+
                       " arguments, but is expects 3 arguments");
    }else{
        console.log(arguments[0],arguments[1],arguments[2]);//对应x,y,z
        return Math.sqrt(x*x+y*y+z*z);
    }    
}
//类似Math.max()求最大值
function max(/*……*/){
    var m=Number.NEGATIVE_INFINITY;
    for(var i=0;i<arguments.length;i++){
        if(arguments[i]>m)m=arguments[i];
    }
    return m;
}
var largest=max(11,10,3,5,60,33,45,43);//60

function f(x){
    print(x);//原始x值
    arguments[0]=null;//通过arguments数组改变参数值
    print(x);//null
}

//Arguments对象定义了callee属性，用来引用当前正执行的函数，可以用来允许对未命名函数递归地调用自身。
function(x){
    if(x<=1)return 1;
    return x*arguments.callee(x-1);
}
```



**把对象属性用作参数**

```js
function arraycopy(/*array*/from,/*index*/ from_start,/*array*/to,/*index*/ to_start,/*integer*/length){
    //code 
}
function easycopy(args){    arraycopy(args.from,args.from_start||0,args.to,args.to_start||0,args.length);
}
var a=[1,2,3,4];
var b=new Array(4);
easycopy({from:a,to:b,length:4});
```



**参数类型**

```js
function sum(a){//参数类型判断，如果非正确类型报错
    if((a instanceof Array)||(a && typeof a=="object"&&"length" in a)){
        var total=0;
        for(var i=0;i<a.length;i++){
            var element=a[i];
            if(!element)continue;
            if(typeof elment=="number")total+=element;
            else {
                throw new Error("sum():all array element must be numbers");
                break;
            }           
        }
        return total;
    }else{
        throw new Error("sum():argument must be an number array");
    }
}

function flexisum(a){//转换参数为合适的类型再计算
    var total=0;
    for(var i=0;i<arguments.length;i++){
        var element=arguments[i];
        if(!element)continue;
        var n;
        switch(typeof element){
            case "number":
                n=element;
                break;
            case "object":
                if(element instanceof Array)
                    n=flexisum.apply(this,element);
                else n=element.valueOf();
                break;
            case "function":
                n=element();
                break;
            case "string":
                n=parseFloat(element);
                break;
            case "boolean":
                n=element?1:0;
                break;            
        }
        if(typeof n=="number"&&!isNaN(n))total+=n;
        else throw new Error("sum():can't convert "+element+" to number");
    }
    return total;
}
```



**作为数据的函数**

```js
function square(x){return x*x;}

var a=square(4);
var b=square;
var x=b(5);

var o=new Object();
o.square=function(x){return x*x;}
y=o.square(16);

var a=new Array()；
a[0]=function(x){return x*x;};
a[1]=20;
a[2]=a[0](a[1]);

function add(x,y){return x+y;}
function subtract(x,y){return x-y;}
function multiply(x,y){return x*y;}
function divide(x,y){return x/y;}

function operation(operator,operand1,operand2){
    return operator(operand1,operand2);
}
var i=operate(add,operate(add,2,3),operate(multiply,4,5));//(2+3)+(4*5)

var operators={
    add:function(x,y){return x+y;},
    subtract:(x,y){return x-y;},
    multiply:(x,y){return x*y;},
    divide:(x,y){return x/y;} ,
   	pow:Math.pow
}

function operate2(op_name,operand1,operand2){
    if(typeof operators[op_name]=="function")
        return operators[op_name](operand1,operand2);
    else throw "unknown operator";
}
var j=operate2("add","hello",operate2("add"," ","world"));//"hello world"
var k=operate2("pow",10,2);
```

**作为方法的函数**

```js
o.m=f;
o.m();
o.m(x,x+2);
var calculator={
    operand1:1,
    operand2:1,
    compute:function(){
        this.result=this.operand1+this.operand2;
    }
};
calculator.compute();
print(calculator.result);//2

rect.setSize(width,height);
setRectSize(rect,width,height);
```



**函数的属性和方法**

```js
//arguments.length传递的参数个数 Function.length函数声明的参数个数
function check(args){
    var actual=args.length;
    var expected=args.callee.length;
    if(actual!=expected){
        throw new Error("wrong number of arguments:expected:"+expected+";actually passed "+actual);
    }
}
function f(x,y,z){
    check(arguments);
    return x+y+z;
}
//定义自己的函数属性
uniqueInteger.counter=0;//将属性存储在Function属性中
function uniqueInteger(){
    return uniqueInteger.counter++;
}
//apply()和call()第一个参数都是要调用的函数对象，在函数体内这一参数是关键字this的值。call()的剩余参数是传递给调用的函数的值。
f.call(o,1,2);
//类似
o.m=f;
o.m(1,2);
delete o.m;
f.apply(o,[1,2]);
var biggest=Math.max.apply(null,array_of_numbers);
```



**工具函数**

```js
function getPropertyNames(/*object*/o){
    var r=[];
    for(name in o)r.push(name);
    return r;
}
function copyProperties(/*object*/ from,/*optional object*/to){
    if(!to)to={};
    for(p in from)to[p]=form[p];
    return to;
}
function copyUnfefinedProperties(/*object*/ from,/*optional object*/to){
    for(p in from){
        if(!p in to)to[p]=form[p];
    }
}
function filterArray(/*array*/a,/*boolean*/predicate){
    var results={};
    var length=a.length;
    for(var i=0;i<length;i++){
        var element=a[i];
        if(predicate(element))results.push(element);
    }
    return results;
}
function mapArray(/*array*/a,/*function*/f){
    var r=[];
    var length=a.length;
    for(var i =0;i<length;i++)r[i]=f(a[i]);
    return r;
}
function bindMethod(/*object*/o,/*function*/f){
    return function(){return f.apply(o,arguments);};
}
function bindArguments(/*function*/f/*,initial arguments...*/){
    var boundArgs=arguments;
    return function(){
        var args={};
        for(var i=1;i<boundArgs.length;i++)args.push(boundArgs[i]);
        for(var i=0;i<arguments.length;i++)args.push(arguments[i]);
        return f.apply(this.args);
    }
}
```



**作为闭包的嵌入函数**

```js
(function(){})()；//未命名函数定义并调用

var x="global";
function f(){
    var x="local";
    function g(){console.log(x);}
    g();
}
f();//local

function makeFun(x){
    return function(){return x;};
}
var a=[makeFun(1),makeFun(2),makeFun(3)];
console.log(a[0]());
```

- 嵌套函数的定义引用了调用对象，由于调用对象在这个函数做定义的作用域链的顶端，若嵌套函数只是在外围函数的内部使用，那么对嵌套函数的唯一的引用在调用函数之中。当外围函数返回的时候，嵌套函数引用了调用用的对象，并且调用对象引用了嵌套函数，但没有其他引用它们两者，者两个对象可以进行垃圾收集。
- 若把对嵌套函数的引用保存在一个全局作用域中，使用的嵌套函数作为外围函数的返回值，或把嵌套函数存储为某个对象的属性来做到这一点。该情况下，有一个对嵌套函数的外部引用，并且嵌套函数将他的引用保留给外围函数的调用对象。结果是，外围函数的一次特定调用的调用对象依然存在，函数的参数和局部变量的名字和值在这个对象中得以维持。
- JavaScript代码不会以任何方式直接访问调用对象，但是它所定义的属性是对嵌入函数任何调用的作用域链一部分。如果一个外围函数存储了两个嵌入函数的全局调用，这两个嵌入函数共享同一个调用对象，并且一个函数的一次调用所作的改变对于另一个函数调用可见。
- JavaScript函数是将要执行的代码以及执行这些代码的作用域构成的一个综合体，这种综合体叫做闭包。所有的JavaScript函数都是闭包的。以上的讨论闭包是：当一个嵌套函数被导出到它所定义的作用域外时，当一个嵌套函数以这种方式使用时。

```js
uniqueID=function(){
    if(!arguments.callee.id)arguments.callee.id=0;
    return arguments.callee.id++;
}
//存在问题:任何人都可以把uniqueID.id设置回0，并且违反函数所保证的它不会两次返回相同的值的约定。可以通过在只有该函数能够访问的闭包中存储持久性值的方法来防止这个月的问题：
uniqueID=(function(){
    var id=0;
    return function(){return id++;};    
})();

//使用闭包的私有属性
function makeProperty(o,name,predicate){
var value;
    o["get"+name]=function(){return value;}
    o["set"+name]=function(v){
        if(predicate&&!predicate(v)){
            throw "set"+name+":invalid value "+v;
            else 
                value=v;
        }
    }
}
var o={};
markeProperty(o,"Name",function(x){return typeof x=="string";});
o.setName("Frank");
console.log(o.getName());//Frank
o.setName(0);//"setName:invalid value 0"
```

断点是指函数中代码的执行停止下来，程序员获得一个机会查看变量的值、计算表达式、调用函数等的一个位置。Steve Yen所发明的断点工具，断电技术所使用一个闭包来捕获一个函数中的当前作用域（包括局部变量和函数的参数），并将它于全局的eval()函数组合起来，从而允许查看作用域。

```js
var inspector=function($){return eval($);}//使用$减少在它想要检测的作用域内发生名字冲突的可能性。

//使用闭包的断点
function inspect(inspector,title){
    var expression,result;
    if("ignore" in arguments.callee) return;
    while(true){
        var message="";
        if(title) message=title+"\n";
        if(expression)message+="\n"+expression+" ==> "+result+"\n";
        else expression="";
        message+="Enter an expression to evaluate:";
        expression =promot(message,expression);
        if(!expression)return;
        result=inspector(expression);
    }
}

//使用断点计数计算阶乘
function factorial(n){
    var inspector=function($){return eval($);}
    inspect(inspector,"Entering factorial()");
    var result=1;
    while(n>1){
        result=result*n;
        n--;
        inspect(inspector,"factorial() loop");
    }
    inspect(inspector,"Exiting factorial()");
    return result;
}
```

**Internet Explorer中的闭包和内存泄漏**

IE浏览器对ActiveX对象和客户端的DOM原始使用垃圾收集的一种弱化形式。这些客户端的对象是引用计数的，并且当他们的引用数达到0的时候，就会被释放。当存在循环引用时候，这种方式失效，如，当一个核心JavaScript对象引用一个文档元素，并且稳定原始有一个属性（如一个事件处理器）指回到核心JavaScript对象的时候。

当闭包和IE的客户端变成一起使用时，这种循环闭包经常发生，。使用一个闭包时，闭包函数的调用对象包括所有函数参数和局部变量，所持续的时间都和闭包一样长。如果那些函数参数或局部变量的任意一个引用一个客户端对象，就可能会导致一次内存泄漏。

**Function()构造函数**

Function()构造函数期待任意数目的字符串参数，最后一个参数时函数的函数体。

注意：Function()构造函数并没有传递给任何一个参数来指定他所创建的函数名。和函数直接量一样，Function()构造函数创建了匿名的函数。

- Function()构造函数允许JavaScript代码被动态地创建并在运行时编译。如全局的eval()函数就是这种方式。
- Function()构造函数接卸函数体，并且每次被调用的时候都创建一个新的函数对象。如果构造函数的调用出现在一个循环中，或者出现在一个经常被调用的函数中，那么这个过程效率很低。相反，出现在一个循环或函数终点直接量或者嵌套函数，并不会每次编译。每次遇到一个函数直接量也不会创建不同的函数对象（尽管，在函数定义所在的词法作用域中捕获不同之处，可能需要一个新的闭包）
- Function()创建的函数并不使用词法作用域，相反，它们总是当作最顶层的函数一样来编译。

```js
var f=new Function("x","y","return x*y;");
//等价于
function f(x,y){return x*y;}

var y="global";
function constructFunction(){
    var y="local";
    return new Funciton("return y");    
}
console.log(constructFunction()());//global
```



### 第9章 类和模块

```js
//创建一个新的空对象
new Object();
var array=new  Array(10);
var today=new Date();
//构造函数的工作是初始化一个新创建的对象，设置在使用对象前需要设置的所有属性。
function Rectangle(w,h){
    this.width=w;
    this.height=h;
}
var rect1=new Rectangle(2,4);//rect1={width:2,height:4}
var rect2=new Rectangle(8.5,11);//rect1={width:8.5,height:11}

//原型和继承
function computeAreaOfRectangle(r){
    return r.width*r.height;
}

var r=new Rectangle(8.5,11);
r.area=function(){return this.width*this.height;}
var a=r.area();

function Rectangle(w,h){
    this.width=w;
    this.height=h;
    this.area=function(){this.width*this.height;}
}
var r=new Rectangle(8.5,11);
var a=r.area();

function Rectangle(w,h){
    this.width=w;
    this.height=h;
}
Rectangle.prototype.area=function(){return this.width*this.height;}

var =new Rectangle(2,3);
r.hasOwnProperty("width");//true
r.hasOwnProperty("area");//false
"area" in r;//true
```



| c.area()读取c的area属性          | →    | area并不定义在c自身中，因此观察和c相关的原型对象的属性       | →    | 这里才是area的定义，返回它，就好像它是c自身的属性一样 |
| -------------------------------- | ---- | ------------------------------------------------------------ | ---- | ----------------------------------------------------- |
|                                  |      | A Circle object ,C{r=1.0,x=2.0,y=3.0}                        | →    | ↓                                                     |
| c.pi=4;写入c的pi属性             | →    | pi并不定义在c中，因此将其作为c的自身的新属性创建。           |      | ↓                                                     |
|                                  |      | A Circle object ,C {r=1.0,x=2.0,y=3.0,pi=4}                  |      | 原型对象Circle.prototype{area=Circle_area,pi=3.14159} |
| a=c.pi\*c.r\*c.r读取c的pi和r属性 | →    | pi和r定义在c自身中，因此可以返回在那里找到的值，而不必再费劲查看原型对象 |      | ↑                                                     |
|                                  |      | A Circle object ,d{r=2.1,x=0.0,y=0.0}                        | →    | ↑                                                     |
| a=d.pi\*d.r\*d.r读取d的pi和r属性 | →    | pi并不定义在d自身中，因此查看和d相关的原型对象和属性，r定义在d中，因此，返回这个值而不必查看原型 | →    | 这里是pi的定义，返回它的值，就好像它真的是d的一个属性 |

**扩展内建类型**

```js
String.prototype.endsWidth=function(c){
    return (c==this.charAt(this.length-1));
}
var message="Hello World";
message.endsWidth("h");//false
message.endsWidth("d");//true
```

注意：不能为Object.prototype添加属性，所添加的任何属性和方法都可以用一个 for/in循环来枚举，将他们添加到Object.prototype就会使它们再每个单个的Javascript对象中都可见。一个空的对象{}应该没有枚举属性，任何添加到Object.prototype的内容都会变成这个空对象的一个可枚举属性，那么把对象用作关联数组的代码可能会产生问题。

```js
//IE 4&5 无法使用Function.apply()
if(!Function.prototype.apply){
    Function.prototype.apply=function(object,parameters){
	var f=this;
        var o=object||window;
        var args=parameters||[];
        o._$_apply_$_=f;
        var argList=stringArgs.join(",");
        var methodcall="o._$_apply_$_{"+argList+"};";
        var result=eval(methodcall);
        delete o._$_apply_$_;
        return result;
   }
}

//Firefox1.5实现新的数组方法 Array.map()
if(!Array.prototype.map){
    Array.prototype.map=function(f,thisObject){
        var results=[];
        for(var len=this.length,i=0;i<len;i++){
            result.push(f.call(thisObject,this[i],i,this));
        }    
        return results;
        }
    
    }// Array.prototype.map
}//if(!Array.prototype.map)
```

**实例方法**

一个实例方法的实现使用this关键紫来引用调用它时所给予的所有对象或实例。一个实例方法可以针对类的任何实例来调用，但是，这并不意味者每个对象都包含该方法的一份自己的私有拷贝，这就是和对实例属性一样。相反，每个实例方法由一个类的所有实例来共享。再JavaScript中，通过构造函数的原型对象中一个属性设置为一个函数之，从而定义了一个实例方法。通过这种方法，构造函数所创造的所有对象都共享一个继承的执行该函数的引用，并且可以使用前面描述的方法调用语法来调用该函数。

在java和c++中，实例方法的作用域包括this对象。

在JavaScript中这些属性显式地指定this关键字。