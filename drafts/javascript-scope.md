---
title: 作用域
date: 2019-05-05
---
- var、let、const 之间有什么区别？
- const 声明的对象和数组能直接修改吗？

<!-- more -->
作用域初始化于执行环境创建阶段，它描述了执行环境中变量对象（VO）的可访问范围。

## 词法作用域和动态作用域
作用域按创建时机可以分为词法作用域（Lexical Scope）和动态作用域（Dynamic Scope），两者的区别在于词法作用域关注函数在何处声明，而动态作用域关注函数在何时调用。

### 词法作用域
词法作用域（静态作用域）在词法分析阶段就已确定，Javascript 采用的正是词法作用域：
```javascript
var str = 'Hello world'
function fn1() {
  console.log(str)
}
function fn2() {
  var str = 'Hello baby'
  fn1()
}
fn2() // Hello world
```
### 动态作用域
动态作用域则是在代码运行时才确定的，Javascript 并未采用动态作用域，不过动态作用域的表现形式有点类似于 Javascript 中函数的 this 指向：
```javascript
var str = 'Hello world'
function fn() {
  var str = 'Hello baby'
  console.log(this.str)
}
fn() // Hello world
```

## 全局作用域和局部作用域
作用域按类型可以统分为全局作用域和局部作用域，其中局部作用域又可细分为函数作用域、块级作用域。
### 全局作用域
全局作用域可以在任意作用域内被访问：
```javascript
// 全局作用域
var str = 'Hello world' //全局变量
function fn() { // 局部作用域
  console.log(str) // Hello world
}
fn()
```
变量的类型同时受其声明的位置和声明方式影响：
```javascript
// 全局作用域
function fn() { // 局部作用域
  str = 'Hello world' // 隐式声明变为全局变量
}
console.log(str) // Hello world
```
### 局部作用域
局部作用域只能在其自身和子作用域内被访问。
#### 函数作用域
函数作用域不可在函数外被访问：
```javascript 
function fn() {
  var str = 'Hello world'
}
console.log(str) // undefined
```
子函数作用域也无法被父函数访问：
```javascript
function fn() {
  function hello() {
    var str = 'Hello world'
  }
  console.log(str) // undefined
}
fn()
```
#### 块级作用域
ES6 新增了 `let` 和 `const` 两个变量声明关键字，Javascript 自此有了块级作用域的概念。

使用 `let` 和 `const` 关键字声明的变量不会出现变量提升:
```javascript
// var 
console.log(str) // undefined
var str = 'Hello world'

// let
console.log(str) // Uncaught SyntaxError: Invalid or unexpected token
let str = 'Hello world'
```
使用 `let` 或 `const` 关键字声明的变量只能在其所在的代码块内被访问：
```javascript
// var
for(var i = 0;i < 10;i++) {
}
console.log(i) // 9

// let
for(let i = 0;i < 10;i++) {
}
console.log(i) // i is not defined
```

## 作用域链
<!-- 自由变量 -->

作用域链描述的是父子作用域间的层级关系：

<Picture name="2.png"></Picture>

作用域的查找遵循就近原则，Javascript 引擎会先在本作用域内查找有无某变量，本作用域内未找到便会向父作用域继续查找，若查找到全局作用域仍未找到则说明该变量未被声明。

```javascript
var str = 'Hello world'
function fn() {
  var str = 'Hello baby'
  console.log(str) // Hello baby
}
fn()
```
根据作用域的就近查找原则，上面的代码会先在 `fn` 函数作用域内查找有无 `str` 变量，而 `fn` 函数内已声明了 `str` 变量，所以上面的代码会输出 `Hello baby`。

### 作用域链延长

作用域之间的访问规则可以概括为子作用域可访问父作用域，父作用域不可访问子作用域。


> 参考文章：
- [let 和 const 命令](http://es6.ruanyifeng.com/#docs/let)
- [深入理解 JS 中声明提升、作用域（链）和 `this` 关键字](https://github.com/creeperyang/blog/issues/16)
- [JavaScript: with的用法及延长作用域链](http://fannieshi.com/156.html)

