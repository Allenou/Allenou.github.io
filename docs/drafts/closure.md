---
title: 闭包
date: 2019-02-19
---
# 闭包
- 什么是闭包？（2017-02-24）
## 作用域
Javascript 中作用域分为全局作用域和局部作用域，局部作用域可访问全局作用域，全局不可访问局部作用域：

```javascript
// 例1：局部作用域访问全局作用域
var str = "Hello"  // 全局变量
function fn(){     
  // 局部作用域
  // 访问全局变量
  console.log(str) 
}
fn() // "Hello"

// 例2：全局作用域访问局部作用域
function fn(){    
  // 局部作用域 
  // 局部变量
  var str = "Hello"
}
// 全局作用域
// 访问局部变量
console.log(str) // Uncaught ReferenceError: str is not defined
```
闭包就是一个能在函数外部访问函数内部作用域的函数：

```javascript
function fn() {
    var str = "Hello"
    return function () {
        console.log(str)
    }
}
var result = fn()
result()
```
闭包在执行时产生，而不是函数定义时。
作用域在函数定义时已经产生，执行上下文环境在函数执行时产生，执行上下文环境有多个，作用域只有一个。

<!-- 闭包就是由函数创造的一个词法作用域，里面创建的变量被引用后，可以在这个词法环境之外自由使用。闭包通常用来创建内部变量，使得这些变量不能被外部随意修改，同时又可以通过指定的函数接口来操作。

>闭包就是能够读取其他函数内部变量的函数 -->

## Why
### 回收机制
每个函数被调用时创建的执行上下文环境是独立的，当函数执行完后执行上下文环境会被销毁，这就是浏览器的回收机制。那为什么闭包的执行上下文环境能保持不被回收呢？
<!-- ```javascript
function people(){
  var str = 'Hello'
  return function(){
    console.log(str)
  }
}
var man = people()
man()

``` -->


上面的 people 函数之所以不能访问内部的 str 是因为 str 是

闭包的作用就是延长变量的使用期限：
闭包主要是用来构建一个私有的作用域，使得在函数外部仍能访问函数内部。

## How

```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(function () {
        console.log(i) 
    }, 0)
}
```
上面的代码不会递增打印,而是会打印出三个 3。
因为在 ES6 之前没有块级作用域， for 内声名的变量其实是全局变量：
```javascript
for(var i=0;i<3;i++){}
console.log(i) // 3
```
又因为 setTimeOut 是异步任务，会在循环执行完后才执行，等循环执行完后 i 已经是 3 了。

闭包的作用是延长作用域，这个问题可以用闭包解决：
```javascript
for (var i = 0; i < 3; i++) {
    (function(i){
      setTimeout(function () {
        console.log(i) 
      }, 0)
    }(i))
}
```
使用闭包还可以使用模块模式：

```javascript
var people = (function () {
    var food = 'banana'
    return {
        eat(f) {
          food = f || food
          console.log('I want to eat' + food)
          }
        }
}())
people.eat('apple')
```
### 内存管理


链式作用域：内部函数一级一级寻找声明在外部函数的变量。



阮老师最后还留了两道思考题：
```javascript
var name = "The Window";
　　var object = {
　　　　name : "My Object",
　　　　getNameFunc : function(){
　　　　　　return function(){
　　　　　　　　return this.name;
　　　　　　};
　　　　}
　　};
alert(object.getNameFunc()());//The Window

var name = "The Window";
　　var object = {
　　　　name : "My Object",
　　　　getNameFunc : function(){
　　　　　　var that = this;
　　　　　　return function(){
　　　　　　　　return that.name;
　　　　　　};
　　　　}
　　};
　　alert(object.getNameFunc()());// My Object
```
弄清了 `this` 的指向，这两道题就很简单，`this` 指向当前调用者。
第一道题结果之所以是 **The Window** 是因为 `alert()` 的 `this` 指向全局。第二道题因为先把 `this` 赋给了变量，在通过变量来访问 `name` ,相当于 `this` 永远指向了 `object`。



## 总结
闭包相对于给函数创建了私有变量。


>参考文章：
http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html
https://developer.mozilla.org/cn/docs/Web/JavaScript/Closures
http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/00143449934543461c9d5dfeeb848f5b72bd012e1113d15000
http://coolshell.cn/articles/6731.html
http://www.w3school.com.cn/js/pro_js_functions_closures.asp
http://www.cnblogs.com/frankfang/archive/2011/08/03/2125663.html
https://cnodejs.org/topic/5482d05e73dcca8d21292928
https://cnodejs.org/topic/577298fd85e22178177ede44
http://stackoverflow.com/questions/111102/how-do-javascript-closures-work
https://www.gracecode.com/posts/2385.html
https://segmentfault.com/a/1190000000652891
[文章]:http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html
http://stackoverflow.com/questions/27254735/filereader-onload-with-result-and-parameter


https://www.jianshu.com/p/ca33fc82c114