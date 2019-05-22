---
title: TypeScript--高级类型
tags:
- TypeScript
---
### keyof用法
keyof是索引类型查询操作符，假设T是一个类型，那么keyof T产生的类型是T的属性名称、字符串字面量类型构成的联合类型；注意：T是数据类型，而非数据本身；

就像typeof查询数据类型一样，keyof查询索引类型；
```javaScript
interface Map<T> {
  [key: string]: T;
}
let keys: keyof Map<number>;//string
```
<!--more-->
### 交叉类型
交叉类型可以将现有的多种类型叠加到一起成为一种类型；
```javaScript
interface T1 {
  a: boolean;
  b: string;
}
 
interface T2 {
  a: boolean;
  b: number;
}
 
type T = T1 & T2;
 
type Ta = T['a']; // boolean
type Tb = T['b']; // string & number
```
### 联合类型
联合烈性表示一个值可以是几种类型中的某一种，类型之间使用竖线"|"分隔；
```javaScript
 class A{
  say="hi";
  walk(){

  }
}

class B{
  say="hello";
  run(){

  }
}

function test():A|B{
  return new A();
}
let C = test();
console.log(C.say);
console.log(C.walk());//报错，只能访问共有成员
```
如上，如果一个值是联合类型，那么只能访问联合类型的共有成员，如果访问私有成员
### 类型保护
类型保护是一些表达式，它们会检查以确保在某一范围里的类型，要定义一个类型保护，需要定义一个函数，它的返回值是一个类型谓词；
```javaScript
class A{
  say="hi";
  walk(){}
}
class B{
  say="hello";
  run(){}
}

function test():A|B{
  return new A();
}

let C = test();

function isA(C:A|B):C is A{
  return (<A>C).walk !== undefined;
}

if(isA(C)){
  C.walk();
}else{
  C.run();
}
```
如上，C is A是一个类型谓词，格式：parameterName is Type，parameterName必须是当前函数签名里的一个参数名，如果函数返回值为true，那么也就意味着类型谓词是成立的，于是它后面作用域的类型也就被固定为Type；

类型保护还有多种方法，比如使用typeof、instanceof、switch case等；
### 字符串字面量类型
一般情况下，数据类型都是一般意义上的string、number、undefined等等，ts1.8新增了字符串字面量类型，这些类型写法和普通的字符串字面量写法完全一致；
```javaScript
type sex = "男"|"女";
class Student{
    div(sex:sex){
        if(sex == "男"){
            console.log("我是男生")
        }else{
            console.log("我是女生")
        }
    }
}
let student = new Student();
student.div("男")
```
字符串字面量类型搭配联合类型配合使用，用来将取值限定在几个字符串字面量之中；
### this类型
通过使方法返回当前对象，也就是this所指向的对象，就能够创建链式调用功能；
```javaScript
class BasicCalcuator{
  public constructor(protected value: number = 0){}

  public currentValue(): number{
    return this.value;
  }

  public add(operand: number){
    this.value += operand;
    return this;
  }

  public subtract(operand: number){
    this.value -= operand;
    return this;
  }

  public multiply(operand: number){
    this.value *= operand;
    return this;
  }

  public divide(operand: number){
    this.value /= operand;
    return this;
  }
}

const calc = BasicCalcuator;
let v = new calc(2)
    .multiply(5)
    .add(1)
    .currentValue();

console.log(v);//11 
```
通过链式方式连续不断的调用相关方法，实现方式就是各个方法返回this，也就是对象本身；

