---
title: JavaScript--浏览器对象
tags:
- js
---
### window对象
window对象表示一个浏览器窗口，该对象是一个全局对象，所有的js的元素都是在此对象环境中运行的，所有的属性和方法集合都可以直接使用，不需要使用window对象来引用；
<!--more-->
window对象的属性：
`window.innerheight`：返回浏览器客户区的高度；
`window.innerwidth` ：返回浏览器客户区的宽度；
`window.name`：设置或返回窗口的名称；

window对象的方法：
`alert()`：显示带有一段指定的文本和一个去人按钮的警告框；
`clearInterval()`：取消由setInterval()设置定时器；
`postMessage()`：允许跨窗口通信，不论两个窗口是否同源；
### screen对象
screen对象是window对象的一部分，包含着有关屏幕的信息，该对象不是全局对象，所以它所有属性和方法都需要使用screen引用；

screen对象的属性：
`screen.height`：返回显示屏幕的高度；
`screen.width`：返回显示器屏幕的宽度；
### location对象
location对象是window对象的一部分，包含当前URL的有关信息，location对象不是全局对象，所以属性和方法都需要使用location对象引用；

location对象的属性：
`location.hash`：设置或返回从（#）开始的URL（锚）；
`location.host`：设置或返回主机名和当前URL的端口号；
`location.hostname`：设置或返回当前URL的主机名；
`location.href`：设置或返回完整的URL；
`location.pathname`：设置或返回当前URL的路径部分；
`location.port`：设置或返回当前URL的端口号；
`location.protocol`：设置或返回当前URL的协议；
`location.search`：设置或返回从问号（？）开始的URL（查询部分）；

location对象的方法列表：
`location.assign()`：加载新的文档；
`location.reload()`：重新加载当前文档；
`location.replace()`：用新的文档替换当前文档；
### history对象
history是window对象的一部分，此对象中包含有用户访问过得URL，所有的方法和属性，都需要使用history引用；

返回当前页面的上一个页面：`history.go(-1)`

history对象的属性：
`history.length`：返回浏览器历史列表中的URL数量；
`history.state`：返回一个表示历史堆栈顶部的状态的值；

history对象的方法：
`history.back()`：加载history列表中的前一个URL；
`history.forward()`：加载history列表中的下一个URL；
`history.go()`：加载history列表中的某个具体页面；
`history.pushState()`：添加一个浏览器url历史访问条目；
`history.replaceState()`：替换修改当前浏览器url访问历史记录条目；
### navigator对象
navigator对象包含有当前浏览器的有关信息，所有方法和属性需要使用navigator引用；

navigator对象的属性：
`navigator.appName`：返回浏览器的名称；
`navigator.appVersion`：返回浏览器的平台和版本信息；
`navigator.cpuClass`：返回浏览器系统的cpu等级；
`navigator.platform`：返回运行浏览器的操作系统平台；
`navigator.userAgent`：返回有客户机发送服务器的user-agent头部的值；

navigator对象的方法：
`navigator.javaEnabled()`：规定浏览器是否启用Java；







