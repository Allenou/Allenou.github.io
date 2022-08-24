title: 深浅拷贝
date: 2018-11-14 21:55:01
categories: 厚积
tags: js
---

<!--more-->

拷贝即复制，就是将一个值为基本类型或引用类型的变量赋給另一个变量。源变量的值为引用类型时，赋值操作只是将指向源变量值的引用赋给新变量，实际上两个变量在内存上指向的仍是同一个值，这便是浅拷贝。浅拷贝会导致新变量和源变量之间会相互影响：

```javascript
// example 1
var a = {
  name : 'allen'
}
var b = a

b.name = 'jenica'
console.log(a.name) // jenica

// example 2
var c = [1, 2, 3]
var d = c
d.push(4)
console.log(c) // [1, 2, 3, 4]
```
有浅拷贝自然有深拷贝，深拷贝出来的对象与源对象互不影响。虽然使用对象的原型方法得到的新对象看上去像是深拷贝，但实际还是浅拷贝：

**Object.assign**

> `Object.assign()` 可将源对象所有可枚举属性复制到目标对象并返回目标对象

```javascript
// example 1
var obj1 = {
  a: 1,
  b: 2,
  c: 3
}
var obj2 = Object.assign({},obj1)
obj2.a = 'a'
console.log(obj1.a) // 1

// example 2
var obj1 = {
  a: 1,
  b: 2,
  c: {
    d: 3
  }
}
var obj2 = Object.assign({},obj1)
obj2.c.d = 'd'
console.log(obj2.c.d ) // 'd'
```
从上例可知 `Object.assign()`  只对源对象的第一层的属性进行了深拷贝处理。



**扩展运算符**
```javascript
// example1
var arr1=['a','b','c']
var arr2 = [...arr1]
arr2[2]='d'
console.log(arr1) //  ["a", "b", "c"]

// example2
var arr1=['a','b',{c:'c'}]
var arr2 = [...arr1]
arr2[2].c='d'
console.log(arr1) //  ["a", "b", {c:"d"}]
```

**Array.prototype.slice**
>`slice()` 在不改变原数组的情况下， 返回一个由参数 begin 和 end（不包括 end）决定的新浅拷贝数组。
```javascript
// example1
var arr1=['a','b','c']
var arr2 = arr1.slice()
arr2[2]='d'
console.log(arr1) //  ["a", "b", "c"]

// example2
var arr1=['a','b',{c:'c'}]
var arr2 = arr1.slice()
arr2[2].c='d'
console.log(arr1) //  ["a", "b", {c:"d"}]

```

**JSON.stringify**
>JSON.stringify() 用于 Javascript 对象或值转换为 JSON 字符串。

```javascript
// example 1
var obj1 = {
  a: 1,
  b: 2,
  c: {
    d: 3
  }
}
var obj2 = JSON.parse(JSON.stringify(obj1))
obj2.c.d = 'd'
console.log(obj1) // {a: 1, b: 2, c: {d:3}}

// example 2
var obj1 = {
  a: 1,
  b: 2,
  c: function(){
     console.log('c')
    }
}
console.log(JSON.stringify(obj1)) // {"a":1,"b":2}

```
从上例可知，`JSON.stringify()` 在对含有方法的对象拷贝的不完全。

深浅拷贝都是刀，具体使用哪把刀要看用来杀鸡还是杀牛。

https://juejin.im/post/59ac1c4ef265da248e75892b
https://github.com/wengjq/Blog/issues/3
https://www.zhihu.com/question/23031215
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/assign

http://latentflip.com/loupe/?code=JC5vbignYnV0dG9uJywgJ2NsaWNrJywgZnVuY3Rpb24gb25DbGljaygpIHsKICAgIHNldFRpbWVvdXQoZnVuY3Rpb24gdGltZXIoKSB7CiAgICAgICAgY29uc29sZS5sb2coJ1lvdSBjbGlja2VkIHRoZSBidXR0b24hJyk7ICAgIAogICAgfSwgMjAwMCk7Cn0pOwoKY29uc29sZS5sb2coIkhpISIpOwoKc2V0VGltZW91dChmdW5jdGlvbiB0aW1lb3V0KCkgewogICAgY29uc29sZS5sb2coIkNsaWNrIHRoZSBidXR0b24hIik7Cn0sIDUwMDApOwoKY29uc29sZS5sb2coIldlbGNvbWUgdG8gbG91cGUuIik7!!!PGJ1dHRvbj5DbGljayBtZSE8L2J1dHRvbj4%3D

https://flaviocopes.com/how-to-clone-javascript-object/
https://scotch.io/bar-talk/copying-objects-in-javascript、
https://www.codementor.io/avijitgupta/deep-copying-in-js-7x6q8vh5d
https://we-are.bookmyshow.com/understanding-deep-and-shallow-copy-in-javascript-13438bad941c
https://medium.com/@tkssharma/objects-in-javascript-object-assign-deep-copy-64106c9aefab
https://baoyuzhang.github.io/2017/07/03/%E3%80%90JS%E8%83%8C%E5%90%8E%E3%80%91JavaScript%E4%B8%AD%E7%9A%84%E5%A0%86%E6%A0%88/
https://www.cnblogs.com/jingwhale/p/4884759.html
https://www.cnblogs.com/zhuzhenwei918/p/7364529.html



question：基本类型赋值是重新是在栈内存重新开辟一个变量存储的，为什么也是浅拷贝

answer:https://stackoverflow.com/questions/184710/what-is-the-difference-between-a-deep-copy-and-a-shallow-copy



https://www.jianshu.com/p/1b8f6e129117

实现深拷贝也需开辟新空间？
