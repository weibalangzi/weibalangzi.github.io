---
title: canvas基础知识
tags:
- canvas
---
# canvas和svg的区别
Canvas是使用JavaScript程序绘图，SVG是使用XML文档描述来绘图；

相对来说，SVG更时候用来做动态交互，而且SVG绘图很容易编辑，只需要增加或移除相应的元素就可以了；

同事SVG是基于矢量的，所以无论放大或者缩小，图形的质量都不会受到影响，而Canvas是基于位图的图像，放大会失真；

所以Canvas适合像素处理，动态渲染和大数据量绘制；SVG适合静态图片展示，高保真文档查看和打印的应用场景；
<!--more-->
# Canvas基本使用
#### 创建画布
```html
<canvas id="myCanvas" width="300" height="300"></canvas>
```
如果不给`<canvas>`设置width、height属性，则默认width为300、height为150；可以使用css属性来设置宽高，但是如果宽高属性和初始比例不一致，就会出现扭曲；
# 渲染上下文（The Rending Context）
`<canvas>`会创建一个固定大小的画布，会公开一个或多个`渲染上下文`，使用渲染上下文来绘制和处理想要展示的内容；
```javaScript
var canvas = document.getElementById('myCanvas');
var ctx = canvas.getContext('2d');//获得2d上下文对象
```
# 检测支持性
```javaScript
if(canvas.getContext){
    var ctx = canvas.getContext('2d');
}else{
    console.log('浏览器不支持canvas')
}
```
# 基本模板
```html
<html>
<head>
    <title>Canvas tutorial</title>
    <style type="text/css">
        canvas {
            border: 1px solid black;
        }
    </style>
</head>
<canvas id="myCanvas" width="300" height="300"></canvas>
</body>
<script type="text/javascript">
    function draw(){
        var canvas = document.getElementById('myCanvas');
        if(!canvas.getContext) return;
        var ctx = canvas.getContext("2d");
        //开始代码

    }
    draw();
</script>
</html>
```
# 绘制矩形
```javaScript
function draw() {
    var canvas = document.getElementById('myCanvas');

    if (!canvas.getContext) return;
    var c = canvas.getContext("2d");
    c.fillStyle = "red";
    c.fillRect(10, 10, 80, 40)；//绘制一个填充的矩形
    c.strokeRect(10,90,80,40);//绘制一个矩形的边框
    c.clearRect(15,15,30,30);//清除指定的矩形区域，使该区域透明
}

draw();
```
如上，绘制一个宽为80px，长为40px的长方形；
# 坐标点
如下图所示，canvas元素默认被网格覆盖，网格中的一个单元相当于cnavas中的一个像素，栅格的起点为左上角，坐标为（0,0），所有元素的位置都相对于圆点来定位，所以图中蓝色方形左上角的坐标为激励左边x像素，距离上边y像素，坐标（x，y）；
![](https://mdn.mozillademos.org/files/224/Canvas_default_grid.png)
# 绘制路径
图形的基本元素是路径，路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合，一个路径，甚至一个子路径，都是闭合的；

步骤：

 - 创建路径的起始点 `beginPath()`
 - 调用绘制方法去绘制出路径 `moveTo(x,y)`
 - 封闭路径 `closePath()`
 - 通过描边或填充路径区域来渲染图形 `stroke()、fill()`

# 绘制圆弧
有两个方法可以绘制圆弧：

1、`arc(x,y,r,startAngle,endAngle,anticlockwise)`
以（x，y）为圆心，以r为半径，从startAngle弧度开始到endAngle弧度结束，anticlosewise是布尔值，true表示逆时针，false表示顺时针，默认是顺时针；

2、`arcTo（x1,y1,x2,y2,radius）`
根据给定的控制点和变紧画一段圆弧，最后再以直线连接两个控制点

```javaScript
    c.beginPath();
    c.arc(50, 50, 40, 0, Math.PI / 2, false);
    c.stroke();//绘制一段90度的圆弧
```
# 添加样式和颜色
`fillstyle` 设置图形的填充颜色
`strokeStyle` 设置圆形轮廓的颜色
`lineWidth` 线条宽度
`lineGap` 线条末端样式
`lineJoin` 同一path内，设定线条与线条间结合处的样式
`setLineDash` 虚线
# 绘制文本
canvas提供两种方法来渲染文本：

1、`fillText(text,x,y[, maxWidth])`
在指定的位置(x,y)填充指定的文本，绘制的最大宽度是可选的；

2、`strokeText（text,x,y[, maxWidth])`
在指定的位置（x，y）绘制文本边框，绘制的最大宽度是可选的；

```javaScript
c.font = "100px sans-serif"
c.fillText("一个文本", 10, 100);
c.strokeText("两个文本", 10, 200)
```
`font = value` 文本样式
`textAlign = value` 文本对齐
`textBaseline = value` 基线对齐选项
`direction = value` 文本方向
# 绘制文本
在canvas上可以直接绘制图片
```javaScript
var img = new Image();//创建一个<img>元素
img.src = url;//设置图片源地址
c.drawImage(img,0,0);//参数1：要绘制的img 参数2，3：绘制的img在canvas中的坐标；
```
然而，由于图片是从网络加载的，所以还需要考虑图片尚未加载的情况；
```javaScript
var img = new Image();
img.onload = function(){
    c.drawImage(img,0,0)
}
img.src = url;
```
# 状态的保存和恢复
save和restore方法是用来保存和恢复canvas状态的，都没有参数，而canvas的状态就是当前页面应用的所有样式和变形的一个快照；

`save()`
Canvas状态存储在栈中，每当save()方法被调用后，当前的状态就会被推送到栈中保存，一个绘画状态包括：

 - 当前应用的变形（移动，旋转和缩放）
 - strokeStyle,fillStyle等值
 - 当前的裁剪路径

**可以任意多次调用save方法**（类似数组的push()）
`restore()`
每一次调用restore方法，上一个保存的状态就从栈中弹出，所有设定都恢复，类似数组的pop()
```javaScript
    c.fillRect(0, 0, 150, 150);
    c.save(); //保存状态1

    c.fillStyle = 'gray';
    c.fillRect(12, 12, 120, 120);
    c.save(); //保存状态2

    c.fillStyle = 'pink';//自定义状态
    c.fillRect(30, 30, 90, 90);//自定义状态

    c.restore();//调用状态1
    c.fillRect(45, 45, 60, 60)

    c.restore();//调用状态2
    c.fillRect(60, 60, 30, 30)
```
每次save一个状态就相当于在一个数组中push一个状态，而每次restoe一个状态则相当于在该数组中pop一个状态；
# 变形
使用translate(x,y)来移动canvas的原点到指定的位置，translate方法接受两个参数，x是左右偏移量，y是上下偏移量；

**注意在做变形之前先保存状态**
```javaScript
function draw() {
    c.save();//保存原点的坐标信息

    c.translate(100, 100);//移动原点坐标
    c.strokeRect(0, 0, 100, 100);//绘制矩形边框

    c.restore();//获取之前保存的原点坐标信息
    c.fillRect(0, 0, 100, 100);//绘制矩形
}

draw();
```
如上，两个矩形设置的起点坐标一样，长宽一样，但是在画布上却并没有重叠，因为绘制第一个矩形时，原点坐标被改变了，而在绘制第二个矩形时，再次获取了之前保存的原点信息，绘制矩形的起点也就发生了改变；
# rotate
rotate(angle)旋转坐标轴，这个方法只接受一个参数：旋转的角度（angle），它是顺时针方向的，以弧度为单位的值，而旋转的中心点则是坐标原点；
![](http://o7cqr8cfk.bkt.clouddn.com/17-6-10/37980271.jpg)
```javaScript
function draw() {
    c.save();

    c.translate(200, 200);
    c.fillStyle = 'gray';
    c.rotate(90);
    c.fillRect(0, 0, 100, 100);

    c.restore();
    c.fillRect(0, 0, 50, 50);
    c.fillStyle = 'red';

}

draw();
```
如上，先保存最初的原点坐标信息，然后将坐标原点移动到（200,200）的地方，绘制矩形，再旋转90度；最后获取之前保存的原点坐标信息，再次绘制矩形；
# scale
scale(x,y)对形状，位图进行缩小或者放大；

x和y表示缩放的量化参数，它们必须是正直，值比1小的表示缩小，比1大的则表示放大，值为1的当然就没什么效果；
```javaScript
function draw() {
    c.fillStyle = 'gray';

    c.fillRect(0, 0, 100, 100);

    c.save()

    c.translate(100, 100);
    c.fillStyle = 'blue';
    c.scale(1.5, 1.5);
    c.fillRect(0, 0, 100, 100);


}

draw();
```
第二个矩形被放大了1.5倍，需要注意的是必须在定义起点和长宽之前，进行放大或者缩小的操作，即scale必须放在fillRect之前；
# 合成
所谓合成就是处理两个图形产生交集之后的情况，使用属性`globalCompositeOperation`来规定产生交集之后的效果；

## source-over(default)
`ctx.globalCompositeOperation = "source-over"`；
`source-over`是默认的效果，即新图像会覆盖在原有的图像之上；
## source-in
只显示出新图像与原来图像重叠的部分，其他区域都变成透明的（包括老图像和新图像没有重叠的地方）；
```javaScript
function draw() {
    c.fillStyle = 'gray';

    c.fillRect(0, 0, 100, 100);

    c.save()

    c.globalCompositeOperation = "source-in"
    c.translate(50, 50);
    c.fillStyle = 'blue';
    c.fillRect(0, 0, 100, 100);
}
draw();
```
![](http://o7cqr8cfk.bkt.clouddn.com/17-6-10/37411043.jpg)
## source-out
只显示新图像与老图像没有重叠的部分，老图像其余部分也不显示；
![](http://o7cqr8cfk.bkt.clouddn.com/17-6-10/41781103.jpg)
## source-stop
新图像仅仅显示与老图像重叠区域，老图像仍然可以显示；
![](http://o7cqr8cfk.bkt.clouddn.com/17-6-10/29772191.jpg)
## destination-over
新图像会在旧图像的下面
![](http://o7cqr8cfk.bkt.clouddn.com/17-6-10/23645788.jpg)
## destination-in
只显示与新图像重叠部分的旧图像，其他区域透明
![](http://o7cqr8cfk.bkt.clouddn.com/17-6-10/78166139.jpg)
## destination-out
只显示旧图像与新图像没有重叠的部分的旧图像
![](http://o7cqr8cfk.bkt.clouddn.com/17-6-10/27996302.jpg)
## destination-atop
显示新图像和重叠部分的旧图像
![](http://o7cqr8cfk.bkt.clouddn.com/17-6-10/73933570.jpg)
## lighter
新图像与旧图像都显示，但是重叠区域的颜色做加色处理
![](http://o7cqr8cfk.bkt.clouddn.com/17-6-10/65148759.jpg)
## darken
保留重叠部分最黑的像素
![](http://o7cqr8cfk.bkt.clouddn.com/17-6-10/88895274.jpg)
## lighten
保留重叠部分最亮的像素
![](http://o7cqr8cfk.bkt.clouddn.com/17-6-10/21148678.jpg)
## xor
重叠部分会变成透明
![](http://o7cqr8cfk.bkt.clouddn.com/17-6-10/44617053.jpg)
## copy
只有新图像会被保留，其余的部分被清除；
# 裁剪路径
clip()把已经创建的路径转换成裁剪路径。裁剪路径的作用是遮罩，只显示裁剪路径内的区域，裁剪路径外的区域会被隐藏；

需要注意的是clip只能遮罩在这个方法调用之后绘制的图像，如果是clip方法调用之前绘制的图像，则无法实现遮罩；
![](http://o7cqr8cfk.bkt.clouddn.com/17-6-10/53022912.jpg)
# 动画
## 动画的基本步骤
**清空canvas**
在绘制每一帧动画之前，需要清空所有，清空所有最简单的做法就是clearRect()；

**保存canvas状态**
如果在绘制的过程中会更改canvas的状态（颜色、移动了坐标原点等），又在绘制每一帧时都是原始状态的话，则最好保存下canvas的状态

**绘制动画图形**
这一步才是真正的绘制动画帧

**恢复canvas状态**
如果前面保存了canvas状态，则应该在绘制完成一帧之后恢复canvas状态；
## 控制动画
可以通过canvas的方法或者自定义的方法把图像绘制到canvas上；而为了执行动画，我们需要一些可以定时执行重绘的方法；

主要使用以下三个方法：

 - setInterval()
 - setTimeout()
 - requestAnimationFrame()







