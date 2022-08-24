---
title: 数据类型的检测 
---
# 数据类型的检测 
- 怎么检测一个对象是 `Object` 类型？（2017-02-21）
- Javascript 基本数据类型有哪些？（2019-04-08）

JS 中 有 6 种基本数据类型，它们分别是 String、Bollean、Number、Null、Undefined 以及 ES6 新增的 Symbol。除基本数据类型外，JS 还有一种复杂数据类型—— Object。

<!-- > 基本数据指的是非对象且无方法的数据。 -->

下面是关于数据类型检测的几种方法：

# typeof

`typeof` 可用于检测某种数据的类型的操作符，其返回结果是该数据类型的字符串形式：
```javascript
typeof x // undefined
typeof true // boolean
typeof 'x' // string
typeof 9 // number 
typeof {} // object
typeof function() {} // function
typeof Symbol() // symbol
```
`typeof` 检测基本数据值类型还是挺有用的，但是对于复杂数据类型就有一点不够用了：

```javascript
typeof [] // object
typeof null // object
```
# instanceof

`instanceof` 可检测一个对象是否是一个构造函数的实列。由于它沿着原型链检测的，所以用来检测值类型并不是太合适：
```javascript
// 预期结果
[] instanceof Array // true

// 意外结果
[] instanceof Object // true
```

# Object.prototype.toString && call












# 总结

```javascript
function isBoolean(arg) {
  return typeof arg === 'boolean';
}

function isNumber(arg) {
  return typeof arg === 'number';
}

function isString(arg) {
  return typeof arg === 'string';
}

function isFunction(arg) {
  return typeof arg === 'function';
}

function isObject(arg) {
  return arg !== null && typeof arg === 'object';
}
```
<!-- https://www.cnblogs.com/onepixel/p/5126046.html -->

https://webbjocke.com/javascript-check-data-types/
https://stackoverflow.com/questions/1303646/check-whether-variable-is-number-or-string-in-javascript