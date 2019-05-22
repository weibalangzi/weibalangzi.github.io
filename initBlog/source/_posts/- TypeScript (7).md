---
title: TypeScript--基本类型
tags:
- TypeScript
---
##布尔类型
和javascript中一样，没什么好说的，只不过可以定义变量的类型；
```javaScript
var trueOrFalse:boolean = true;
```
如上，声明一个布尔型变量trueOrFalse，并复制true；
##数值类型
```javaScript
var num:number = 11
```
##字符串类型
```javaScript
let say:string = "hi"
```
##数组类型
数组类型js区别开了，虽然都是数组，但是ts中要规定数组元素的类型；

在ts中，数组有两种声明方式，一种是只用[]，另一种是只用Array

```javaScript
let arr:string[] = ["a","b","c"]
```
或者
```javaScript
let arr:Array<string> = ["a","b","c"]
```
##元组类型
元组类型和数组类似，但是有一个很明显的区别；在**数组类型**中，**数组中的元素的类型必须一致**，要么是数组，要么是字符串，要么是其它的什么；

但是，在**元组类型**中，数组内可以包含**多个类型**的元素；
```javaScript
let sayNum:[string,number] = ["hi",11]
```
值得注意的是数组中的数据类型必须和规定的类型顺序对应起来；
```javaScript
let sayNum:[string,number] = [11,"hi"]
```
也就是说上面这种写法就会报错了
```
test.ts(8,5): error TS2322: Type '[number, string]' is not assignable to type '[string, number]'.
```
##any类型
上面的几种数据类型使用的前提是，你得先知道自己将要赋值的数据是个什么类型，但是在实际中，通常碰到的值得类型是不确定的，比如用户的输入值；

这个时候就需要使用any类型了；
```javaScript
let anyValue:any = 11;
console.log(anyValue)//11

anyValue = "hi";
console.log(anyValue)//hi

anyValue = true;
console.log(anyValue)//true
```
如上，any类型的变量可以被赋值为任意值，这与js中的Object类型类似，但是Object赋值后，并不能调用该值得类型所自带的方法；

```javaScript
let anyValue:any = 11.11111;
anyValue.toFixed(1);
console.log(anyValue.toFixed(1))//11.1
```
而如果是使用Object
```javaScript
let anyValue:Object = 11.11111;
console.log(anyValue.toFixed())
```
就会报错
```
test.ts(9,22): error TS2339: Property 'toFixed' does not exist on type 'Object'.
```
any类型数据可以赋值给任何数据类型变量（除了never类型）；
```javaScript
let anyValue: any = 8;
let str:string;
str=anyValue;
console.log(str);//8
```
如果数据是any类型，那么可以访问它的任意属性（即便是不存在的）；
```javaScript
let anyValue:any = 8;
console.log(anyValue.say);
```
any类型的对象，其任意属性值都是any类型；

any类型数据可以当做函数或者构造函数调用，可以有任意参数；
```javaScript
let anyValue: any = 111;
anyValue();
```
如果没有明确的给出数据类型，并且编译器无法推断，那么该数据会被规定为any类型；
```javaScript
let obj:{a,b}
```
##void类型
void类型表示没有任何类型；
void类型的变量只能被赋值为undefined或者null；
void类型是any类型的子类型，是null和undefined类型的父类型；
```javaScript
let nothing:void = null
```
##Null和Undefined类型
null和undefined分别是Null和Undefined的直接量，也是唯一值；
##never类型
never类型表示的是那些永不存在的值得类型；
即总是会抛出异常，或者根本就不会有返回值的函数表达式或箭头函数表达式的返回值类型；
##Symbol类型
Symbol是es6中新增的一种值类型数据，表示一种绝不重复的值；
```javaScript
let v = Symbol();
```
Symbol()函数可以接受一个字符串作为参数，用作Symbol值得描述，也可以理解为键（key）
```javaScript
let v1 = Symbol("aaaa")
let v2 = Symbol("aaaa")
console.log(v1 == v2)//false
```
如上，v1和v2是不相等的；

Symbol值不能被隐式转换为字符串类型，即不能在模板字符串中直接作为一个变量来使用；

Symbol类型的值也可以作为对象的属性名称；

##类型断言
```javaScript
let anyValue:any = "say hi";
let strLength:number = (<string>anyValue).length;
console.log(strLength);//6
```
如上，尖括号<>中的数据类型为目标类型，以上代码将any类型数据转换为string类型；