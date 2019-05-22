# typeScript--枚举
---
首先，枚举是一种数据类型，就像boolean、string之类的一种**数据类型**。
使用枚举可以**定义**一些**有名字**的**数字常量**，枚举类型是一种特殊的原始数据类型，里面列举了每一个带有名字的可能用到的枚举成员，每个枚举成员都是常量。
枚举类型使用**enum**进行定义
``` javascript
enum Direction{
    Up=1,
    Down,
    Left,
    Right
}
console.info(Direction);//一个对象
console.info(Direction.Down);//2
```
###console.log()与console.info()
在上面的代码中，使用到了console.info()，因为平时只是使用过console.log()，并没有使用过console.info()，所以刚看到的时候真的是一脸懵逼啊。

查了一下才知道，console后面其实可以接上很多其它的参数，具体有哪些就不多说了，直接控制台console然后回车就能看到。

关于console，常用的有一下几种：
1. console.log用于输出普通信息；
2. console.info用于输出提示性信息；
3. console.error用于输出错误信息；
4. console.warn用于输出警示信息；

这些东西了解一下就好，重点还是来说一下枚举吧！

先看以下代码：

```typeScript
enum Color {
  blue,
  red,
  yellow
}
```
以上代码表达的意思是声明一个枚举类型，该类型具有三个成员，默认情况下，从0开始为元素编号，并且依次递增1；
```javascript
enum Color {
    blue,
    red,
    yellow
}

console.log(Color.blue)//0
console.log(Color.red)//1
console.log(Color.yellow)//2
```
当然，也可以手动指定它们的值；
```
enum Color {
    blue=2,
    red=1,
    yellow=0
}

console.log(Color.blue)//2
console.log(Color.red)//1
console.log(Color.yellow)//0
```
还可以只指定部分的值
```
enum Color {
    blue=2,
    red,
    yellow=0
}

console.log(Color.blue)//2
console.log(Color.red)//3
console.log(Color.yellow)//0
```
如果是没有被指定值得成员，其值为上一个成员的值+1后的结果；

也可以由枚举值获取成员名：
```
enum Color {
    blue=2,
    red,
    yellow=0
}
let selectColor:string = Color[2];
console.log(selectColor)//blue
```
但是，值得注意的一点是，不同枚举类型之间是不允许互相赋值的；

在TypeScript2.4版本以后，枚举成员变量可以包含字符串：
```
enum Color {
    Red = "RED",
    Green,
    Blue = "BLUE",
}

console.log(Color.Red)//RED
console.log(Color.Green)//undefine
```
但是由上面的结果可以看到，如果某个没有赋值的成员，其前面一个成员的值是字符串，那么就不会出现什么+1了，字符串怎么咧，只能是undefine了；
使用已经定义的常数枚举成员的值初始化：
```
enum Color {
    blue,
    red=2,
    yellow=red
  }

console.log(Color.blue)//0
console.log(Color.yellow)//2
```
不同的枚举类型，引用则需要使用限定名；
```
enum Color {
    blue,
    red=2,
    yellow=red
  }

enum face{
    happy=Color.red
}

console.log(face.happy)//2
```
在常数枚举表达式中使用运算符：
```
enum Color {
    blue,
    red=1+2,
    yellow=1*2
}

console.log(Color.red)//3
console.log(Color.yellow)//2
```
常数枚举：
```
const enum Color {
    blue,
    red,
    yellow
  }
  let colorArray = [Color.blue, Color.red, Color.yellow]//[0,1,2]
```
常数枚举只能使用常数表达式，并且不同于常规枚举的是，它们在编译阶段就会被删除，常数枚举成员在使用的地方被内联进来，因为常数枚举不可能有计算成员；

外部枚举：
外部枚举用来描述已经存在的枚举类型的形状,所以。。。形状是什么鬼？？？
```
declare enum Enum {
    A = 1,
    B,
    C = 2
}
console.log(Enum.B)
```
查了一下资料，说是外部枚举的作用就是为了避免重复的问题，即，如果其他的文件中定义有相同的枚举对象，为了避免相同的枚举对象里定义相同枚举名称带来的冲突，这样定义了以后，重复的名称就不能再使用了

总的来说，一个枚举类型可以包含零个或者多个枚举成员；
枚举成员具有一个数字值，可以是常数，或者是计算得出的值；