---
title: JavaScript--基础知识
tags:
- js
---
### JavaScript的特点
 - 它是一门脚本语言
 - 它是一门轻量级的编程语言
 - js代码可直接插入HTML元素之中
 - 以浏览器作为运行环境
<!--more-->
 ### JavaScript的构成
 **javaScript是ECMAScript规范在浏览器中的具体实现**，而`ECMAScript`是一个标准，`定义了最基本的行为准则`，与特点的宿主环境无关的，它为不同的宿主环境提供核心编程能力，没有与用户交互的功能；

除了javaScript，Flash和DirectorMX的ActionScript也是它的具体实现；

所以js可以认为由一下三部分构成
`BOM（浏览器对象模型）` `DOM（文档对象模型）` `ECMAScript标准`

![](http://www.softwhy.com/data/attachment/portal/201702/26/001902ddvrm9wv93r3fvwk.gif)

### JavaScript如何放置
理论上，js脚本可以放置在页面的任何位置，比如放置在head或者body之中，甚至放置在html标签之外都可以正常的运行；

但是，为了边脚本代码找不到对象，一般放置在靠近底部body的地方，或者从外部引用js，在外部文件中定义--待页面加载完毕后执行js；

### 书写格式
JavaScript是一门弱类型语言，所以在变量声明的时候不需要指定类型，变量赋值时会自动判断类型并进行转换；

js中函数名、变量名和属性名等都是区分大小写的；

ECMAScript并没有强制要求每一行代码结尾需要分好“；”，如果没有加分好，浏览器就会把每一行代码的结尾看做是一条语句的结尾，为了保持良好的编码习惯，还是加上分号比较好；
### 注释
单行使用`//注释内容`，多行使用`/*注释内容*/`
### 变量命名规则
 - 第一个字符必须是字母、下划线(_)或美元符号（$）
 - 其它字符可以是下划线、美元符号或任何字母、数字字符
 - 变量名不能是关键字和保留字

使用var可以重复声明变量，后面的会覆盖前面的变量；另外需要注意var的变量提升；
### 数据类型
js中的数据类型分为两类，一为`值类型`，一为`引用类型`；

js为值类型提供了对应包装类型，包装类型和引用类型的差别在于对象的生存周期，在读取模式下访问值类型数据的值时，内部会自动为之创建包装类型的对象，提供了相关的方法和属性，但是操作值类型数据的语句一旦执行完毕，就会立即销毁新创建的包装类型；

值类型：boolean、number、string；
引用类型：引用类型都是Object或者子类，比如Date、Array和RegExp等；

#### String类型
字符串类型使用单引号或者双引号包裹形成；
#### Number类型
此类型表示整形和浮点型数字；
如果要判断一个值是否是住址类型，可以使用isNaN()；

NaN（not a number），表示一个本来要返回数值的操作而未返回的情况；

 - 任何NaN参与的操作返回值都是NaN
 - 任何值和NaN都不相等，甚至和其本身都不相等
#### Boolean类型
Boolean只有两个值，false和true；
#### undefined
undefined类型只有一个值，即undefined，一个变量声明但是未赋值的时候，其缺省值就是undefined；
#### Null类型
此类型和undefined类型一样也只有一个值，即null，null表示一个空对象，虽然是Null类型，但是typeof返回值却是object；
#### Object类型
Object对象是无序键值对集合，数组是有序键值对集合；

### 转义字符串
转义字符串允许字符串中包含一些无法直接键入的字符串或者具有特殊含义的字符串；

使用反斜杠（\）转义符定义一个转义字符串，比如输出一个双引号：`console.log("\"hi")//"hi`

如果需要换行：`console.log("hi \nhello \nbyebye")`
### 函数
函数是具有特定语法规则，能够完成指定任务且能重复利用的代码块；

在js中，创建函数的方式有两种

 - 函数声明方式 `function foo(){}`
 - 函数表达式方式 `var bar = function foo(){}`

ECMAScript5中规定，函数声明必须带有函数名，而函数表达式则可以省略；

`函数声明`会有提升作用，即可以在函数创建之前调用函数；

`函数表达式方式`实质是给一个变量赋值一个函数对象，变量声明也会有提升效果，但是并不会被赋值，只有代码执行阶段才会赋值；

通过return语句可以跳出函数执行，也就是return语句后面的代码不再被执行；
### 作用域
作用域用来规定变量或者函数的可访问范围，控制着变量与函数的可见性和声明周期；

js中具有两种作用域，`全局作用域`和`函数作用域`

函数中使用var声明的变量是局部变量
```javaScript
function func(){
    var theName = "hehe";
    address = "wuhan"
}
```
如上，theName就是一个局部变量，该变量只能在声明变量的函数中使用，而address则是一个全局变量，全局变量在函数外部也是可见的；
### 作用域链
作用域链式查找一个变量的时候，一层层的向上查找形成的类似于链子的轨迹；
### 递增运算符
递增运算符为其操作数增加1，返回一个数值；
递增运算符分为两种使用方式：

 - 后置使用，`x++`将会在递增前返回数值；
 - 前置使用，`++x`将会在递增后返回数值；
 
在递增前返回数值：
```javaScript
var x = 1;
console.log(x++);//1
console.log(x);//2
```

在递增后返回数值：
```javaScript
var x = 1;
console.log(++x);//2
console.log(x);//2
```
### 求余运算符（%）
```javaScript
var num = 3 % 2;
console.log(num);//1
```
如上，3除以2余1，num就是最终的余数；

关于求余需要注意的地方：

 - 如果被除数是Infinity，或者除数是0，那么结果为NaN
 - Infinity被Infinity除，结果为NaN
 - 如果除数是无穷大的数，结果为被除数
 - 如果被除数为0，结果为0

### 立即执行函数
`(function(){})()`是一个表达式，会强制其理解成函数直接量，也就是表达式方式创建函数，`(function(){})`它会返回函数对象的引用，最后使用小括号()调用此函数；

`(function(){}())`的小括号放在里里面，同样也会变成表达式，也会被理解成直接量方式；
### typeof与instanceof
`typeof`可以实现数据类型的判断，特点如下：

 - 对值类型数据能够实现准确判断，比如数字、字符串
 - 对function函数也能够实现准确判断；
 - 对数组只能返回Object，不能实现精准判断；
 - 对null返回object；
 - 对object返回object
 
如果要实现精准的判断，可以使用以下代码；
```javaScript
function getType(obj) {
    var type = Object.prototype.toString.call(obj).slice(8, -1);
    return type;
}

console.log(getType([]))//Array
```
`instanceof`返回一个布尔值，用以说明指定的对象实例是否是指定类或者构造函数的实例；

所以typeof和instanceof的区别在于`typeof`用于判断数据类型，`instanceof`用于返回当前对象是不是一个类的实例，返回true或者false；

### in运算符
`in运算符`可以判断当前指定的属性是否属于一个对象
```javaScript
var obj = { theName: "hehe", age: 18 }
console.log("theName" in obj); //true
```
### delete运算符
delete运算符可以删除对象的属性，如果删除成功则返回true，否则返回false；
```javaScript
var obj = { theName: "hehe", age: 18 }
delete obj.age;
console.log(obj);//{theName: "hehe"}
```
### void运算符
void是一个运算符，计算后面的额操作数并且返回undefined；
void运算符的返回值一定是undefined；
```javaScript
var x = 6;
console.log(void 5); //undefined
console.log(void(x == 5)); //undefined
console.log(void 2 + 3);//NaN
```