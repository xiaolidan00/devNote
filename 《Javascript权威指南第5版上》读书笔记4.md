

# 《Javascript权威指南第5版上》读书笔记4

[TOC]

## 第一部分 核心Javascript

### 第十章 模块和名字空间

```js
//避免定义全局变量，以免该变量被其他模块或者使用模块的程序员覆盖
var Class={};
Class.define=function(data){};
Class.provides=function(o,c){}

var flanagen;
if(!flanagan)flanagan={};
flanagan.Class.define=function(data){/*code*/};
flanagan.Class.provides=function(o,c){/*code*/};

//根据一个域名创建一个名字空间
var com;
if(!com)com={};
else if(typeof com!="object")
    throw new Error("com already exists and is not an object");
if(com.davidflanagan)com.davidflanagan={};
else if(typeof com.davidflanagan!="object")
     throw new Error("com.davidflanagan already exists and is not an object");
if(com.davidflanagan.Class)
 throw new Error("com.davidflanagan.Class already exists");
com.davidflanagan.Class={
    define:function(data){/*code*/},
    provides:function(o,c){/*code*/}
}

//一个复数类作为模块
if(!com||!com.davidflanagan||!com.davidflanagan.Class)
    throw new Error("com/davidflanagan/Class.js has not been loaded");
com.davidflanagan.Complex=com.davidflanagan.Class.define({
    name:"Complex",
    construct:function(x,y){this.x=x;this.y=y;},
    methods:{
        add:function(c){
            return new com.davidflanagan.Complex(this.x+c.x,this.y+c.y);
        }
    }
});
//包含图形类的一个模块
var com;
if(!com||!com.davidflanagan||!com.davidflanagan.Class)
     throw new Error("com/davidflanagan/Class.js has not been loaded");
var define=com.davidflanagan.Class.define;
if(com.davidflanagan.shapes)
    throw  new Error("com.davidflanagan.shapes namespace already exists");
com.davidflanagan.shapes={};
com.davidflanagan.shapes.Circle=define({/*code*/});
com.davidflanagan.shapes.Rectangle=define({/*code*/});
com.davidflanagan.shapes.Triangle=define({/*code*/});

//匿名函数，定义后立即被调用
(function(){
    /*code*/
})();

//使用闭包定义一个私有名字空间
var com;
if(!com)com={};
if(!com.davidflanagan)com.davidflanagan={};
com.davidflanagan.Class={};
(function(){
    function define(data){counter++;}
    function provides(o,c){/*code*/}
    var counter=0;
    function getCounter(){return counter;}
    var ns=com.davidflanagan.Class;
    ns.define=define;
    ns.provides=provides;
    ns.getCounter=getCounter;
})();

//Module.createNamespace处理名字空间的创建和错误检查
Module.createNamespace("com.davidflanagan.Class");
com.davidflanagan.Class.define=function(data){/*code*/};
com.davidflanagan.Class.provides=function(o,c){/*code*/};

//Module.require检查一个命名的模块的指定版本是否存在，如果不存在就抛出一个错误
Module.require("com.davidflanagan.Class",1.0);

//Module.importSymbols简化了将标记导入到全局名字空间或另一个指定的名字空间的任务
Module.importSymbols(Module);
importSymbols(com.davidflanagan.Complex);
var Class={};
importSymbols(com.davidflanagan.Class,Class,"define");
//Module.registerInitializationFunction()允许一个模块注册一个函数，该函数包含稍后允许的初始化代码。

//包含一个模块相关的工具的一个模块 
var Module;
if(Module&&(typeof Module!="Object"||Module.NAME))
    throw new Error("Namespace 'Module already exists");
Module={};
Module.NAME="Module";
Module.VERSION=0.1;
Module.EXPORT=["require","importSymbols"];
Module.EXPORT_OK=["createNamespace","isDefined","registerInitializationFunction","runInitializationFunctions","modules","globalNamespace"];
Module.gobalNamespace=this;
Module.modules={"Module":Module};
Module.createNamespace=function(name,version){
    if(!name)throw new Error("Module.createNamespace():name required");
    var parts=name.split('.');
    var container=Module.globalNamespace;
    for(var i=0;i<parts.length;i++){
        var part=parts[i];
        if(container[part])container[part]={};
        else if(typeof container[part]!="object"){
            var n=parts.slice(0,i).join('.');
            throw new Error(n+" already exists and is not an object");
        }
        container=container[part];
    }
    var namespace=container;
    if(namespace.NAME)throw new Error("Module "+name+" is already defined");
    namespace.VEERSION=version;
    Module.modules[name]=namespace;
    return namespace;
}
Module.isDefined=function(name){
    return name in Module.modules;
}
Module.require=function(name,version){
    if(!(name in Module.modules)){
        throw new Error("Module "+name+" is not defined");
    }
    if(!version)return;
    if(!n.VERSION||n.VERSION<version)
        throw new Error("Module "+name+" has version "+n.VERSION+" but version "+version+" or greater is required");
};

Module.importSymbols=function(from){
    if(typeof from=="String")from=Module.modules[from];
    if(!from||typeof from!="object") throw new Error("Module.importSymbols(): namespace object required");
    var to=Module.globalNamespace;
    var symbols=[];
    var firstsymbol=1;
    if(arguments.length>1&&typeof arguments[1]=='object'){
	if(arguments[1]!=null)to arguments[1];
        firstsymbol=2;
    }
    for(var a=firstsymbol;a<arguments.length;a++)
       symbols.push(arguments[a]);
        
        if(symbols.length==0){
            if(from.EXPORT){
                for(var i=0;i<from.EXPORT.length;i++){
                    var s=from.EXPORT[i];
                    to[s]=from[s];
                }
                return;
            }else if(!from.EXPORT_OK){
                for(s in from) to[s]=from[s];
                return;
            }
        }
        
        var allowed;
        if(from.EXPORT||from.EXPORT_OK){
            allowed={};
            if(from.EXPORT)
                for(var i=0;i<from.EXPORT.length;i++)allowed[from.EXPORT[i]]=true;
            if(from.EXPORT_OK)
                for(var i=0;i<from.EXPORT_OK.length;i++)
                    allowed[from.EXPORT_OK[i]]=true;
        }
        for(var i=0;i<symbols.length;i++){
            var s=symbols[i];
            if(!(s in from))
                throw new Error("Module.importSymbols():symbol "+s+" is not defined");
            if(allowed&&!(s in allowed))
                throw new Error("Module.importSymbols():symbol "+s+" is not public and cannot be imported.");
            to[s]=from[s];
        }
    };
    Module.registerInitializationFunction=function(f){
        Module._initfuncs.push(f);
        Module._registerEventHandler();
    }
    Module.runInitializationFunctions=function(){
        for(var i=0;i<Module._initfuncs.length;i++){
            try{Module._inifuncs[i]();}
            catch(e){/**/}
        }
        Module._initfuncs.length=0;
    }
    Module._initfuncs=[];
    Module._registerEventHandler=function(){
        var clientside="window" in Module.globalNamespace&&"navigator" in window;
        if(clientside){
            if(window.addEventListener){//w3c
                window.addEventListener("load",Module.runInitializationFunctions,false);
            }else if(window.attachEvent){//IE5+
                window.attachEvent("onload",Module.runInitailizationFunctions);
            }else{
                //IE4 and old browsers
                window.onload=Module.runInitailizationFunctions;
            }
        }
        Module._registerEventHandler=function(){};
    }
    

```