使用this的时候需要注意的就是this的指向；
### 索引类型
索引类型查询操作符keyof可以获取操作数的可访问索引字符串字面量类型；
```javaScript
interface Person{
  theName: string;
  age: number;
  address: string;
}

let type:keyof Person;
//let type:"theName" | "age" | "address"
```
如上，字符串索引就形成了字符串字面量类型，并且这些字面量类型组合成一个联合类型，而typoe的类型就是这个联合类型；
```javaScript
interface Person{
  theName: string;
  age: number;
  address: string;
}

function func<T,K extends keyof T>(obj: T,names: K[]): T[K][]{
  return names.map(name => obj[name]);
}

let person: Person = {
  theName: 'hehe',
  age: 5,
  address: '武汉'
}

let strings = func(person,['theName','age']);

let strings2 = func(person, ['theName','target']);
// Type '"target"' is not assignable to type '"address" | "theName" | "age"'.
```
如上，使用keyof可以动态获取索引类型，也就是如果接口新增一个属性，它会实时获取，于是可以很方便的检查时候传递了正确的属性名给函数；

而如果传递的属性是一个并不存咋的属性，则会报错，就像上面的target，因为本身就不存在，所以会报错；
### 映射类型
在时间应用中，可能需要将现有类型转换成可选的；
除了可以在属性名后面加上问号"?"，还可以使用Partial
```javaScript
interface Person{
  name: string;
  age: number;
  location: string;
}

type Partial<T> = {
  [P in keyof T]?: T[P];
};
type PartialPerson = Partial<Person>;
```
Partial应景在TypeScript内置，让开发人员可以创建现有类型的所有可选版本；

然而我用的时候还是报错了，可能版本太低，懒得深究;
### 类型别名
在实际应用中，有些类型名字比较长或者难以记忆，所以需要重新命名；

TypeScript可以通过type关键字给类型重命名，别名不会新建一个类型，而是创建一个新名字来引用此类型；
```javaScript
type name = string;
let theName: name="hehe"
```
ts1.6版本开始支持为泛型提供别名
```javaScript
type name=string | (()=>string)
```
如上，为一个非泛型的联合类型重命名；
```javaScript
type name<T>=T | (()=>T)
```
如上，为一个泛型相关类型重命名；
### 类型推断
在ts中，有些数据没有明确指定类型，但是可以通过类型推断给出类型；
```javaScript
let str="say hi"
```
以上代码中，str会被推断为string类型数据；

当要从几个表达式中推断类型的时候，会从这些表达式的类型来推断出一个最合适的通用类型，也就是对所有的成员都适合的；
```javaScript
let arr = [0,1,null]
```
如上，数组成员中，除了数字还有一个null，类型并不完全一致，那么就要推断出一种兼容所有数据的类型，在默认情况下null是其他所有类型的子类型，所以number类型是最佳类型；
### 可辨识联合
可以合并字符串字面量类型，联合类型，类型保护和类型别名来创建一个叫做可辨识联合类型；

#### ts基于js模式，具有3个要素：

 - 具有普通字符串字面量属性--可辨识的特征
 - 一个类名别名包含类型的联合--联合
 - 此属性上的类型保护
```javaScript
interface Square {
  kind: "square";
  size: number;
}
interface Rectangle {
  kind: "rectangle";
  width: number;
  height: number;
}
interface Circle {
  kind: "circle";
  radius: number;
}

type Shape = Square | Rectangle | Circle;

function area(shape: Shape) {
  switch (shape.kind) {
    case "square": return shape.size * shape.size;
    case "rectangle": return shape.height * shape.width;
    case "circle": return Math.PI * shape.radius ** 2;
  }
}
```
如上代码，声明三个接口，每个接口都有kink属性，但有不同的字符串字面量类型，kind属性称做可辨识的特征或标签，其它属性则特定于各个接口

之后将三个类型联合起来，组成一个联合类型，并重新命名；

最后通过类型保护，实现辨识联合类型中的每一个类型；
### Widened类型
当变量、属性、函数在推断类型的时候，Widened类型将会被使用；
一个类型的Widened形式就是将原始数据中的null或者undefined的数据类型有any类型替换；
### Iterators[ɪtə'reɪtə]遍历器
Iterators遍历器是一种接口，它为不同的数据结构提供了统一 的访问机制，如果一个数据接口具有遍历器几口看，那么就可以一次处理该数据结构的成员；

当前javaScript用来表示集合的数据结构有四种，分别是数组、对象、Set和Map，并且这四种数据结构可以互相嵌套使用，所以需要一种具有统一访问机制的遍历器接口；

