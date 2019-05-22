---
title: cookie
tags:
- js
- cookie
---
### cookie是什么
当用户访问某些web站点时，这些站点会在用户本地的浏览器的本地硬盘上创建一个很小的文本文件用以存储一些信息，这个文件称之为cookie文件；

cookie可以使用js操作，也可以使用其它语言操作；

每一个cookie都是名值对的组合，如果有多个名值对，可以使用分好将他们分割，使用document.cookie属性设置或者后去与当前文档相关的所用cookie；
```javaScript
document.cookie = "userName=hehe";
document.cookie = "pw=123123";
```
<!--more-->
使用document.cookie可以获取同一域名下所有的cookie；
### 写入cookie封装
```javaScript
function SetCookie(name, value) {
    var Days = 30; //此cookie将被保存30天
    var exp = new Date(); //创建时间日期对象；
    exp.setTime(exp.getTime() + Days * 24 * 60 * 60 * 1000);
    document.cookie = name + "=" + escape(value) + ";expires=" + exp.toGMTString();
}

SetCookie("hehe", "19")
```
expires是用来设置对应cookie的过期时间；
escape()用来对特殊字符进行编码；
### 读取cooki封装
```javaScript
//取cookies函数        
function getCookie(name){
  var arr = document.cookie.match(new RegExp("(^| )" + name + "=([^;]*)(;|$)"));
  if (arr != null) return unescape(arr[2]); return null;
}
```
### 删除cookie
默认情况下，当关闭浏览器结束会话时，cookie就会被删除，此时cookie仅仅是驻留在内存中，而没有被写入硬盘，所以可是使用expires设置cookie的过期时间；

删除指定的cookie只需要将该cookie的过期时间修改一下即可；
### cookie路径
当创建一个cookie的时候，这个cookie可能会在多个页面中共享，也就说并非只有创建cookie的页面可以访问和操作cookie；

默认请款下，只有和创建cookie的页面在同一个目录或者创建cookie所在的目录的子目录的下的网页才可以访问cookie；

在setCookie.html创建cookie，那么cookie/这个路径下所有页面都可以访问此cookie，其他页面无法访问；

cookie的有效路径也可以进行设置：`document.cookie = "name=value;path=path"`

如果将path设置为根目录，那么整个站点的页面都可以访问cookie：
```javaScript
document.cookie = "userName=hehe;path=/"
```
### cookie域
默认状态下，cookie是不能进行跨域访问的，可以进行人为设置以实现跨域访问；
语法格式：`document.cookie="name=value;path=path;domain=domain"`

domain就是可以访问cookie的其他的域



