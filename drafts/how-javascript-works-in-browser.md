---
title: Javascript 在浏览器中是如何工作的
---
# Javascript 在浏览器中是如何工作的
- 普通任务、宏任务、微任务的执行顺序
- 为什么 Promise 执行优先级高于 SetTimeOut？（2019-04-20）
<!-- 
你或许已经零散地了解了 Javascript 的调用栈（Call stack）、事件循环（Event loop）、回调队列（Callback queue）的概念，但是还是不知道 Javascript 到底是如何工作的，这可能是因为你的知识还没成体系。知识要成体系不但要从微观上去攻破还要从宏观上去关联，这样得到的知识才能记得牢固而不是过眼便忘。下面开始我的讲述。 -->

Javascript 工作过程可分为解析和执行两个阶段。
# 概念
## EC

### Globl EC
### Function EC
函数每次被调用时，都会创建函数执行环境。
## CES（执行环境栈）
函数执行环境会进入执行环境栈。
CES 也叫 Call Stack（调用栈），
Javascript 是单线程语言，它只有一个调用栈，所以它每次只能执行一段代码。至于为什么它被设计成单线程语言，是因为它的用途决定了它的设计。Javascript 最早的目的是被用来丰富网页交互，这必然避免不了要操作 DOM ，如果 Javascript 是被设计成多线程，就会出现两个线程同时修改同一个 DOM 时 ，浏览器不知以两个线程为准的难题，所以 Javacript 在浏览器中只能是单线程。
## 作用域






## 解析阶段
## 词法分析

## 语法分析

## 作用域确定



## 执行阶段
Javascript 代码自上向下解析，解析过程中首先会创建 Global Objct（全局对象），解析到函数是会创建 VO(变量对象)，在创建 VO 过程中会建立 EC 作用域（所以时候函数的作用域是在函数定义的时候就已经决定了，这叫词法作用域）。
## 创建 Global EC
### 创建 VO 对象
#### 创建作用域链
#### this 指向
#### 压入 CES

## 创建 Function EC
## 创建 AO 对象
#### 创建作用域链
#### this 指向
#### 压入 CES


## 执行函数代码


## 垃圾回收




























## Call Stack (调用栈)  
调用栈是一个数据结构，它描述了是代码执行的一个行为空间。
```javascript
function baz(){
    console.log('Hello from baz')
}

function bar(){
    baz()
}

function foo(){
   bar()
}

foo()
```
上面的代码会先按调用顺序压入调用栈（Call stack），然后再按调用栈中的顺序从上往下执行，执行完的代码后会立即弹出调用栈，然后再继续执行下一段代码。这就是调用栈的后进先出（LIFO：Last in first out）特点：

![](/better/browser/1_rRoLpv-Zrmpa-srNhwlbvA.gif)

后进先出有点类似于枪械(🔫)的射击，后装的子弹反而会先被发射。
## Event Loop（事件循环）
上面是同步任务的执行过程，我们再看下异步任务的执行过程。

```javascript
function printHello() {
    console.log('Hello from baz');
}

function baz() {
    setTimeout(printHello, 3000);
}

function bar() {
    baz();
}

function foo() {
    bar();
}

foo();
```
::: tip
setTimeout 不属于 Javascript API，它属于浏览器 API。
:::
所有的任务都会先进入调用栈，如果 Javscript 引擎检测到该任务是同步任务则会在调用栈保留，异步任务则会在回调队列先挂起，等调用栈中的同步任务全部执行完且全部出栈后，异步任务才会进入调用栈，这种针对同一个任务的调用方式（进栈->出栈->进栈）就叫事件循环。事件循环是针对于异步任务的，每当调用栈无任务时，事件循环就会从回调队列取出一个任务压入调用栈执行。

![](/better/browser/1_9mv-g9E-87Sji9j7YR08Fw.gif)



如果调用栈永远有任务，回调队列中的异步任务就永远不会执行。有限的任务总会执行完的，如果执行不完就说明一直有任务进入调用栈：
![](/better/browser/3925f8363d7a763e6474709ccddf7d96.png)
调用栈是有任务个数限制的，如果超出限制浏览器就会报栈溢出错误：

![](/better/browser/1_VODaYMxARLv6E1fguKYVKA.png)

异步任务有分宏任务和微任务，微任务的执行优先级要高于宏任务。
```javascript
 console.log('script start');
 2 
 3 setTimeout(function() {
 4   console.log('setTimeout');
 5 }, 0);
 6 
 7 Promise.resolve().then(function() {
 8   console.log('promise1');
 9 }).then(function() {
10   console.log('promise2');
11 });
12 
13 console.log('script end');

script start
script end
promise1
promise2
setTimeout
```

## 回调队列
上面事件循环中已经讲解，回调队列是异步任务的暂存区域。回调队列中的任务要等到调用栈清空时在进入调用栈，所以 setTimeout() 设置的延迟执行并不是可靠的。


浏览器和 NodeJs 中的 Event loop并不一致，因为对 NodeJs 不是很了解就不讲述了。 
参考文章：
> https://itnext.io/how-javascript-works-in-browser-and-node-ab7d0d09ac2f
http://www.ruanyifeng.com/blog/2014/10/event-loop.html
https://jakearchibald.com/2015/tasks-microtasks-queues-and-schedules/


https://feclub.cn/post/content/ec_ecs_hosting
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Functions#%E9%97%AD%E5%8C%85%28Closures%29


https://blog.fundebug.com/2019/03/20/understand-javascript-context-and-stack/