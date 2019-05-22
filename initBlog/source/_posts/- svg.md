---
title: SVG--基础
tags:
- svg
---
# SVG是什么
SVG(Scalable Vector Graphics)：可缩放矢量图形，它是基于可扩展标记语言（XML）（标准通用标记语言的子集），用于描述二维矢量图像的一种图形格式；
## 主要特点

 - SVG是可伸缩矢量图形；
 - SVG用来定义用于网络的基于矢量的图形；
 - SVG使用XML格式定义图形；
 - SVG图像在放大或缩小的情况下，其图形质量不会受损失；
<!--more-->
## 主要优势

 - SVG可被总舵工具读取和修改；
 - SVG与JPEG和GIF图像比起来，尺寸更小，且可压缩性更强；
 - SVG图像中的文本是可选的，同时也是可搜索的（适合做地图）；
 - SVG可以与Java技术一起运行；
## 代码结构
SVG代码其实就是标准的XML代码，代码既可以单独写入一个SVG文件，也可以直接内嵌在HTML代码中；

svg文件是以.svg为后缀的文件；

**首先要有一个XML声明**
```javaScript
<!doctype html>
```
类似于h5的DTD声明，XML也有一个声明
```javaScript
<?xml version="1.0"?>
```
**描述SVG的XML命名空间**
命名空间是通过xmlns属性规定的：
```javaScript
<svg xmlns="http://www.w3.org/2000/svg">
```
在HTML中也能找到相应的部分：
```javaScript
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
```
**创建SVG文件**
```javaScript
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" 
"http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">

<svg width="100%" height="100%" version="1.1"
xmlns="http://www.w3.org/2000/svg">

<rect width="300" height="100"
style="fill:rgb(0,0,255);stroke-width:1;
stroke:rgb(0,0,0)"/>

</svg>
```
创建的SVG文件必须以.svg后缀来保存；

第一行包含了XML声明，standalone规定svg文件是独立的还是有对外部文件的引用，standalone="no"表示SVG文档会引用一个外部文件，此处引用DTD文件；

第二行和第三行引用了外部的DTD文件，该DTD位于W3C，含有所有允许的SVG元素；

SVG代码以`<svg>`标签开始，以`</svg>`标签结束，`width`和`height`属性可设置此SVG文档的宽度和高度，`version`属性可定义所使用的SVG的版本，xmlns属性可以定义SVG的命名空间；

**嵌入HTML的SVG**
SVG不但可以单独形成一个文件，也可以直接嵌入到HTML代码中；
```html
<!doctype html>
<html>

<head>
    <meta charset="utf-8">
    <title>test</title>
</head>

<body>
    <svg xmlns="http://www.w3.org/2000/svg" version="1.1">
        <circle
                cx="100" cy="50"
                r="40"
                stroke="black"
                stroke-width="5"
                fill="green" />
      </svg>
</body>
<script src="test.js"></script>

</html>
```
以上代码直接在HTML中嵌套了SVG代码，有两点需要注意的就是：XML声明不在需要；由于对SVG支持的问题，HTML的DTD应该采用HTML5的方式；
### 嵌入SVG的方式
SVG文件可以通过以下标签嵌入HTML文档：`<embed>`、`<object`、`<iframe>`

