# 《HTML5 Canvas核心技术 图形、动画与游戏开发》 读书笔记

[TOC]

## 第1章 基础知识

```html
 <style>
        body{
            background: #dddddd;
        }
        #theCanvas{
            margin:10px;
            padding:10px;
            background: #ffffff;
            border: thin inset #aaaaaa;
        }

        #theCanvas1{
            margin:10px;
            padding:10px;
            background: #ffffff;
            border: thin inset #aaaaaa;
            height: 300px;
            width: 600px;
        }
        </style>
    </head>
<body>
    

    <!-- 注意：设置canvas的宽度高度时不可使用px后缀 -->
    <canvas id="theCanvas" height="300" width="600"></canvas>
    <!-- 默认情况下，canvas元素大小时300*150个屏幕元素，可通过css和width，height属性改变大小 -->
    
    <canvas id="theCanvas1" ></canvas>

    <script>
        //获取canvas引用
    var canvas=document.getElementById('theCanvas');
    //在canvas对象上调用getContext('2d'),注意'2d'必须小写d
    var context=canvas.getContext('2d');
    //设置绘制样式属性
    context.font='38pt Airal';
    context.fillStyle='cornflowerblue';
    context.strokeStyle='blue';
    //绘制文本内容
    context.fillText('Hello Canvas',canvas.width/2-150,canvas.height/2+15);
    context.strokeText("Hello Canvas",canvas.width/2-150,canvas.height/2+15);
    
         //获取canvas引用
         var canvas1=document.getElementById('theCanvas1');
    //在canvas对象上调用getContext('2d'),注意'2d'必须小写d
    var context1=canvas1.getContext('2d');
    //设置绘制样式属性
    context1.font='38pt Airal';
    context1.fillStyle='cornflowerblue';
    context1.strokeStyle='blue';
    //绘制文本内容
    context1.fillText('Hello Canvas',canvas1.width/2-150,canvas1.height/2+15);
    context1.strokeText("Hello Canvas",canvas1.width/2-150,canvas1.height/2+15);
    
    </script>
    </body>
```

对比theCanvas和theCanvas1可知

- canvas元素实际上由两套尺寸，一个时元素本身的大小，还有一个时元素绘制表面的大小
- 属性width和height设置的时元素本身大小和元素绘制表面的大小
- css设置canvas大小，只修改元素本身大小，不影响绘制表面，容易出现像素伸缩的情况
-  浏览器可能会自动缩放canvas,当元素大小域canvas绘图表面不相符，浏览器会缩放后者，使之复合前者大小，可能会导致奇怪、无用的效果



**canvas元素的属性**

| 属性   | 描述   | 类型     | 取值范围   | 默认值 |
| ------ | ------------------------------------------------------------ | -------- | ----------------------------------------------------------- | ------ |
| width  | canvas元素绘图表面的宽度，在默认情况下，浏览器会将canvas元素的大小设定与绘图表面大小一致，然而，如果在CSS中覆写了元素的大小，那么浏览器会将绘图表面进行缩放，使之复合元素尺寸 | 非负整数 | 在有效范围内的任意非负整数，可添加+与空格，不能给数值添加px | 300    |
| height | canvas元素绘图表面的高度。浏览器可能将绘图表面缩放至与元素相同的尺寸。 | 非负整数 | 在有效范围内的任意非负整数，可添加+与空格，不能给数值添加px | 300    |

**canvas元素的方法**

| 属性  | 描述     |
| ----------------------------- | ------------------------------------------------------------ |
| getContext()                  | 返回与该canvas元素相关的绘图环境对象。每个canvas元素均有一个这样的环境对象，而且每个环境对象均与一个canvas元素相关联 |
| toDataURL(type,quality)       | 返回一个数据地址（data URL),你可以将它设定为img元素的src属性值。第一个参数指定了图形的类型，例如image/jpeg或image/png，如果不指定第一个参数，则默认使用image/png。第二个参数必须是0~1.0之间的double值，表示JPEG图像显示的质量 |
| toBlob(callback,type,args...) | 创建一个用于表示canvas元素图像文件的Blob，第一个参数是回调函数，浏览器会以一个指向blob的引用为参数，去调用该回调函数。第二个参数以“image/png"这样的象时来指定图像类型。如果不指定，则默认使用”image/png“，最后一个参数是介于0~1.0之间的值，表示JPEG图像的质量。将来很可能会加入其他一些用于精确调控图像属性的参数。 |

**CanvasRenderingContext2D对象所含的属性**

