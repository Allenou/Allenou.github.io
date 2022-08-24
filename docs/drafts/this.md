title: this
date: 2020-07-06
categories: 厚积
tags: js
---
this 是 Javascript 中的一个关键字，它表示当前对象的一个引用。this 并非固定不变，它会随着执行环境的改变而改变。this 一直被认为很难理解，但其实并不是那么可怕，只要弄清楚谁是最终调用者。

## 对象中的 this
```javascript
var person = {
  firstName: "John",
  lastName : "Doe",
  id       : 5566,
  fullName : function() {
    return this.firstName + " " + this.lastName;
  }
};
person.fullName() // John Doe
```
上面代码中最终的调用者是 person 所以 this 指向 person。

## 单独使用 this
```javascript
var content = this
```
上面代码中，因为是全局环境中给 context 赋值，所以 this 指向全局对象，浏览器中的全局对象是 window ，nodejs 中则是 global。

## 方法中的 this

### 默认
```javascript
function food (){
    return this
}

food()
```
方法种的 this 会默认指向全局对象

### 严格模式

```javascript
"use strict";
function food (){
    return this
}
```

严格模式中，this 无明确指向，它为 undefined
### 箭头函数
```javascript
const context = (()=>this)
```
箭头函数 this 指向全局


## 事件中的 this
```javascrpt
<button onclick="this.style.display='none'">
点我后我就消失了
</button>
```
事件中的 this 指向 HTML 元素。



## 显示函数绑定




```javascript
function Boy(food){
    this.food = food     
}


Boy.prototype = {
    eat:function(){
        console.log('I like to eat' + this.food)
    }
}

function Girl (){
    this.food = food
}
Girl.prototype ={
    eat:Boy.eat.call(Girl)
}

var gril = new Girl()
gril('apple')
```

## 构造函数


## 原型链

## getter、setter

https://codeburst.io/the-simple-rules-to-this-in-javascript-35d97f31bde3
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this
https://github.com/getify/You-Dont-Know-JS/blob/1ed-zh-CN/this%20%26%20object%20prototypes/ch2.md
