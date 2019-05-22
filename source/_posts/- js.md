---
title: JavaScript--事件
tags:
- js
---
### addEventListener()方法
`addEventListener`能够为指定的对象注册事件处理函数

但是，IE9以下浏览器不支持此方法

语法结构：`target.addEventListener(type,listener,useCapture)`

`targt`:需要注册事件处理函数的对象；
`type`:事件类型；
`listener`:事件处理函数，即事件触发后要执行的函数；
`useCapture`:布尔值，规定是捕获还是冒泡；
<!--more-->
#### 捕获事件
事件从根元素开始向目标元素进行传递，在传递过程中，如果中间有元素注册了事件处理函数，并且useCapture参数值为true，那么此事件处理函数就会执行;
#### 冒泡事件
从目标元素开始向根元素传递，在传递过程中，如果中间有元素注册了事件处理函数，并且useCapture参数值为false，那么此事件处理函数就会执行；
![](http://www.softwhy.com/data/attachment/portal/201703/02/000619fqy8t8yeh5ft6qqm.png)
### attachEvent()方法
此方法与addEventListener()类似，同样是注册事件处理函数，但是区别很大；

此方法只会被IE低版本浏览器支持;

语法结构：`target.attachEvent(type,listener)`
`target`：要注册事件处理函数的对象；
`type`：事件类型；
`listener`：事件处理函数，也就是事件触发后要执行的函数；

该方法的type值和在addEventListener()不同，例如click事件在attachEvent()中前面要加“on”，在addEventListener()中则不需要；

如果需要对IE浏览器做兼容性封装，可使用如下方法：
```javaScript
function(element,type,handler){      
  if(element.addEventListener){        
    element.addEventListener(type,handler,false);      
  } 
  else if(element.attachEvent){        
    element.attachEvent("on" + type, handler);      
  } 
  else{        
    element["on"+type]=handler;      
  }    
}
```
### animationStart事件
此事件在animation动画开始时触发
### animationend事件
此事件在animation动画结束时触发
### contextmenu事件
点击鼠标右键会触发contextmenu事件，通常使用此事件来禁用右键菜单；
```javaScript
var clickTest = document.getElementById("test")
clickTest.oncontextmenu = function(e) {
    e.preventDefault();
};
```
### DOMContentLoaded事件
当DOM文档被加载和解析完成之后DOMContentLoaded事件触发，这与load事件相似，但是DOMContentLoaded事件无需等待图片、css样式文件和子框架加载完成；

IE9以上支持此事件

#### 添加defer属性
```javaScript
<script src="JS/ts.js" defer type="text/javascript"></script>
```
此时js代码的加载是异步进行，即便是js提前加载完毕，也会等待HTML文档解析完毕再去执行；
#### 添加async属性
```javaScript
<script src="JS/ts.js" async type="text/javascript"></script>
```
此时js代码会异步加载，加载完成之后立即执行并阻塞HTML文档的解析；

如果`<script>`中添加了defer，那么DOMContentLoaded事件会在文档解析、CSSOM构建完成和js执行完毕后触发；
### hashchange事件
事件在当前URL的锚部分发生改变时触发；

可以触发此事件的方式：

 - 通过设置location.hash或location.href属性修改锚部分；
 - 使用不同书签导航到当前页面；
 - 点击链接跳转到书签锚；
### reset事件
表单中的重置按钮被点击时触发此事件；
### resize事件
当调整浏览器窗口大小时会触发此事件；
### submit事件
表单中的提交按钮被点击的时候会触发此事件；
### select事件
当文本框中的文本被选中时会触发此事件；
## 事件对象
### event.button
此事假属性可返回一个整数，指示当事件被触发时哪个鼠标按键被点击；
```javaScript
var clickTest = document.getElementById("test")
clickTest.onmousedown = function(e) {
    var e = e || event;
    console.log(e.button)
};
//0，鼠标左键被点击
//2，鼠标右键被点击
```
### event.bubbles
此事件属性返回一个布尔值，如果事件是冒泡类型，则返回true，否则返回false；
### event.offsetX和event.offsetY
这两个属性返回触发事件的位置在事件源元素的坐标系统中的坐标；
### event.clientX和event.clientY
这两个属性返回当事件被触发时鼠标指针在浏览器页面的坐标；
### event.screenX和event.screenY
返回事件发生时鼠标指针相对于屏幕的水平坐标；
### event.target
此事件属性可以返回触发事件的对象；
### event.currentTarget
此事件属性返回处理该事件的对象；
### enent.keyCode
此属性可以返回被敲击的按键生成的Unicode字符吗；
### event.preventDefault()
此方法可以取消事件的默认动作；
### stopPropagation()
此方法可以阻止事件冒泡现象；
## 操作DOM的方法
#### cloneNode()
此方法可以克隆一个节点和节点的内容；
语法结构：`var newElement=oldElement.cloneNode(bool)`
oldElement：将要被克隆的元素；
bool：一个布尔值；
#### hasChildNodes()
判断当前节点是否包含子节点
#### removeChild()
删除父元素的指定子元素
#### replaceChild()
实现节点的替换功能
语法结构：`nodeObject.replaceChild(new_node,old_node)`
#### getAttribute,hasAttribute,setAttribute，removeAttribute
操作元素指定属性的属性值
#### select()
选取文本域中的内容；
语法结构：`text.select()`
#### write()
向文档写入HTML代码或者js代码
语法结构：`document.write()`
#### add()
为指定元素的class类样式集合添加新的样式类，也就是添加class名
#### contains()
判断指定元素样式类集合上是否具有class样式类，也就是判断class是否存在
#### toggle()
切换指定class样式类集合中样式类的删除和添加；
语法结构：`classList.toggle(className)`;
例如点击的时候切换一个类名是否存在；
## 操作DOM属性
#### childNodes
返回指定元素的所有一级子节点，返回值是一个集合
#### children
children本身是不符合w3c标准的，然而所有主流浏览器都支持该属性；
此属性能够获取父节点下的所有一级子节点，子节点中不包括文本节点；
#### contentDocument
获取子窗口的document对象，例如获取当前窗口的iframe对象；
#### firstChild和lastChild
获取指定元素节点的第一个子节点或最后一个子节点
#### parentNode
返回指定节点的父节点
#### previousSibling
返回当前元素的上一个兄弟元素节点
#### nextSibling
返回当前节点的下一个同级节点；
#### innerHTML
设置或者返回指定元素开始可借宿标签之中的HTML内容；








