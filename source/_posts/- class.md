---
title: ES6--class类
tags:
- es6
- class
---
>在es6之前，js并没有class（类）的概念，es6新增的class类并非是一种全新的继承模型，依然是基于现有的原型继承的语法包装；

### 在es6之前我们是这样创建实例对象
```javaScript
function Person(theName, age) {
    this.theName = theName;
    this.age = age;
}

Person.prototype.say = function() {
    console.log("hi", this.theName, this.age)
}

var person = new Person("hehe", 18)
person.say();//hi hehe 18
```
<!--more-->
而如果使用es6的class类来创建实例对象，可以这样写：
```javaScript
class Person {
    constructor(theName, age) {
        this.theName = theName;
        this.age = age;
    }

    say() {
        console.log("hi", this.theName, this.age)
    }
}

let person = new Person("hehe", 18);
person.say();//hi hehe 18
```
class类Person本质上还是一个函数，`typeof Person`打印出来依然是`function`，而且`Person`的`prototype`依然是存在的，仍然可以在它的原型上添加方法；

### constructor构造器
每个类必然会有一个`constructor`构造器，通过new创建对象实例时，构造器自动执行，并返回对象实例，而如果类中没有显式定义构造器，那么会默认添加一个空的构造器；

构造器中的this是指向对象实例的，构造器默认返回对象实例，但是也可以人为自定义返回值;

`__proto__`是指向原型对象的；
```javaScript
console.log(person.__proto__);//{constructor: ƒ, say: ƒ}
```
### 创建类的方式
**（1）、类方式**
使用class关键字后跟一个类名，就可以定义一个类；
注意：`类声明不具有类名提升的效果，也就是说必须先声明，再使用`
```javaScript
class Person {
    constructor(theName, age) {
        this.theName = theName;
        this.age = age;
    }

    say() {
        console.log("hi", this.theName, this.age)
    }
}
```
**（2）、表达式方式**
类表达式是定义类的另一种方式，就像函数表达式一样，在类表达式中，类名是可有可无的；
```javaScript
var People = class {
    constructor(theName, age) {
        this.theName = theName;
        this.age = age;
    }
    say() {
        console.log("hi", this.theName, this.age)
    }
}
```
也可以添加类名
```javaScript
var People = class person{
    constructor(theName, age) {
        this.theName = theName;
        this.age = age;
    }
    say() {
        console.log("hi", this.theName, this.age)
    }
}
```
注意：`类内部天然使用严格模式，无需特别声明`
### extends用法
类之间可以通过extends实现继承；
```javaScript
class Child extends Parent{}
```
子类child可以继承Parent所有属性和方法；
```javaScript
class Parent {
    constructor(firstName, address) {
        this.firstName = firstName;
        this.address = address;
    }

    parentSay() {
        console.log("this is from parent")
    }
}

class Child extends Parent {
    constructor(firstName, address, hobby) {
        super(firstName, address);
        this.hobby = hobby;
    }

    childSay() {
        console.log("this is from child")
    }

    detail() {
        console.log(this.firstName, this.address, this.hobby)
    }
}

let person = new Child("he", 18, "music");
person.parentSay(); //this is from parent
person.childSay(); //this is from child
person.detail();//he 18 music
```
如上，使用extends实现了两个类的继承关系，在子类的构造函数中，必须先调用`super()`函数才能够使用this，因为子类中没有自己的this对象，而是继承父类的this对象，然后在父类this对象的基础上进行修改作为自己的对象使用；

