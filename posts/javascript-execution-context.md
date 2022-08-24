---
title: 执行环境
date: 2020-08-17
---

**执行环境**（Execution Context）也被译为**执行上下文**，它是 Javascript 引擎（Engine ）在解析和执行一段可执行代码（Executable Code）时的一个抽象概念。
<!-- more -->

在 Javascript 中可执行代码分为三种（全局代码，函数代码，Eval 代码），所以便有三种执行环境：
- 全局执行环境（Global Execution Context）：包含函数执行环境。当浏览器解析到 Javascript 代码时会立即创建一个全局执行环境。由于 Javascript 是单线程语言，所以也仅会创建一个
- 函数执行环境（Function Execution Context）：创建于每次函数被调用时
- Eval 执行环境（不建议使用）

<Picture name="4.jpeg"></Picture>

执行环境的生命周期主要分为创建、执行、销毁，销毁涉及回收机制，会单独开一篇文章讲，本文主要讲创建和执行。

## 创建
在创建阶段，引擎会创建一个包含 **变量对象**（Variable Object）、**作用域链**（Scope Chain）、 **this** 的 **执行环境对象** （Execution Context Object）:
```javascript
ExecutionContextObject = {
 VO: {},
 ScopeChain: {},
 this: {}
}
```
### 变量对象
创建阶段中的变量对象是不能直接被访问的，

不同执行环境中，变量对象和 this 指向会有所不同。在**全局执行环境**中，变量对象即全局对象， this 指针也会指向全局对象：
```javascript
GlobalExecutionContextObj = {
 VO: window,
 ScopeChain: {},
 this: window
}
```
而在函数执行环境中，变量对象为函数自己，this 指向该函数的调用者： 
```javascript
FunctionExecutionContextObj = {
 VO: 函数,
 ScopeChain: {},
 this: 最终调用者
}
```
全局执行环境和函数执行环境的变量对象包含的属性也不一致。

**全局执行环境**：
- 变量声明（Variable Declaration）
- 函数声明（FunctionDeclaration）

**函数执行环境**：
- 变量声明（Variable Declaration）
- 函数声明（FunctionDeclaration）
- 函数形参（Function Arguments）

在函数执行环境的创建阶段（函数被调用，其内部代码还未执行），其变量对象中的变量虽只被声明还未赋值，但函数的形参值及形参个数已确定。

```javascript

function test(x) {
  var a = 10
  var b = function d () {}

  function c () {}
}
test(20)
```
上面 `test` 函数的执行环境在创建阶段如下 ：
```javascript
VO(test functionContext): {
  arguments: {
   0: 20,
  length: 1
  },
  x: 20,
  c: function c(),
  a: undefined,
  b: undefined
}
```
## 执行

在执行阶段，函数内部代码开始执行。

### 活动对象

在执行阶段执行环境的变量对象（VO）也被激活成为活动对象（AO）：
```javascript
AO(test functionContext): {
  arguments: {
   0: 20,
  length: 1
  },
  x: 20,
  c: function c() {},
  a: 10,
  b: function d() {}
}
```

关于 Javascript 环境从创建到执行的过程可以在 [javascript-visualizer](https://ui.dev/javascript-visualizer) 查看。

## 大白话

全局执行环境和函数执行环境的关系可以简单的抽象为一个面试过程。公司为全局执行环境，员工为函数执行环境，
> 参考资料：
- [了解JavaScript的执行环境](https://yanhaijing.com/javascript/2014/04/29/what-is-the-execution-context-in-javascript/)
- [Execution context, Scope chain and JavaScript internals](https://medium.com/@happymishra66/execution-context-in-javascript-319dd72e8e2c)

<!-- http://jibbering.com/faq/notes/closures/#clIRExSc -->
<!-- http://web.jobbole.com/91134/ -->
<!-- 
https://www.cnblogs.com/tomxu/archive/2012/01/16/2309728.html

https://blog.bitsrc.io/understanding-execution-context-and-execution-stack-in-javascript-1c9ea8642dd0


https://zhuanlan.zhihu.com/p/72959191 -->
