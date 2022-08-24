---
title: 字符串倒序
date : 2017-02-21
---
- 实现字符串倒序

<!-- more -->

字符串没有原生的倒叙方法，只能自己实现，下面是几种实现方法：
## Plan A:
```javascript
String.prototype.reverse = function (){
    var reverseStr = ''
    for(var i = this.length; i >= 0; i--){
        reverseStr += this.charAt(i)
    }
    return reverseStr;
}
"一闪一闪亮晶晶".reverse() // "晶晶亮闪一闪一"
```
::: tip
charAt() 方法从一个字符串中返回指定的字符
:::

## Plan B:

```javascript
String.prototype.reverse = function (){
    var reverseStrArr = []
    for(var i = this.length; i >= 0; i--){
        reverseStrArr.push(this.charAt(i))
    }
    return reverseStrArr.join('');
}
"一闪一闪亮晶晶".reverse() // "晶晶亮闪一闪一"
```
::: tip
join() 方法将一个数组（或一个类数组对象）的所有元素连接成一个字符串并返回这个字符串。如果数组只有一个项目，那么将返回该项目而不使用分隔符。
:::


## Plan C: 

```javascript
String.prototype.reverse = function (){
   return this.split('').reverse().join('')
}
"一闪一闪亮晶晶".reverse() // "晶晶亮闪一闪一"
```
::: tip
split() 方法使用指定的分隔符字符串将一个String对象分割成字符串数组，以将字符串分隔为子字符串，以确定每个拆分的位置。 
:::
这种方法是先将字符串分割成字符组成的数组，然后通过数组的倒叙方法实现倒叙，再讲数组中的字符连接成字符串。
