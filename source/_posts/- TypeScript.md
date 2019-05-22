---
title: TypeScript--模块
tags:
- TypeScript
---
关于模块，基本只需要记住export导出，require导入就行了，而所谓模块就是一个独立的文件，文件与文件之间是相互封闭的；

如果洗碗一个模块能够读取当前模块的内容，就需要使用export来导出指定的变量或者函数；
### 命名空间
这里的命名空间是指声明的分类，不是具体的实体，一个命名空间内可以包含值或类型，对命名空间建立应用需要使用`import`语句，如果使用`const`、`let`会使新的实体不再具有命名空间的属性；
<!--more-->
其作用就是防止产生命名冲突；
```javaScript
namespace Validation {
    export interface StringValidator {
        isAcceptable(s: string): boolean;
    }

    const lettersRegexp = /^[A-Za-z]+$/;
    const numberRegexp = /^[0-9]+$/;

    export class LettersOnlyValidator implements StringValidator {
        isAcceptable(s: string) {
            return lettersRegexp.test(s);
        }
    }

    export class ZipCodeValidator implements StringValidator {
        isAcceptable(s: string) {
            return s.length === 5 && numberRegexp.test(s);
        }
    }
}

// Some samples to try
let strings = ["Hello", "98052", "101"];

// Validators to use
let validators: { [s: string]: Validation.StringValidator; } = {};
validators["ZIP code"] = new Validation.ZipCodeValidator();
validators["Letters only"] = new Validation.LettersOnlyValidator();

// Show whether each string passed each validator
for (let s of strings) {
    for (let name in validators) {
        console.log(`"${ s }" - ${ validators[name].isAcceptable(s) ? "matches" : "does not match" } ${ name }`);
    }
}
```
如上，将所有与验证器相关的类型都放到了一个叫做Validation的命名空间里。因为需要这些接口和类在空间之外也是可以访问的，所以需要使用export，相反的，变量lettersRegexp和numberRegexp是实现细节，不需要导出，因此在命名空间外是不能访问的；

如果在多个文件中存在同一个命名空间，那么尽管是不同的文件，但是仍然是同一个命名空间，并且在使用的时候就如同它们在一个文件中定义的一样；




