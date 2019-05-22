---
title: TypeScript--类型兼容
tags:
- TypeScript
---
### 函数兼容
比较两个函数是否兼容，首先要看参数是否兼容，第二个还要看返回值是否兼容；
#### 函数参数
```javaScript
let funcX = (num: number) => 0;
let funcY = (num: number, str: string) => 0;

funcY = funcX;

funcX = funcY;//error
```
如上代码，将funcX赋值给funcY是成立的，但是将funcY赋值给funcX就会报错，也就是说只能从参数少的往参数多的函数赋值，反过来则不行，因为被赋值的函数必须找到对应的参数；
<!--more-->

**参数的名称没必要是相同的，只要类型兼容即可**
```javaScript
let funcX = (ant: number) => 0;
let funxY = (num: number, str: string) => 0;
 
funxY = funcX;
```
如上，参数名并不相同，但是并没有报错;

也就是说，两个函数参数的类型没必须要完全相同，只需要源函数参数参数可以赋值给目标函数对应参数，或者目标函数参数可以赋值给源函数对应参数即可；
```javaScript
let funcX = (ant: number&boolean) => 0;
let funcY = (num: number, str: string) => 0;
  
funcY = funcX;
```
如上，源类型上有额外的可选参数不是错误，目标类型的可选参数在源类型里没有对应的参数也不是错误；

### 函数的返回值
如果目标函数的返回值类型是void，那么源函数返回值可以是任意类型；
```javaScript
let funcX:(ant: number) => void;
let funxY=(num: number) => "say hi";
funcX = funxY;
```
如果不是void类型，那么源函数的返回值需要能够赋值给目标函数返回值类型；
```javaScript
let funcX:(ant: number) => number|string;
let funxY=(num: number) => "say hi";
funcX = funxY;
```
### 类兼容
TS类和接口的兼容性非常类似，但是类分为实例部分和静态部分；
比较连个类的类型数据时，吱哟实例会员会被比较，静态成员和构造函数不会比较；
```javaScript
class Person{
  theName: string;
  constructor(address: string,age: number){}
}

class People{
  theName: string;
  constructor(age: number){}
}

let person: Person=new Person("say hi",666);
let people: People;

people = person;
```
上面的函数会赋值成功，虽然Person中有一个constructor，但是静态成员和构造函数不会被比较

如果将上面的代码稍微修改一下
```javaScript
class Person{
  theName: string;
  constructor(address: string,age: number){}
}

class People{
  theName: string;
  say: string;
  constructor(age: number){}
}

let person: Person=new Person("say hi",666);
let people: People;

people = person;
```
结果赋值失败，就和函数兼容类似，被赋值的类具有源类所不具有的属性；
### 泛型兼容
TS是结构性的类型系统，泛型的类型参数影响数据的成员；
```javaScript
interface Empty<T> {
}
let x: Empty<number>;
let y: Empty<string>={};
 
x = y;
```





