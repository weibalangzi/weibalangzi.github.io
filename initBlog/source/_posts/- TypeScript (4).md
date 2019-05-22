---
title: TypeScript--接口
tags:
- TypeScript
---

## 接口的作用
**规定数据必须具有的结构和类型**
可以使用以下方法来规定数据的类型和结构
```javaScript
function test(obj: { webName: string }) {
    console.log(obj.webName);
  }
   
  let myObj = { webName: 'hehe'};
  test(myObj);//hehe
```
如果webName不是字符串，编译成js的时候就会报错；
当然，接口显然不是这么写的，我们可以把test括号里面的内容抽离出来，使用interface声明成一个接口;
<!--more-->
```javaScript
interface personTest{
    name: string;
    age: number;
}

function person(obj:personTest){
    console.log(obj.name,obj.age)
}

let theObj = {name:"hehe",age:18}
person(theObj)
```
如上，就是将规定数据类型和结构的部分单独用interface定义成一个接口，替换掉原来的位置；

如果实际对象结构中多了一个参数：
```javaScript
interface personTest{
    name: string;
    age: number;
}

function person(obj:personTest){
    console.log(obj.name,obj.age)
}

let theObj = {name:"hehe",age:18,gender:"boy"}
person(theObj)
```
只要person里面没有调用gender，就不会报错；可是，如果在函数调用时声明对象，则会报错； 
```javaScript
person({name:"hehe",age:18,gender:"boy"})
```
如果对象结构中少了一个参数，也会报错
```javaScript
interface personTest{
    name: string;
    age: number;
}

function person(obj:personTest){
    console.log(obj.name,obj.age)
}

let theObj = {name:"hehe"}
person(theObj)//error
```
### 可选属性
就像可选参数一样在属性名后面添加一个问号就行了，在匹配数据结构和类型的时候，该属性可有可无；
```javaScript
interface Itest {
  webName: string;
  age?:number;
}
```
如果一旦添加了该属性，则需要遵守该属性的数据类型，也就是说上面的age只能是number类型；
### 只读属性
如果属性只能在其创建的时候修改值，那么可以将其设置为只读；
```javaScript
interface Itest {
    readonly name: string;
    readonly age:number;
  }
   
  let data:Itest={
    name:"hehe",
    age:5
  }
console.log(data)//{name:"hehe",age:5}

data.age = 18;//error
```
如上，在外部修改属性值的时候会报错，因为age是只读;

#### 也可以将数组设置为只读
```javaScript
let ro: ReadonlyArray<number>=[1,2,3,5];
```
如果数组设置了只读，那么数组相关的方法就不能使用了，比如ro[1]之类的；
### 函数类型
接口除了可以规定普通对象的类型，也可以规定函数的类型；
```javaScript
interface Ifunc {
  (str: string): boolean;
}
let func:Ifunc=function(antString:string){
  return true;
}
```
如上，规定函数接受一个字符串参数，并且返回值是布尔型，且函数的参数名不需要与接口里定义的名字相匹配，只需要类型相同即可;
### 可索引的类型
即规定索引值类型，要么是字符串，要么是数字；
```javaScript
interface IArray {
  [index: number]: string;
}
 
let arr: IArray= ["hi", "hello"];
let ant: string = arr[0];
```
共有两种索引签名：字符串和数字；可以同时使用两种类型的索引，但是**数字索引的返回值必然是字符串索引返回值类型的子类型**，因为当使用number来索引时，javaScript会将它转换成string然后再去索引对象；
### 类实现接口
强制一个类符合某种规定
```javaScript
interface Itest {
    webName: string;
}

class say implements Itest {
    webName: "sayhi";
    age:4;
    constructor() { }
}
```
如上，实现类接口的关键字是implements
### 继承接口
通过继承，可以将一个接口成员赋值到另一个接口，即一个接口继承另一个接口的规则；
```javaScript
interface A {
    name: string;
}

interface B extends A {
    age: number;
    address:string;
}
```
如上，B接口继承了A接口中对于name的类型规定，同时自身又对age和address作出规定；
### 混合类型接口
```javaScript
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}

var c: Counter;
c(10);
c.reset();
c.interval = 5.0;
```
声明一个接口，如果只有(start:number):string一个成员，那么这个几口就是函数接口，如果同时还具有其他两个成员，可以用来描述对象的属性和方法，这样就构成一个混合接口；
### 接口继承类
TypeScript接口可以继承类的成员，包括private和protected成员，但不包括其实现；
```javaScript
class ClassA {
  private privateA: any;
}
 
interface Itest extends ClassA {
  test(): void;
}
```
如上，Itest包含了类classA的所有成员，并且只有类classA的子类可以实现Itest接口，即只有classAz子类可以具体的去使用这些成员；