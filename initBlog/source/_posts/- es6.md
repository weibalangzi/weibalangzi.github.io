---
title: ES6--Proxy和Reflect
tags:
- es6
---

>Proxy即代理，而代理就是指当要获取某些东西或者操作某些东西需要经过某些媒介，而不是直接作用于对象本身；
### Proxy代理
Proxy对象就可以认为是一个媒介，要操作一个对象的话，就需要经过这个媒介的过滤；
<!--more-->
```javaScript
let person = {
    theName: "hehe",
    age: "18"
}

let p = new Proxy(person, {
    get: function(target, key) {
        if (key in target) {
            return target[key]
        } else {
            console.log("没有对应属性")
        }
    },
    set: function(target, key, value) {
        return target[key] = value;
    }
});

console.log(p.theName);//hehe
console.log(p.gender);//undefined
```
以上代码通过Proxy实现了对指定对象属性读取或者设置的代理操作，而我们实际操作的是Proxy对象实例，而不是被代理的对象；

Proxy对象的第一个参数是原始对象，第二个参数是代理处理器；

使用Proxy构造器，可以在对象与各种操作对象的行为之间收集有关请求操作的各种信息，并返回任何开发者想做的事；初步看来,Proxy代理与中间件有很多共同点；

Proxy代理能够让你**截取原对象的各种操作方法**，Proxy代理也能配置为任何时候停止拦截请求，有效的撤销他们所服务的对象的所有访问代理；

应用实例：
```javaScript
let person = {
    theName: "hehe",
    age: 18
}

person = new Proxy(person, {
    set(target, key, value, proxy) {
        if (typeof target.age !== 'number') {
            throw Error("error");
        }
        return Reflect.set(target, key, value, proxy);
    }
})

person.theName = "Mr.he";
person.age = 19;
console.log(person);//Proxy {theName: "Mr.he", age: 19}
```
如上，利用Proxy设置一个修改验证，当重新修改的age不是number类型的时候，就会抛出一个错误，而如果修改后的age是一个number类型，就会执行`Reflect.set`;

于是，Reflect又是什么呢？
### Reflect对象
Reflect与Proxy的作用类似，而且新增里一些方法，这些方法可以使一些操作更加规范化；

只要Proxy对象具有的代理方法，Reflect对象全部具有，以静态方法的形式存在，这些方法能够执行默认行为，无论Proxy怎样修改默认行为，总是可以通过Reflect对应的方法获取默认行为；

Proxy和Reflect配合使用还可以实现重载，Proxy用于修改某些操作的默认行为，Reflect对象的方法与Proxy对象的方法相对应，所以Proxy对象可以调用对应的Reflect方法完成默认行为；









