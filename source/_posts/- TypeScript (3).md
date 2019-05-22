---
title: TypeScript--泛型
tags:
- TypeScript
---
### 泛型类型
泛型函数的目的就是为了保证传递进函数的参数类型与返回值类型完全一致；
```javaScript
function identity<T>(arg: T): T {
  return arg;
}
```
如上，给identity添加了类型变量T，使用<>包裹，它可以捕获用户传入的类型，之后再次使用T当做返回值类型;

泛型函数相较于普通函数，主要就是在函数签名声明的前面加上泛型类型参数<类型>
<!--more-->
### 接口中应用泛型
```javaScript
interface rule{
    <T>(arg:T): T;
}

function identity<T>(arg: T): T{
    return arg;
}

let test: rule = identity;
```
如上，在普通函数后面加个< T >

### 泛型类
```javaScript
class Person<T>{
    name: T;
}
let person = new Person<string>();
```
泛型类与泛型几口非常相似，在类的名称后面传递泛型类型参数；另外，泛型类指的是实例部分的类型，类的静态属性不能使用这个泛型类型；
### 泛型限定
```javaScript
interface Lengthwise {
    length: number;
}
function identity<T extends Lengthwise>(arg: T): T {
  console.log(arg.length); 
  return arg;
}
identity(5)//5是number类型，不具有length属性
identity("sayhi")//字符串具有length属性
```
上面的代码使用extends对泛型添加了约束，所以函数传递的参数也是有限定的，必须具有length属性；