### 第11章 使用正则表达式

```js
//定义正则表达式
var pattern=/s$/;
var pattern=new RegExp("s$");
```



**正则表达式的直接量字符**

| 字符         | 匹配                                                |
| ------------ | --------------------------------------------------- |
| 字母数字字符 | 自身                                                |
| \o           | NULL字符(\u0000)                                    |
| \t           | 制表符(\u0009)                                      |
| \n           | 换行符(\u000A)                                      |
| \v           | 垂直制表符(\000B)                                   |
| \f           | 换页符(\u000C)                                      |
| \r           | 回车(\u000D)                                        |
| \xnn         | 由十六进制数nn指定的拉丁字符，例如\x0A等价于\n      |
| \uxxxx       | 由十六进制xxxx指定的Unicode字符，例如\u0009等价于\t |
| \cX          | 控制符^X，例如\cJ等价于换行符\n                     |

**正则表达式的字符类**

| 字符   | 匹配                                        |
| ------ | ------------------------------------------- |
| [...]  | 位于括号内的任意的字符                      |
| [^...] | 不在括号之中的任意字符                      |
| .      | 除了换行符和其他Unicode行终止符外的任意字符 |
| \w     | 任何ASCII单子字符，等价于[a-zA-Z0-9_]       |
| \W     | 任何非ASCII单字符，等价于\[^a-zA-Z0-9_]     |
| \s     | 任何Unicode空白符                           |
| \S     | 任何非Unicode空白符的字符，注意\w和\S不同   |
| \d     | 任何ASCII数字，等价于[0-9]                  |
| \D     | 除了ASCII数字外的任何字符，等价于\[^0-9]    |
| [\b]   | 退格直接量(特例)                            |

**正则表达式的重复字符**