在子类构造函数中`super()`函数可以认为是父类的构造函数，调用它就可以创建父类的this对象；
### static静态方法和静态属性
`实例方法`需要使用类的实例对象进行调用，但是`静态方法`是属于类的方法，直接用类就可以调用，不能使用实例对象调用；
#### 实例方法
```javaScript
class Person {
    constructor(theName, age) {
        this.theName = theName;
        this.age = age;

        this.say = () => {
            console.log("hello" + theName)
        }
    }
}

let peo = new Person("hehe", 18);

peo.say();//hellohehe
```
如上，say()方法就是实例方法，需要用对象实例调用；
#### 静态方法
```javaScript
class Person {
    constructor(theName, age) {
        this.theName = theName;
        this.age = age;
    }
}
Person.say = () => { console.log("hello") }
let peo = new Person("hehe", 18);

Person.say(); //test.js:14 hello
peo.say();//error peo.say is not a function
```
如上，在es5中就是这么定义静态方法的，也可以称之为静态属性，只不过它的属性值是一个函数；

在es6中新增了定义静态方法的方式：
```javaScript
class Person {
    constructor(theName, age) {
        this.theName = theName;
        this.age = age;
    }

    static say() {
        console.log("hi")
    }
}

let peo = new Person("hehe", 18);

Person.say(); //hi
peo.say(); //error peo.say is not a function
```
es6中新增了`static`，只要在方法前面加上`static`关键字就能将该方法定义为静态方法；并且，该静态方法不能被实例对象调用，但是可以被子类继承；
```javaScript
class Person {
    constructor(theName, age) {
        this.theName = theName;
        this.age = age;
    }

    static say() {
        console.log("hi")
    }
}

class Who extends Person {
    constructor() {

    }
}

Who.say();//hi
```
如上，子类也可以继承父类的静态方法；

使用`super`关键字也可以调用静态方法；
```javaScript
class Person {
    constructor(theName, age) {
        this.theName = theName;
        this.age = age;
    }

    static say() {
        console.log("byebye")
    }
}

class Who extends Person {
    static talk() {
        return super.say()
    }
}

Who.talk(); //byebye
```
如上，使用`super`就调用了父类的静态方法；

总结一下，在`constructor`里面的就是实例属性和实例方法，在外面的且使用`static`标记的就是静态属性和静态方法；

父类的`constructor`里面的this指向的是对象实例，子类本身是没有this的，需要在子类本身的`constructor`里面调用`super()`才能使用this，这里的super表示父类的constructor构造函数；

每个类必然会有一个`constructor`，通过new创建对象实例时，构造器自动执行，并返回实例对象，`如果类中没有显式定义构造器，那么会默认添加一个空的构造器`；
```javaScript
class Person {
    constructor(theName, age) {
        this.theName = theName;
        this.age = age;
    }

    talk() {
        console.log("hihihihi")
    }

    static say() {
        console.log("byebye")
    }
}

class Who extends Person {

}

class Are extends Who {

}

let hehe = new Are()

Who.say(); //byebye
Are.say();//byebye
hehe.talk(); //hihihihi
```
如上，虽然子类中并没有`constructor构造器`,但是仍然可以创建实例对象，并继承父类原型中的方法；

而在子类`constructor`里面使用`super`时，`super`指代的是父类的`constructor构造器`

在子类中`constructor`以外的地方调用`super`的时候，分为两种情况：一种是在原型方法里使用，此时`super`指代父类的原型对象；一种是在带有`static`标记的静态方法里面使用，此时`super`指代的是父类本身；
```javaScript
class Person {
    constructor(theName, age) {
        this.theName = theName;
        this.age = age;
    }

    test() {
        console.log("super在子类原型上方法中", this)
    }

    static test2() {
        console.log("super子类静态方法中", this)
    }
}

class Who extends Person {
    constructor(theName, age) {
        super();
        this.theName = theName;
        this.age = age;

        this.self = () => { console.log("super在子类构造器中", this) }
    }

    say() {
        super.test()
    }

    static talk() {
        super.test2()
    }

}

let hehe = new Who("tony", 18);


hehe.say(); //super子类在原型上方法中     Who {theName: "tony", age: 18, self: ƒ}
Who.talk(); //super子类静态方法中     class Who extends Person{...}
hehe.self(); //super子类在构造器中    Who {theName: "tony", age: 18, self: ƒ}
```