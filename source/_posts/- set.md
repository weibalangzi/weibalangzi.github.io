---
title: ES6--Set和WeakSet
tags:
- es6
- set
---
## set数据结构
### set对象的创建
```javaScript
let test = new Set();
let arr = [1, 2, 1, 2, 4, 5, 6, 4];
arr.map((elem) => {
    test.add(elem)
})
console.log(test); //Set(5) {1, 2, 4, 5, 6}
```
如上，可以看出，虽然用大括号包裹，但是没有键值对，所以Set更像是一个数组，而且Set里面的元素是不能重复的，会自动剔除重复元素，所以可以用来去重；
<!--more-->
Set的构造函数的参数可以是一个数组，这样去重就更方便了；
```javaScript
let arr = [1, 1, 1, 2, 4, 4, 6, 4];
let test = new Set(arr);
console.log(test);//Set(4) {1, 2, 4, 6}
```
如上，可以看到，数组中的重复元素只会被添加一次；

Set对象基友遍历器接口，所以可以使用展开运算符和for of循环；

另外，Set对象具有自己的方法和属性；

### add()  
给一个Set对象添加元素
```javaScript
test.add(7)
console.log(test);//Set(5) {1, 2, 4, 6, 7}
```
### clear()  
清空Set对象所有元素
```javaScript
test.clear();
console.log(test); //Set(0) {}
```
### delete()
删除Set对象中指定的元素
```javaScript
test.delete(4);
console.log(test); //Set(3) {1, 2, 6}
```
### forEach()
遍历Set对象中的每一个元素；
### has()
判断Set对象是否具有指定元素；
```javaScript
let arr = [1, 1, 1, 2, 4, 4, 6, 4];
let test = new Set(arr);
console.log(test.has(1));//true
```
另外还有entries()和values()，没明白有什么作用；
## WeakSet数据结构
WeakSet与Set十分类似，对象里面的元素也是不能重复的，但与Set也有不同之处，WeakSet里面的元素只能是对象；

另外，WeakSet成员的对象都是弱引用，即垃圾回收机制不考虑WeakSet对该对象的引用，如果其他对象不再引用该对象，那么垃圾回收机制会自动回收该对象所占内存，不考虑对象还存在于WeakSet之中，也就是说，无法引用WeakSet的成员，因此WeakSet是不可遍历的；

```javaScript
let weakTest = new WeakSet([
    [1, 2],
    [3, 4]
])
weakTest.add([5, 6])
console.log(weakTest); //WeakSet {Array(2), Array(2), Array(2)}
```
如上，WeakSet里面的成员只能是对象，如果单个数字或者字符串就会报错；


