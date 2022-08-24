---
title: 对象转数组
date: 2018-11-19
categories: 厚积
tags: js
---
*Javascript* 中并非一切皆对象，而是主要分为以下几种基本类型：
<!-- more -->
* Number
* String
* Boolean
* Function
* Object
* Symbol

可以发现数组并不在上述类型里面，它是归在 *Object* 一类的，因为数组是一种特殊对象。所以上述类型若要详细分又可分为：
* Number
* String
* Boolean
* Symbol
* Object
    * Function
    * Array
    * Date
    * RegExp
* Null
* Undefined

一般说的对象指的是含有键值对的普通对象，所以本文所说的“对象转数组”就是普通对象转特殊对象（数组）。

下面以一个普通对象为例子，开始讲对象转数组的几种方式：

```javascript
// 普通对象
const peoples={
        allen:{
            age:20
        },
        james:{
            age:19
        },
        frank:{
            age:18
        }
    }
```

## Object.values()

`Object.values()` 返回一个对象可枚举属性的值数组。
```javascript
Object.values(peoples) //  [ { age: 20 },  { age: 19 },  { age: 18 }]
```

## Object.keys()

`Object.keys()` 返回的是一个对象可枚举的属性数组：

```javascript
Object.keys(peoples) // ["allen", "james", "frank"]
```
`Object.keys()` 和 `Array.prototype.map()` 组合使用可以得到一个对象可枚举属性的值数组：
```javascript
Object.keys(peoples).map(index => peoples[index]) //  [{ age: 20 },{ age: 19 },{ age: 18 }]
```

## Object.entries()

`Object.entries()` 是 `Object.values()` 和 `Object.keys()` 的组合版，它返回的是一个对象可枚举的键值对数组：
```javascript
Object.entries(peoples) // [["allen",  { age: 20 }],  ["james",  { age: 19 }], ["frank",  { age: 18 }]]
```

> 参考文章：
- [重新介绍 Java​Script](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/A_re-introduction_to_JavaScript)