| 属性                     | 简介                                                         |
| ------------------------ | ------------------------------------------------------------ |
| canvas                   | 指向该绘图环境所属的canvas对象，该属性最常见的用途就是通过它来获取canvas的宽度和高度，分别调用context.canvas.width与context.canvas.height即可 |
| fillstyle                | 指定该绘图环境在后续的图形填充操作中所使用的颜色、渐变色或图案 |
| font                     | 设定在调用绘图环境对象的fillText()或strokeText()方法所使用的字形 |
| globalAlpha              | 全局透明度设定，它可以取0（完全透明）~1.0（完全不透明）之间的值，浏览器会将每个像素的alpha值与该值相乘，在绘制图像时也是如此 |
| globalCompositeOperation | 该值决定了浏览器将某个物体绘制在其他物体之上时，所采用的绘制方式。 |
| lineCap                  | 该值告诉浏览器如何绘制线段的端点，可用值：butt、round及square，默认butt |
| lineWidth                | 该值决定了在canvas之中绘制线段的屏幕像素宽度，它必须时非负、非无穷的double值，默认1.0 |
| lineJoin                 | 该值告诉浏览器两条线段相交时如何绘制焦点，可取值：bevel，round，miter，默认值miter |
| miterLimit               | 告诉浏览器如何绘制miter形式的线段焦点                        |
| shadowBlur               | 该值决定了路篮球如何延伸阴影效果。值越高，阴影效果延伸越远。该值不是指阴影的像素长度，而是代表高斯模糊方程式中的参数值，它必须时一个非负非无穷的double值，默认0 |
| shadowColor              | 该值告诉浏览器使用何种颜色绘制阴影。通常采用半透明色作为该属性的值，以便让后面的背景能显示 |
| shadowOffsetX            | 以像素为单位，指定阴影效果的水平方向偏移量                   |
| shadowOffsetY            | 以像素为单位，指定阴影效果的垂直方向偏移量                   |
| strokeStyle              | 指定了对路径进行描边时索用的绘制风格，该值可被设定为某颜色、渐变色或图案 |
| textAlign                | 决定了以fillText()或strokeText()进行描绘时，所画文本的水平对齐方式 |
| textBaseline             | 决定了以fillText()或strokeText()进行绘制时，所画文本垂直对齐方式 |

canvas提供save()和restore()用于保存以及回复当前canvas绘图环境属性

```js
function drawGrid(strokeStyle,fillStyle){
    controlContext.save();//保存context到栈
    controlContext.fillStyle=fillStyle;
    controlContext.strokeStyle=strokeStyle;
    /**draw grid*/
    controlContext.restore();//从栈中恢复context
    //save()和restore()方法可以嵌套式调用
}
```

**CanvasRenderingContext2D之中与状态操作有关的方法**

| 方法      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| save()    | 将当前canvas的状态推送到一个保存canvas状态的堆栈顶部。canvas状态包括了当前的坐标变换（transformation)信息、剪辑区域（clipping region）以及所有canvas绘图环境对象的属性，包括strokeStyle，fillStyle与globalCompositeOperation等。canvas状态并不保存当前的路径或位图，只能通过调用beginPath()来充值路径，至于位图，他是canvas本身的一个属性，并不属于绘图环境对象。注意：尽管位图是canvas对象本身的属性，而可以通过绘图环境对象来访问它（在环境对象上调用getImageData()方法） |
| restore() | 将canvas状态堆栈顶部的条目弹出，原来保存于栈顶的那组状态，在弹出之后，被设置成canvas当前状态，浏览器必须根据该值来设定canvas对应的属性，因此在调用save()与restore()之前，对canvas状态所进行的修改，其效果指挥持续至restore()方法调用之前。 |

**基本的绘制操作**

```js
.arc()
.beginPath()
.clearRect()
.fill()
.fillText()
.lineTo()
.moveTo()
.stroke()
```

**一个基本的时钟程序**

```js
var canvas=document.getElementById('canvas');
var context=canvas.getContext('2d');
var FONT_HEIFHT=15,MARGIN=35,HAND_TRUNCATION=canvas.width/25,
HOUR_HAND_TRUNCATION=canvas.width/10,
NUMBER_SPACING=20,
RADIUS=canvas.width/2-MARGIN,
HAND_RADIUS=RADIUS+NUMBER_SPACING;

function drawCircle(){
    context.beginPath();
    context.arc(canvas.width/2,canvas.height/2,RADIUS,0,Math.PI*2,true);
    context.stroke();
}

function drawNumerals() {
    var numerals=[1,2,3,4,5,6,7,8,9,10,11,12],
    angle=0,numeralWidth=0;
    numerals.forEach(function (numeral) {
        angle=Math.PI/6*(numeral-3);
        numeralWidth=context.measureText(numeral).width;
        context.fillText(numeral,
            canvas.width/2+Math.cos(angle)*(HAND_RADIUS)-numeralWidth/2,
            canvas.height/2+Math.sin(angle)*(HAND_RADIUS)+FONT_HEIFHT/3);
    });
  }

  function drawCenter(){
    context.beginPath();
    context.arc(canvas.width/2,canvas.height/2,5,0,Math.PI*2,true);
    context.stroke();
  }

  function drawHand(loc,isHour){
    var angle=(Math.PI*2)*(loc/60)-Math.PI/2,
    handRadius=isHour?RADIUS-HAND_TRUNCATION-HOUR_HAND_TRUNCATION:RADIUS-HAND_TRUNCATION;
    context.moveTo(canvas.width/2,canvas.height/2);
    context.lineTo(canvas.width/2+Math.cos(angle)*handRadius,canvas.height/2+Math.sin(angle)*handRadius);
    context.stroke();
  }

  function drawHands(){
      var date=new Date(),
      hour=date.getHours();
      hour=hour>12?hour-12:hour;
      drawHand(hour*5+(date.getMinutes()/60)*5,true,0.5);
      drawHand(date.getMinutes(),false,0.5);
      drawHand(date.getSeconds(),false,0.2);
  }

  function drawClock(){
      context.clearRect(0,0,canvas.width,canvas.height);
      drawCircle();
      drawCenter();
      drawHands();
      drawNumerals();
  }
  context.font=FONT_HEIFHT+'px Arial';
  loop=setInterval(drawClock,1000);
```





