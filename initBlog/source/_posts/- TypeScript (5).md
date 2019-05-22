---
title: TypeScript--类
tags:
- TypeScript
---
> es6中新增了class类的概念，javaScript中没有传统的类的概念
> js中利用原型链完成继承，class只是模仿传统的继承方式，其本质仍然是使用prototype的委托机制
> 重点是要理解constructor、extends、super

## 传统js写法
```javaScript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.say = function() {
    console.log('他叫' + this.name + ',' + this.age + '岁')
}

var person = new Person('hehe', 18)
person.say()//他叫hehe,18岁
```
<!--more-->
## es6中class的写法
```javaScript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    say() {
        console.log('他叫' + this.name + ',' + this.age + '岁')
    }
}

var person = new Person('hehe', 18)
person.say()//他叫hehe,18岁
```
### class写法中需要注意的有一下几点：
1. 定义类方法前面没有function关键字
2. 方法之间不能加“，”逗号
3. 类没有提升行为
4. 类内部定义方法不可枚举

### 函数中存在函数提升，class类中没有
```javaScript
foo();
function foo(){
    console.log("hi")
}
```
如上，在声明函数之前调用函数，这样是没毛病的；

但是，如果像下面这样就会报错
```javaScript
var person = new Person();
class Person{
    console.log("hi")
}
```
因为class类还没有声明；
### class类内部定义的方法是不可枚举的
```javaScript
for(var prop in person){
    console.log(prop);
}
```
如上，传统原型链会打印name、age、say，但是使用class方法只打印了name、age
## 类表达式
上面的代码中表达类，使用的方式是声明式，接下来使用表达式；
```javaScript
var test = class {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    say() {
        console.log('他叫' + this.name + ',' + this.age + '岁')
    }
}
var person = new test('hehe', 18);
person.say();//他叫hehe,18岁
```
### 类继承
类之间的继承可以通过extends关键字实现

先定义一个父类
```javaScript
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    toString() {
        return this.x + '+' + this.y + '=' + '什么呢？'
    }
}
let qs = new Point(1, 2);
console.log(qs.toString())//1+2=什么呢？
```
再定义一个子类继承父类
```javaScript
class as extends Point {
    constructor(x, y, z) {
        super(x, y);
        this.z = z;
    }
    toString() {
        return this.x + '+' + this.y + '=' + this.z;
    }
}

let theAs = new as(1, 2, 3);
console.log(theAs.toString()); //1+2=3
```
这个子类的构造函数中出现了一个新的关键字super，如果没有super，就无法继承父类的实例属性；

注意：子类中如果有**constructor**，内部就要有**super**，子类中没有自己的this对象，需要继承弗雷的this对象再添加东西；

super指代的是父类的实例，即父类的this对象，上面的super(x,y)就是调用父类的构造函数，super.toString()就是调用父类toString()方法；

### es5和es6的继承机制是不同的
es5的继承是先创造子类的实例对象this，然后将父类的方法添加到this上面；

es6的继承是先创造父类的实例对象this，在此必须先调用super，然后再用子类的构造函数修改this
### 静态方法
类相当于实例中的原型，所有类中定义的方法都会被实例继承，但是，如果在类方法前加上static，就不会被实例继承，而是直接通过类来调用；
```javaScript
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }
    toString() {
        return this.x + '+' + this.y + '=' + '什么呢？'
    }
    static say() {
        return 666
    }
}
let qs = new Point(1, 2);

class as extends Point {
    constructor(x, y, z) {
        super(x, y);
        this.z = z;
    }
    toString() {
        return this.x + '+' + this.y + '=' + this.z;
    }
    static hi() {
        return super.say()
    }
}

let theAs = new as(1, 2, 3);
console.log(Point.say())//666
console.log(as.hi())//666
console.log(thsAs.hi())//error
```
如上，可以看出静态方法也可以从super调用，且子类调用父类的static方法也只能在静态函数中调用
### TypeScript中的class
TypeScript中的类与es6中的类几乎是一样的，主要的区别就是类成员的访问修饰符；
```javaScript
class Animal{
    private name: string;
    constructor(theName: string){
        this.name = theName;
    }
}
```
## TypeScript中的修饰符
### public访问修饰符
在ts中，成员都默认为public，使用public修饰的成员，可以在类的内外自由访问；
### private修饰符
当成员被标记为private时，就不能在声明它的类的外部访问；
```javaScript
class Point {
    private x: string;
    public constructor(x: string) {
        this.x = x;
    }
    public res(z: number) {
        console.log(this.x,z)
    }
}
let point = new Point("1")
point.x
```
如上，编译时会报错，因为x是私有类型；
### protected修饰符
protected与private行为相似，但是protected成员在派生类中可以访问；
```javaScript
class Point {
    private x: string;
    public constructor(x: string) {
        this.x = x;
    }
    public res(z: number) {
        console.log(z)
    }
}
class newPoint extends Point{
    private y: number;
    constructor(x: string,y: number){
        super(x);
        this.y = y;
    }
    say(){
        console.log(this.x,this.y)
    }
}

let pointB = new newPoint("一",1)
pointB.say();//error
pointB.x//error
```
如上，在使用private的情况下，其派生类实例化后不能调用x；
而如果改成protected，则可以在派生类中调用x，但是仍然不能在外部调用x；
```javaScript
class Point {
    protected x: string;
    public constructor(x: string) {
        this.x = x;
    }
    public res(z: number) {
        console.log(z)
    }
}
class newPoint extends Point{
    private y: number;
    constructor(x: string,y: number){
        super(x);
        this.y = y;
    }
    say(){
        console.log(this.x,this.y)
    }
}

let pointB = new newPoint("一",1)
pointB.say();//一 1
pointB.x//error
```
### readonly修饰符
使用readonly关键字将属性设置为只读的，只读属性必须在声明时或构造函数里被初始化；
```javaScript
class Point {
    readonly x: string;
    public constructor(x: string) {
        this.x = x;
    }
    public res(z: number) {
        console.log(this.x,z)
    }
}
let point = new Point("hi")
point.res(2012)//hi 2012
point.x = "hello";//error
```
如上，由于x是只读的，所以在非声明或者构造函数里给它赋值就会报错;
### gets/set存取器
主要目的就是限制外部环境对当前对象成员的访问，也就是说外部环境需要满足一定的条件才能读取当前对象里面各个成员的信息；
```javaScript
let passcode="12345";
class Antzone {
    private _webName: string;
    get webName(): string {
      return this._webName;
    }
    set fullName(newName: string) {
      if (passcode && passcode == "12345") {
        this._webName = newName;
        console.log(this._webName)
      }
      else {
        console.log("输入的密码错误");
      }
    }
}
 
let antzone = new Antzone();
antzone.fullName="aaaaa";//aaaaa
```
## abstract抽象类
抽象类作为其它派生类的基类使用，一般不会直接被实例化，不同于接口，抽象类可以包含成员的实现细节；

abstract用于定义抽象类和抽象类内部的抽象方法；
```javaScript
abstract class Person{
    abstract do(): void;
    constructor(public word: string){

    }
    say(): void{
        console.log("hello")
    }
}

class A extends Person{
    constructor(){
        super("hi")
    }
    do():void{
        console.log("继承的抽象方法")
    }
}
let a = new A();
a.do();
a.say();
```
如上，**抽象类中的抽象方法不包含具体实现且必须在派生类中实现**，抽象方法**必须包含abstract关键字**并且可包含访问修饰符；do()方法是继承与父类的抽象方法，具体怎么实现由派生类去处理，而say()方法并不是抽象方法，所以和父类一样，当然也可以在派生类中作出修改；