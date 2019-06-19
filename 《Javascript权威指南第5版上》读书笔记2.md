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
```

