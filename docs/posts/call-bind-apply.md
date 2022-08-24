---
title: call 、apply 和 bind
date: 2020-07-06
---
谈这三兄弟之前不得不谈下 `this`，因为它们都是用于改变函数执行时的上下文，即函数内部的 `this` 指向。

## this
**函数内部的 `this` 最终指向最后调用该函数的对象**。
```javascript
var bar = 2;

var obj = {
  bar: 1,
  foo: function() { 
    console.log( this.bar ) 
  }
};

var foo = obj.foo;

obj.foo() // 1
foo() // 2
```
`obj.foo()` 输出 1 是因为函数调用对象是 `obj` ，而 `foo()` 输出 2 则是因为函数调用对象是全局对象（window）。

```javascript
var obj = {
  foo: function() {
    console.log( this === obj,this === window ) 
  },
};

var foo = obj.foo;

obj.foo() // true false
foo() // false true
```
如果想要 `foo()` 也输出 1 就该三兄弟出场了。

## call、apply 和 bind
call、apply 和 bind 都可改变函数调用时的 `this` 指向，要想 this 指向谁第一个参数就传谁：
```javascript
var bar = 2;

var obj = {
  bar: 1,
  foo: function() { 
    console.log( this.bar ) 
  }
};

var foo = obj.foo;

obj.foo()       // 1
foo.call(obj)   // 1
foo.apply(obj)  // 1
foo.bind(obj)() // 1
```
从上面例子看起来 call 和 apply 似乎并无差别，但是他们还是有差别的：
```javascript
 var obj = {
    a: 1,
    add: function (b, c) {
        console.log(this.a + b + c);
    },
};

var add = obj.add;
      
add.call(obj, 2, 3);    // 6
add.apply(obj, [2, 3]); // 6
add.bind(obj, 2, 3)();  // 6
```
call 和 apply 的区别在于传参上，call 的参数是以单个接收，apply 是接收一个数组。bind 的接参形式与 call 一致，其与前两者的区别在于 bind 不会立即执行而是返回一个新函数。

## call 的应用

- 类型验证

```javascript
isArray(obj){ 
    return Object.prototype.toString.call(obj) === '[object Array]';
}
```

## apply 的应用

- 找出数组中最大的数：
```javascript
var a = [2, 4, 6, 8, 10, 7, 5, 3, 1];

Math.max.apply(null, a) // 10
```
<!-- 语法：
func.apply(thisArg, [argsArray])

参数：
thisArg
必选的。在 func 函数运行时使用的 this 值。请注意，this可能不是该方法看到的实际值：如果这个函数处于非严格模式下，则指定为 null 或 undefined 时会自动替换为指向全局对象，原始值会被包装。


argsArray
可选的。一个数组或者类数组对象，其中的数组元素将作为单独的参数传给 func 函数。如果该参数的值为 null 或  undefined，则表示不需要传入任何参数。从ECMAScript 5 开始可以使用类数组对象。 浏览器兼容性 请参阅本文底部内容。 -->


## bind 的应用

柯里


参考资料

https://wangdoc.com/javascript/oop/this.html