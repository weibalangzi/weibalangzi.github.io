---
title: ES6--Promise
tags:
- es6
- promise
---

 > 以往为了实现异步操作的效果，通常需要使用回调函数；
 > 但是如果存在多重回调嵌套，就会进入传说中的回调地狱；
 > Promise的出现就是为了解决回调多重嵌套的问题；

## Promise概念
Promise对象是一个新的异步操作解决方案，比原有的回调函数等方式更为合理；
Promise对象保存着指定操作的结果，然后通过为此对象提供方法进行相关操作；
Promise对象具有三种状态：Pending（等待）、Resolved（已完成）、Rejected（未完成）；
Promise对象状态的改变只有两种可能：Pending转变为Resolved或者Pending转变为Rejected；
状态一旦改变就无法再次被更改，一直保持；
<!--more-->
## Promise的基本用法
Promise()是一个构造函数，通过new可以创建一个Promise对象实例，该构造函数接受一个回调函数作为参数，回调函数的参数时两个js提供的回调函数，创建Promise对象实例的时候，Promise（）构造函数的回调函数会立即执行；

如果条件成立，就会调用resolve()函数，Promise对象的状态就会变成Resolved，如果条件不成立就会调用reject函数，状态变为Rejected状态，这两个函数的参数会被传递给then中对应的回调函数；
```javaScript
var p = new Promise(function(resolve, reject) {
    //添加异步操作
    setTimeout(function() {
        console.log('执行完毕');
        resolve('完成后的结果')
    }, 2000)
})
//执行完毕
```
如上，Promise的构造函数接收一个函数作为参数，并且在该函数中传入两个参数：resolve，reject，分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数；

在上面的代码中，存在一个异步操作，会在2秒后输出“执行完毕”，需要注意的是，上面只是new了一个对象，并没有调用该实例对象，可是里面的函数已经执行了，所以在使用Promise的时候一般是包在一个函数中，在需要的时候去运行这个函数；
```javaScript
function test() {
    let p = new Promise(function(resolve, reject) {
        setTimeout(function() {
            console.log('执行完毕');
            resolve('成功后的结果')
        }, 2000)
    })
    return p
}
test().then(function(data) {
    console.log(data)//成功后的结果
})
```
如果是多个回调嵌套，可以在后面继续添加then，在then方法中，既可以return数据，又可以return一个Promise对象，这样后面的then方法中就可以前一个then里面return的数据；
```javaScript
function test1() {
    let p = new Promise(function(resolve, reject) {
        setTimeout(function() {
            console.log('执行完毕');
            resolve('test1的结果')
        }, 2000)
    })
    return p
}

function test2() {
    let p = new Promise(function(resolve, reject) {
        setTimeout(function() {
            console.log('执行完毕');
            resolve('test2的结果')
        }, 2000)
    })
    return p
}

function test3() {
    let p = new Promise(function(resolve, reject) {
        setTimeout(function() {
            console.log('执行完毕');
            resolve('test3的结果')
        }, 2000)
    })
    return p
}

test1().then(function(data) {
        console.log(data) //test1的结果
        return test2()
    })
    .then(function(data) {
        console.log(data)//test2的结果
        return test3()
    })
    .then(function(data) {
        console.log(data)//test3的结果
        return data;
    })
```
以上代码中，我们一直使用的resolve，也就是成功后的回调，接下来再说一下失败后的回调reject；
```javaScript
function getNumber() {
    var p = new Promise(function(resolve, reject) {
        //做一些异步操作
        setTimeout(function() {
            var num = Math.ceil(Math.random() * 10); //生成1-10的随机数
            if (num <= 5) {
                resolve(num);
            } else {
                reject('数字太大了');
            }
        }, 2000);
    });
    return p;
}

getNumber()
    .then(function(data) {
        console.log(data)
    }, function(reason, data) {
        console.log(reason)
        return getNumber()
    })
    .then(function(data) {
        console.log(data)
    }, function(reason, data) {
        console.log(reason)
        return getNumber()
    })
    .then(function(data) {
        console.log(data)
    }, function(reason, data) {
        console.log(reason)
        return getNumber()
    })
```
如上，当getNumber的状态变为reject的时候，就会执行then里面的第二个参数所代表的函数，该函数的第一个参数表示reject状态所携带的值，第二个参数表示上一个回调return的结果；

## catch的用法
在执行resolve的回调时，如果抛出异常了，为了不让js卡死，可以使用catch方法；
```javaScript
function getNumber() {
    var p = new Promise(function(resolve, reject) {
        //做一些异步操作
        setTimeout(function() {
            var num = Math.ceil(Math.random() * 10); //生成1-10的随机数
            if (num <= 5) {
                resolve(num);
            } else {
                reject('数字太大了');
            }
        }, 2000);
    });
    return p;
}

getNumber()
    .then(function(data) {
        console.log(data);
        console.log(somedata); //此处的somedata未定义
    })
    .catch(function(reason) {
        console.log(reason);
    });
//5
//ReferenceError: somedata is not defined
//    at test.js:26
//    at <anonymous> 
```
如上，在resolve的回调中，somedata并没有定义，如果不容catch接收这个错误，代码运行到这里就直接在控制台报错了，而且不会往下运行了，但是如果使用catch方法接收这个错误原因，并传递到reason参数中，即便是有错误的代码也不会报错了，这与try/catch语句有相同的功能；

Promise的all方法提供了并行执行异步操作的能力，如下：
```javaScript
Promise
    .all([test1(), test2(), test3()])
    .then(function(results) {
        console.log(results);//["test1的结果", "test2的结果", "test3的结果"]
    });
```
可以看到，all接收一个数组参数，里面的值最终都算返回Promise对象；三个异步操作并行执行，等到它们都执行完毕后才会进入then里面，而all会吧所有异步操作的结果放进一个数组传给then，也就是上面的results；

## race的用法
如果将 test1的延时改为1秒，并使用race方法：
```javaScript
Promise
    .race([test1(), test2(), test3()])
    .then(function(results) {
        console.log(results + "aaa");
    });
```
可以看到，最终results的结果为test1的结果，因为test1是最先执行完毕的，也就是说是最先产生结果的，then在接收这个结果之后，就传到results里面，并打印出来了；



