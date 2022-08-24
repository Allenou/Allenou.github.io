---
title: 严格模式
---
# 严格模式
-  use strict 有什么作用？使用它有什么优缺点？（2017-02-21）

`use strict` 是应用“严格模式”（strict mode）的编译指示，该指示所在的作用域会使用严格模式。使用严格模式后禁止以下行为：
<!-- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Strict_mode、 -->
```javascript
// 使用未声明的变量
// 非严格模式：创建全局变量   
// 严格模式：抛出 ReferenceError
name = 'Allen'   

// 对变量使用 `delete` 操作符
// 非严格模式：静默失败
// 严格模式：抛出语法错误
var sex = 'Man'
delete sex

// 使用保留字作为变量名或函数名
// 非严格模式：创建全局变量和函数
// 严格模式：抛出语法错误
var implements
var interface

function implements(){}
function interface(){}
...

// 重名属性
// 非严格模式： 使用第二个属性
// 严格模式：抛出语法错误
var boy = {
    name: "Allen",
    name: "OuYeung"
}

// 重名参数
// 非严格模式：使用第二个参数
// 严格模式：抛出语法错误
function sayHello(name,name){
    console.log(name) // OuYeung
}

sayHello(boy.name,boy.name) 

// 修改命名参数
function showBook(name){
    name = "人生"
    console.log(arguments[0]) // 非严格模式：“人生”， 严格模式：“平凡的世界”
}

showBook("平凡的世界")
```
因为严格模式抛出了原来的静默处理的错误并且修复了以前 JS 引擎难以修复的缺陷，所以使用严格模式可以提前发现代码错误，有利于提升编码效率和代码和代码质量。
