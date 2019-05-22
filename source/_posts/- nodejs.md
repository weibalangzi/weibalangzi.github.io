---
title: NodeJs--路由、全局对象
tags:
- 路由
- nodejs
---
<p style="padding:10px;color:white;background:#43853d;font-weight:900">路由机制</p>
所谓路由，可以用一句话简单的概括，即`通过路由定义匹配规则，url匹配正则，匹配通过就加载对应数据和模板`

**后端路由**
浏览器在地址栏中切换不同的url时，每次都向后台服务器发出请求，服务器相应请求，在后台拼接html文件传给前端显示，java web中的jsp就是如此实现的；

常用的后台MVC模式的基本路由处理流程：

 - 浏览器输入一个url请求
 - 从请求中找到Controller和Action的值，将请求传递给Controller处理
 - Controller获取Model数据对象，并且将Model传递给View
 - View呈现页面

例如输入一个url：localhost/home/index，其中localhost是域名，对应结构{controller}/{action}/{id}

后端路由的优点在于分担了前端的压力，html和数据的拼接都是由服务器完成，缺点则是当项目十分庞大时，加大了服务器的压力，同时在浏览器端不能输入指定的url路径进行指定模块的访问，而且在当前网速较慢时，将会延迟页面的加载；

**前端路由**
随着SPA（单页面应用）的不断普及，前后端开发分离，目前项目基本都适用前端路由，在项目使用期间页面不会重新加载；

前端路由的优点在于用户体验好，和后台网速没有关系，不需要每次都从服务器全部获取，见面展现快；可以在浏览器中输入指定想要访问的url路径地址；实现了前后端的分离，方便开发；

前端留有的缺点在于对SEO不是很友好；在浏览器前进和后退时重新发送请求，没有合理缓存数据；初始加载时由于加载所有模块渲染，所以会比较慢；

**前端路由的实现方法**
`使用url的hash`，就是常用的锚点（#）操作，类似页面中点击某小图标，返回页面顶部，js通过hashChange事件来监听url的变化，IE7及以下需要轮询进行实现

`利用HTML5`的history模式，使url看起来类似普通网站，以"/"分割，没有"#"，但页面并没有跳转，不过使用这种模式需要服务器端的支持，服务器在接收到所有的请求后，都指向一个html文件，通过historyAPI，监听popState事件，用pushState和replaceState来实现；

**实现一个简单的路由**
`创建一个server.js`
```javaScript
var http = require("http");
var url = require("url");
var fs = require("fs");

function start(){
    function onRequest(request,response){
        let pathname = url.parse(request.url).pathname;
        let content 
        response.writeHead(200,{"Content-Type":"text/html;charset=utf-8"} );

        switch(pathname){
            case '/page1':
            content = fs.readFileSync("./page1.html")
            response.write(content)
            break;
            case '/page2':
            content = fs.readFileSync("./page2.html")
            response.write(content)
            break;            
            default :
            response.write('hello world')
        }
        response.end();
    }

    http.createServer(onRequest).listen(8888);
    console.log("Server has started");
}

exports.start = start;
```
`创建一个index.js`
```javaScript
let server = require("./server");

server.start();
```
<p style="padding:10px;color:white;background:#43853d;font-weight:900">全局对象</p>
在js中，全局对象是window，而在node.js中，全局对象是global，所有全局变量都是global对象的属性（出列global本身以外）；

**全局对象与全局变量**
global最根本的作用是作为全局变量的宿主，满足以下条件的变量即为全局变量：

 - 在最外层定义的变量；
 - 全局对象的属性；
 - 未定义直接赋值的变量；

当定义一个全局变量时，这个变量同事也会成为全局对象的属性，反之亦然，但是，在node.js中不可能在最外层定义变量，因为所有用户代码都是属于当前模块的，而模块本身不是最外层上下文；

**__filename**
__filename表示当前正在执行的脚本的文件名，它将输出文件所在位置的绝对路径，且和命令行参数所指定的文件名不一定相同，如果实在模块中，返回的值是模块文件的路径；

`创建一个server.js`
```javaScript
var http = require('http');

var server = http.createServer(function(req,res){
    res.writeHead(200,{"Content-Type":"text/plain"});
    res.write("hello");
    res.end()
})

server.listen(3000,()=>{
    console.log("server is running at 3000")
    console.log(__filename);//D:\hewei\reg\server.js
    console.log(__dirname);//D:\hewei\reg
})
```
**__dirname**
__dirname表示当前执行脚本所在的目录

<p style="padding:10px;color:white;background:#43853d;font-weight:900">util模块</p>
util是一个node.js核心模块，提供常用函数的集合，用于弥补核心JavaScript的功能过于精简的不足；

**util.inherits**
util.inherits的面向对象特性是基于原型的，与常见的基于类的不同，JavaScript没有提供对象继承的语言级别特性，而是通过原型复制来实现的；
