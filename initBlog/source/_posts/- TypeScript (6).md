---
title: TypeScript--函数
tags:
- TypeScript
---

## 函数类型
相比于js里面的函数，ts里面的函数在创建时，要声明**参数的类型**，并且规定**返回值的类型**；

### 以函数声明的方式创建函数
```javaScript
function person(gender: string,age: number): string{
    return gender+age+"岁"
}
console.log(person("男",18))//男18岁
```
<!--more-->
### 以表达式的方式创建函数
```javaScript
let person = function(gender: string,age: number): string{
    return gender+age+"岁"
}
console.log(person("男",18))//男18岁
```
### 显示的给出所有数据类型，完整写法如下
```javaScript
let person:(gender: string,age: number)=>string=
    function(gender: string,age: number): string{
        return gender+age+"岁"
    }
console.log(person("男",18))//男18岁
```
ts中函数的类型必须是完整的，即便函数没有返回值，也要显式的规定void
## 可选参数和默认参数
ts中关于函数传参的规定：给一个函数的参数个数必须与函数期望的参数个数一致；

同时，ts也给出了可选参数和默认参数；
```javaScript
function Person(name: string,age?: number): string{
    return name+age+"岁"
}

console.log(Person("hehe"))//heheundefined岁
```
如果不添加问号，则在编译成js时会报错；
由此可知，**在参数名称后面加一个问号(?)可以规定此参数为可选参数，那么此参数可以省略**；
## 默认参数
可以为参数提供一个默认值，当没有传递参数值或传递的值是undefined时，就使用默认参数值；
```javaScript
function Person(name: string,age = 18): string{
    return name+age+"岁"
}

console.log(Person("hehe"))//hehe18岁
```
默认参数就是直接把参数类型替换成默认值就行了，注意是赋值，所以要使用等号=；
## 箭头函数
箭头函数就没什么好说的了，只要记住箭头函数可以捕获它声明时所在的上下文中的this的指向；
## 函数重载
函数重载就是指同一个函数，根据传递的参数不同，会有不同的表现形式；
```javaScript
function Person(x: any): any{
    if(typeof x == "number"){
        return x+1
    }else if(typeof x == "string"){
        return x+"hi"
    }
}

console.log(Person("hehe"))//hehehi
console.log(Person(1))//2
```