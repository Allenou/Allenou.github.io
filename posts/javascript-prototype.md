--- 
title: 原型
date: 2019-02-18
---
- prototype、[[prototype]]、constructor 三者之间的关系

<!-- more -->
Javascript 是弱类型语言，它没有类（Class）的概念，但它通过更灵活的原型（Prototype）实现了类的功能。

::: tip
ES6 的 Class 只是语法糖，本质上还是 Prototype。 
:::

## 函数和原型

每个函数都有一个 `prototype` 对象，该对象有一个指向函数本身的 `constructor` 属性：
```javascript
function SuperType(){}

SuperType.prototype.constructor  // SuperType
```
<Picture name="0.jpg"></Picture>

`prototype` 对象中可以添加自定义属性：

```javascript
function SuperType(){
    this.value = 'Hello world'
}

SuperType.prototype.getSuperValue = function(){
    return this.value
}
```

也可以直接覆盖函数的 `prototype` 对象，覆盖时需要在新对象中将 `constructor` 属性指向原来的构造函数，否则就会破坏原型链：

```javascript
function SuperType(){
    this.value = 'Hello world'
}

SuperType.prototype = {
    getSuperValue: function () {
        return this.value;
    }
};

SuperType.prototype.constructor  // Object

// 其它
```
正确做法：
```javascript
function SuperType(){
    this.value = 'Hello world'
}

SuperType.prototype = {
    constructor: SuperType,  // 注意点
    getSuperValue: function () {
        return this.value;
    }
};

SuperType.prototype.constructor  // SuperType
```

<Picture name="1.jpg"></Picture>

函数的实例对象之所以能访问函数的原型对象是因为其有一个 `[[prototype]]` 指针指向了原型对象，浏览器则是通过 `__proto__` 属性实现了这个指针：
```javascript
var instance = new SuperType()
instance.getSuperValue() // Hello world

instance.__proto__=== SuperType.prototype  // true
```
::: tip
对象的 `__proto__` 指针指向那个创建它的函数的原型对象。
:::

<Picture name="2.jpg"></Picture>

<!-- 由于 `SuperType` 函数是 `Function` 的实例，所以 `SuperType` 的 `[[prototype]]` 指针指向 `Function.prototype`：
```javascript
SuperType.__proto__ === Function.prototype  // true
``` -->

## 函数和 Function

所有函数都由 `Function` 创建,即所有函数都是 `Function` 的实例,所以所有函数的 `[[prototype]]` 指针指向 `Function` 的原型对象（`Function.prototype`）：

```javascript
SuperType.__proto__ === Function.prototype  // true
```
`Function` 本身也是一个函数,所以是它的创建者是它自己：

```javascript
Function.__proto__ === Function.prototype  // true
```

`Object` 和 `Function` 一样也是函数，所以它的创建者也是 `Function`：

```javascript
Object.__proto__ === Function.prototype  // true
```

`Function` 是所有函数的起源，那它的原型对象指针又指向哪呢？答案是 `Object`:

```javascript
Function.prototype.__proto__ === Object.prototype   // true
```
而 `Object` 的原型对象指针则指向原型对象的终点 `null`: 
```javascript
Object.prototype.__proto__ === null                 // true
```
hasOwnProperty
## Function 和 Function
## Function 和 Object

`Function` 和 `Object` 之间的关系有点绕：
```javascript
Object.__proto__ === Function.prototype             // true
Function.__proto__ === Function.prototype           // true
Function.prototype.__proto__ === Object.prototype   // true
Object.prototype.__proto__ === null                 // true
```

`Function` 是由它自己创建，所以:

```javascript
Function.__proto__ === Function.prototype           // true
```
同时 `Funtion` 又是对象：
```javascript
Function.prototype.__proto__ === Object.prototype   // true
```
<!-- ```javascript
Object instanceof Function;   // true
Function instanceof Object;   // true
Function instanceof Function; // true
``` -->
之所以会出现这种现象是因为 Object 的



从实例和原型的关系可知函数由 `Function` 创建，所以函数是 `Function` 的实例：


 `[[prototype]]` 指针都指向 `Function.prototype` ,而 `Function` 本身就是一个实例，所以 `Function.__proto__` 指向 `Function.prototype` ，即指向它自己。

对象都是 `Object` 创建出来的，所以所有的原型对象的 `[[prototype]]` 指针都指向 `Object.prototype`，而 `Object` 又是一个函数，所以 `Object.__proto__` 指向 `Function.prototype`。

但作为对象最顶端的 `Object.prototype.__proto__` 指向 `null`。 

<Picture name="3.jpg"></Picture>


## 原型链
结合上面的结论可得如下关系图：
<Picture name="4.jpg"></Picture>

这种实例和构造函数以及原型对象之间的关系便是原型链。
这肿属性的查找路径就是原型链。

原型对象只存在于函数，普通对象没有原型对象：
```javascript
var obj = {}
obj.prototype // undefined
```
但是有  `[[prototype]]` 指针：
```javascript
var obj = {}
obj.__proto__ === Function.prototype  // true
obj.__proto__ === Object.prototype  // false
obj2.__proto__.__proto__ === Object.prototype)
```

## 总结 
* 原型的主要作用是继承
* 函数都是 `Function` 创建出来的，即所有函数都是 `Function` 的实例
* 对象都是 `Object` 创建出来的，即所有对象都是 `Object` 的实例
* `prototype` 链接的是构造函数和原型对象，`__proto__` 链接的是实例对象和原型对象。

> 参考文章：
- [小角度看JS原型链](https://segmentfault.com/a/1190000003507782)
- [深入理解javascript原型和闭包系列](http://www.cnblogs.com/wangfupeng1988/p/4001284.html)
- https://www.cnblogs.com/TomXu/archive/2012/01/16/2309728.html
- https://www.javascripttutorial.net/javascript-prototype/
- https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Inheritance_and_the_prototype_chain
