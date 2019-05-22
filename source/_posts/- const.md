---
title: ES6--let和const
tags:
- es6
- let
- const
---
### let命令
es6新增let命令用来声明变量，使用let声明的变量具有以下特点：

 - 使用let声明的变量只在变量声明时所在的代码块内有效
 - 不存在变量提升现象
 - 块级作用域内，如果使用let声明变量，那么该变量就会被封闭在当前作用域内，不再受外部作用域的影响；
 - 在同一作用域内，不允许使用let重复声明同一变量

<!--more-->
 ### const命令
const命令用来声明一个常量，它的绝大多数特性和let命令是相同的，但也具有自身的一些特点；

 - cosnt命令用来生成一个常量，常量被声明后，值无法改变；
 - 常量必须在声明的同时赋值，否则会报错；
 - 如果常量是一个对象，那么常量存储的是该对象的引用地址；这个地址是不能改变的，地址所指向的对象的属性是可以改变的；
```javaScript
var obj = {
    name: "hehe",
    age: "18"
}
const person = obj;
obj.age = "19";
console.log(person.age)
```
以上代码并不会报错，因为常量中存储的对象地址并没有发生改变；

在此，必须要提一下js中存在的两种类型的数据：

 - 基本类型
 - 引用类型
 
**基本类型**说明白点就是检点的数据段，而**引用类型**是有多个值构成的对象，当我们进行复制操作时，解析器首先会分析数据是值类型还是引用类型；

js中存在六中基本类型：string、number、boolean、symbol（es6）、null、undefined；
这六种基本数据类型可以直接操作保存在变量中的实际值；
举一个栗子：
```javaScript
var a = 10;
var b = a;
b = 20;
console.log(a)
```
上面代码是一个简单的赋值操作，其过程如下：
1、首先给a赋值；
2、将a的数据拷贝一份，赋值给变量b；
3、b的值就是10了；
4、最后将b重新赋值为20，此时修改b的值并不会影响a的值；
图示如下：
![](http://www.softwhy.com/data/attachment/portal/201703/06/141800pana2snrjtj26yzn.jpg)
### 引用数据类型
在js中，引用类型数据存储在堆内存中，但是不可以直接访问堆内存空间中的位置和操作堆内存空间；

只能通过操作对象在栈内存中的引用地址，所以引用类型的数据在栈内存中保存的实际上是对象在堆内存中的引用地址，通过这个引用地址可以快速查找到保存在堆内存中的对象；

举一个栗子：
```javaScript
var obj1=new Object();
var obj2=obj1;
obj2.name="say hi";
console.log(obj1.name);
```
1、var obj1=newObject()，这是创建一个对象，是一个引用类型数据，变量obj1存储的是对象在堆内存中的地址。

2、var obj2=obj1，这个赋值操作其实是将对象在堆内存中的存储地址复制给变量obj2，也就是两个变量存储的都是指向实际对象的内存地址，指向的是同一个对象。

3、obj2.name="say hi"，为对象添加一个属性。

4、console.log(obj1.name)，输出"蚂蚁部落"，因为两个变量指向同一个对象。

图示如下：
![](http://www.softwhy.com/data/attachment/portal/201703/06/141919xeysdexa0ex1pnrs.jpg)

