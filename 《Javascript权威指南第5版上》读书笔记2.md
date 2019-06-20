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

//可变长度的参数列表对象:Argument对象
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

//把对象属性用作参数
function arraycopy(/*array*/from,/*index*/ from_start,/*array*/to,/*index*/ to_start,/*integer*/length){
    //code 
}
function easycopy(args){    arraycopy(args.from,args.from_start||0,args.to,args.to_start||0,args.length);
}
var a=[1,2,3,4];
var b=new Array(4);
easycopy({from:a,to:b,length:4});

//参数类型
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

//作为数据的函数
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

