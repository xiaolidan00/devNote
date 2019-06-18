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
factorial(5);//5*4*3*2*1=120
distance(0,0,2,2);//2倍根号2
distance(0,0,2);//NaN 因为y2为undefined
```