**使用`<embed>`标签**
```javaScript
<embed src="test.svg" width="300" height="100" type="image/svg+xml" pluginspage="http://www.adobe.com/svg/viewer/install/" />
```
**使用`<object>`标签**
```javaScript
 <object data="test.svg" width="300" height="100" type="image/svg+xml" codebase="http://www.adobe.com/svg/viewer/install/" />
```
## SVG常用形状
`矩形<rect>`、`圆形<circle>`、`椭圆<ellipse>`、`直线<line>`、`折线<polyline>`、`多边形<polygon>`、`路径<path>`、
## SVG常用属性
`x,y="起点坐标"`、`r="半径"`、`stroke="描边"`、`stroke-width="边框宽度"`、`fill="填充颜色"`
## 矩形
```javaScript
<rect x="20" y="20" width="250" height="250"
style="fill:blue;stroke:pink;stroke-width:5;
opacity:0.9"/>
```
## 圆角矩形
```javaScript
<rect x="20" y="20" rx="10" ry="10" width="250"
height="100" style="fill:red;stroke:black;
stroke-width:5;opacity:0.5"/>
```
rx 和 ry 属性可使矩形产生圆角，如果是在圆形里面，rx和ry则表示圆点坐标；
## 圆形
```javaScript
<circle cx="100" cy="50" r="40" stroke="black"
stroke-width="2" fill="red"/>
```
## 椭圆
```javaScript
<ellipse cx="300" cy="150" rx="200" ry="80"
style="fill:rgb(200,100,50);
stroke:rgb(0,0,100);stroke-width:2"/>
```
cx属性定义圆点的x坐标
cy属性定义圆点的y坐标
rx属性定义水平半径
ry属性定义垂直半径
## 线条
```javaScript
<line x1="0" y1="0" x2="300" y2="300"
style="stroke:rgb(99,99,99);stroke-width:2"/>
```
## 多边形
```javaScript
<polygon points="220,100 300,210 170,250"
style="fill:#cccccc;
stroke:#000000;stroke-width:1"/>
```
points属性定义多边形每个角的x和y坐标
## 折线
```javaScript
<polyline points="0,0 0,20 20,20 20,40 40,40 40,60"
style="fill:white;stroke:red;stroke-width:2"/>
```
points属性定义多边形每个角的x和y坐标
## 路径
```javaScript
<path d="M250 150 L150 350 L350 350 Z" />
```
## 滤镜
`<filter>`标签用来定义SVG滤镜，该标签使用id属性来定义图形应用哪个滤镜；
`<filter>`标签必须嵌套在`<defs>`标签内，`<defs>`标签是definitions的缩写，它允许对诸如滤镜等特殊元素进行定义

```javaScript
<defs>
    <filter id="Gaussian_Blur">
        <feGaussianBlur in="SourceGraphic" stdDeviation="3" />
    </filter>
</defs>

<ellipse cx="200" cy="150" rx="70" ry="40"
style="fill:#ff0000;stroke:#000000;
stroke-width:2;filter:url(#Gaussian_Blur)"/>
```

 - `<filter>`标签的id属性可为滤镜定义一个唯一的名称；

 - filter:url属性用来把元素链接到滤镜，当链接滤镜id时，必须使用#字符；
 - 滤镜效果是通过`<feGaussianBlur>`标签进行定义的，fe后缀可用于所有的滤镜；
 - `<feGaussianBlur`标签的stdDeviation属性可定义模糊的程度；
 - in="SourceGraphic"这个部分定义了由整个图像创建效果；
## 线性渐变
```javaScript
<defs>
    <linearGradient id="orange_red" x1="0%" y1="0%" x2="100%" y2="0%">
        <stop offset="0%" style="stop-color:rgb(255,255,0);
        stop-opacity:1"/>
        <stop offset="100%" style="stop-color:rgb(255,0,0);
        stop-opacity:1"/>
    </linearGradient>
</defs>

<ellipse cx="200" cy="190" rx="85" ry="55"
style="fill:url(#orange_red)"/>
```

 - `<linearGradient>`标签的id属性可为渐变定义一个唯一的名称；
 - fill:url(#orange_red)属性吧ellipse元素链接到此渐变；
 - `<linearGradient>`标签的x1、x2、y1、y2属性可定义渐变的开始和结束位置；
 - 渐变的颜色范围可有两种或多种颜色组成，每种颜色通过一个`<stop>`标签来规定；offset属性用来定义渐变的开始和结束位置；
## 给SVG添加链接
```javaScript
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="100%" height="100%" version="1.1">

<a xlink:href="http://www.baidu.com" target="_blank">
<rect x="20" y="20" width="250" height="250" style="fill:blue;stroke:pink;stroke-width:5;opacity:0.9"/>
</a>

</svg>
```
## 给SVG添加事件
```javaScript
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="100%" height="100%" version="1.1">


<rect x="20" y="20" width="250" height="250" style="fill:blue;stroke:pink;stroke-width:5;opacity:0.9" onclick="say()"/>
<script>
    function say(){
        console.log(11111)
    }
</script>

</svg>
```
## 给SVG添加动画
```javaScript
<svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" width="100%" height="100%" version="1.1">
    <rect x="20" y="20" width="250" height="250" style="fill:blue;stroke:pink;stroke-width:5;opacity:0.9">
        <animate xmlns="http://www.w3.org/2000/svg" attributeType="CSS" attributeName="opacity" from="1" to="0" dur="5s" repeatCount="indefinite"/>
    </rect>
</svg>
```
  

  