| 字符  | 匹配                              |
| ----- | --------------------------------- |
| {n,m} | 匹配前一项至少n次，但不能超过m次  |
| {n,}  | 匹配前一项n次，或更多次           |
| {n}   | 匹配签一下恰好n次                 |
| ?     | 匹配前一项0或1次，等价于{0,1}     |
| +     | 匹配前一项1次或更多次，等价于{1,} |
| *     | 匹配前一项0次或更多次，等价于{0,} |

```  js
/\d{2,4}/  //匹配2和4位数字
/\w{3}\d?/  //匹配3个字符和一个可选的数字
/\s+java\s+/  //匹配前后一个或多个空格的“Java”
/[^""]*/  //匹配0个或多个非引号字符
```

**正则表达式的选择、分组和引用字符**

| 字符    | 含义                                                         |
| ------- | ------------------------------------------------------------ |
| \|      | 选择，匹配的是该复合左边或右边的子表达式                     |
| (...)   | 组合，将几个项目组合为一个单位，这个单元可由*、+、？和\|等符号使用，而且还可以记住和这个组合配合的字符以提供引用和使用 |
| (?:...) | 之组合，那项目组合到一个单元，但不记忆于该组匹配的字符       |
| \n      | 和第n个分组一次匹配的字符相匹配，组是括号中的子表达式（可能是嵌套的）。组好是从左到右计数的左括号数，以(?:形式分组的组不编码 |

**正则表达式的锚字符**

| 字符  | 含义                                                         |
| ----- | ------------------------------------------------------------ |
| ^     | 匹配自负床的开头，再多行检索中，匹配一行的开头               |
| $     | 匹配字符串的结尾，再多行检索中，匹配一行的结尾               |
| \b    | 匹配一个词语的边界，位于字符\w和\W之间的位置，或位于\w和字符串的开头或结尾位置(注意[\b]匹配的是退格符) |
| \B    | 匹配非词语边界的位置                                         |
| (?=p) | 正前向声明，要求接下来的字符都与模式p匹配，但是不包括匹配中的那些字符 |
| (?!p) | 反前向声明，要求接下来的字符不与模式p匹配                    |

**正则表达式的标志**

| 字符 | 含义                                                         |
| ---- | ------------------------------------------------------------ |
| i    | 执行不区分大小写的匹配                                       |
| g    | 执行一个全局匹配，即找到所有匹配，而不是再找到第一个之后就停止 |
| m    | 多行模式，^匹配一行的开头和字符串的开头，$匹配一行的结尾或字符串的结尾 |

**JavaScript不支持的Perl RegExp特性**

- s(单行模式)和x(扩展语法)标志
- \a,\e,\l,\u,\L,\U,\E,\Q,\A,\Z,\z和\G转移序列
- (?<=正后向锚和(?<!反后向锚
- (?#注释和其他(?扩展语法



**用于模式匹配的String方法**

```js
//search()返回第一个与之匹配的子串的开始字符位置，若没找到则返回-1，search()不支持全局搜索
"JavaScript".search(/script/i);//4，从0开始

//replace()检索与替换字符串，第一个参数正则表达式，第二个参数替换的字符串，若无标志g，值替换第一个与模式匹配的子串。
"Hello, Javascript".replace(/javascript/gi,"JavaScript");//"Hello, JavaScript"
var quote/"([^"]*)"/g;//把字符串中的直接引号替换为大引号
text.replace(quote,"``$1''");
//replace第二个参数可以是函数

//match()唯一的参数是正则表达式，返回一个包含匹配结果的数组，若设置的标志g，则返回所有出现的匹配项
"1 plus 2 equals 3".match(/\d+/g);//["1","2","3"]

//解析url
var url=/(\w+):\/\/(\w.|+)\/(\S*)/;
var text="Visit my blog at http://./www.example.com/~david";
var result=text.match(url);
if(result!=null){
    var fullurl=result[0];
    var protocol=result[1];
    var host=result[2];
    var path=result[3];
}

"1,2,3,4,5".split(',');//["1","2","3","4","5"]
"1,2,3,4,5".split(/\s*,\s*/);//["1","2","3","4","5"]

//RegExp对象
var zipcode=new RegExp("\\d{5}","g");
var zipcode=/\d{5}/g;

var pattern=/Java/
var text="JavaScript is more fun than Java";
var result;
while((result=pattern.exec(text))!=null){
    console.log("Matched "+result[0]+" at postion"+result.index+" next search begins at "+patter.lastIndex);
}

var pattern=/java/i;
pattern.test("JavaScript");//true

```

### 第13章 Web浏览器中的Javascript

**客户端JavaScript**

- 作为全局对象的Window对象和客户端Javascript代码的全局执行环境
- 客户端对象的层次和构成它的一部分的文档对象模型(DOM)
- 事件驱动的编程模型

Window对象是位于作用域链头部的全局对象，Javascript中的所有客户端对象都是作为其他对象的属性存取的。