```javaScript
var arr = ["say hi", 4, "bye bye", 5];
var it = arr[Symbol.iterator]();
console.log(it.next().value);//say hi
console.log(it.next().value);//4
console.log(it.next().value);//bye bye
console.log(it.next().value);//5
console.log(it.next().value);//undefined
```
如果一个结构具有Symbol.iterator属性，那么就称这个数据结构具有遍历器接口；

Symbol.iterator返回Symbol对象的iterator属性，这是一个预定义好的、类型为Symbol的特殊值；

Symbol.iterator属性指向一个方法，调用此方法返回一个遍历器对象，它是一个指针对象，默认指向数据结构的起始位置；
1、第一次调用next(),可以将指针指向数据结构第一个成员；
2、第二次调用指向第二个成员，以此类推，知道数据结构的结尾；

每一次调用next()方法都会返回一个对象，此对象包含value和done属性，value属性值是数据结构成员的值，如果遍历完成，value属性值为undefined；done属相是一个布尔值，如果为true，说明遍历完成，如果为false，说明遍历尚未完成；

所以在遍历出所有的值以后需要在后面再接上一个next()，以表示遍历完成；

es6中有四类数据结构默认具有遍历器接口：

 - 数组
 - 默写类数组
 - Map
 - Set
 但是，**Object对象默认没有遍历器接口**
### Generator[ˈdʒenəreɪtə(r)]函数
Generator函数是es6新增的一种异步变成方案；
**Generator函数不等同于Generator()，是一种新的语法结构**

Generator函数语法和普通的function函数类似，但是有三个不同点：
1、function关键字和函数名称之间有一个星号（*）。
2、函数体内可以使用yield语句。
3、函数调用后，内部代码不会立即执行，返回的是一个遍历器对象。
```javaScript
function* person() {
    yield "hehe";
    yield "boy";
    yield "18";
    yield "china";
    return "over"
}

var g = person();
console.log(g.next());//{value: "hehe", done: false}
console.log(g.next());//{value: "boy", done: false}
console.log(g.next());//{value: "18", done: false}
console.log(g.next());//{value: "china", done: false}
console.log(g.next());//{value: "over", done: true}
```
以上代码中，person是一个Generator函数，函数内部使用yield语句定义不同的状态，return也可以定义一个状态，也就是说上面的代码有五个状态，调用此函数并不会立即执行其中的代码，而返回一个遍历器对象；
 
也就是说想要访问Generator函数中的每一个状态，需要使用遍历器兑现调用next()方法；

调用next()方法hi返回一个具有value和done属性的对象；
### declare[dɪˈkleə(r)]用法
顾名思义--声明，声明什么呢？
#### 声明变量
```javaScript
declare var ant:string
```
#### 声明常量
```javaScript
declare const min:1
```
#### 声明函数
```javaScript
declare function func(str:string):string
```
#### 声明class类
```javaScript
declare class Person {
  static maxAge: number //静态变量
  static getMaxAge(): number //静态方法
 
  constructor(name: string, age: number)  //构造函数
  getName(id: number): string 
}
```
#### 声明命名空间
```javaScript
declare namespace space {
  function func(str: string): string;
  let num: number;
}
```
#### 声明模块
```javaScript
declare module "abcde" {
  export let a: number
  export function b(): number
  export namespace c{
    let cd: string
  }
```
### d.ts文件
.d.ts文件里面可以使用declare声明一些变量、模块之类的，然后引用这个.d.ts文件就能出现智能提示效果；
引入文件方式如下：
```javaScript
///<reference path="test.d.ts"/>
```
注意：使用三条斜杠的指令来引入d.ts文件；
### @types类型定义
Typescript 2.0之后，TypeScript将会默认的查看node_modules/@types 文件夹，自动从这里来获取模块的类型定义，当然需要独立安装这个类型定义。

如果在控制台输入`npm install @types/jquery -save`,就会发现项目里多了`node_modules/@types/jquery`文件夹；





