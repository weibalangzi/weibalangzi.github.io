---
title: NodeJs--Stream（流）
tags:
- nodejs
- stream
---
<p style="padding:10px;background:#43853d;color:white;font-weight:900;border-radius:3px;">Stream(流)</p>
Stream是一个抽象接口，Node中有很多对象实现了这个接口，例如，对http服务器发起请求的request对象就是一个Stream；

**Stream有四种流类型：**

 - Readable：可读操作；
 - Writable：可写操作；
 - Duplex：可读可写操作；
 - Transfrom：操作被写入数据，然后读出结果；

**所有的Stream对象都是EventEmitter的实例，常用事件有：**

 - data：当有数据可读时触发；
 - end：没有更多的数据可读时触发；
 - error：在接收和写入过程中发生错误时触发；
 - finish：所有数据已经被写入到底层系统时触发；

<!--more-->
 <p style="padding:10px;background:#43853d;color:white;font-weight:900;border-radius:3px;">Stream读写操作</p>

**写入流**
```javaScript
//定义将要写入的数据
let data = 'helloKitty';

//创建一个可以写入的流，写入到文件test.txt中
let writerStream = fs.createWriteStream('test.txt');

//使用urf8编码并写入数据
writerStream.write(data,'UTF8');

//标记文件末尾
writerStream.end();

//处理流事件
writerStream.on('finish',function(){
    console.log('写入完成')
})

//如果产生错误，则抛出
writerStream.on('error',function(err){
    console.log(err.stack);
})
```

**读取流**
```javaScript
let data = '';

//创建可读流
let readerStream = fs.createReadStream('test.txt');

//设置编码为utf8
readerStream.setEncoding('UTF8');

//处理流事件
readerStream.on('data',function(chunk){
    data += chunk;
})

//读取完毕后
readerStream.on('end',function(){
    console.log(data);//helloKitty
})

//如果抛出错误
readerStream.on('error',function(err){
    console.log(err.stack)
})
```
<p style="padding:10px;background:#43853d;color:white;font-weight:900;border-radius:3px;">管道流</p>
管道提供了一个输出流到输入流的机制，通常用于从一个流中获取数据并将数据传递到另外一个流中；
![](http://www.runoob.com/wp-content/uploads/2015/09/bVcla61)
如果将文件比作装水的桶，而水就是文件里内容，我们使用一根管子（pipe）连接两个桶使得水从一个桶流入到另一个桶，这样就慢慢的实现了大文件的复制过程；
```javaScript
let fs = require('fs');

//创建一个可读流
let readerStream = fs.createReadStream('test.txt');

//创建一个可写流
let writerStream = fs.createWriteStream('test1.txt');

//管道读写操作
readerStream.pipe(writerStream);

//读取写入后的文件
fs.readFile('./test1.txt', 'utf8', function(err, data){
    console.log(data);//helloKitty
});
```
<p style="padding:10px;background:#43853d;color:white;font-weight:900;border-radius:3px;">链式流</p>
链式是通过连接输出流到另一个流并创建多个流操作链的机制，链式流一般用于管道操作；

**压缩文件**
```javaScript
let fs = require('fs');

let zlib = require('zlib');

//压缩文件
fs.createReadStream('test.txt')
    .pipe(zlib.createGzip())
    .pipe(fs.createWriteStream('test.txt.gz'))
```

**解压文件**
```javaScript
let fs = require('fs');

let zlib = require('zlib');

//解压文件
fs.createReadStream('test.txt.gz')
    .pipe(zlib.createGunzip())
    .pipe(fs.createWriteStream('test.txt'))
```

