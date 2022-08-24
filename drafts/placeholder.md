---
title: 模拟 placeholder
date: 2015-11-20
categories: 厚积
tags:
  - mobile
  - css
  - js
---
# 模拟 placeholder
HTML5 新增的 Date pickers 给开发带来了极大的便利，不过不支持输入提示（placeholder），这对移动端来说有点美中不足。
<!--more-->
下面是在移动端能达到输入提示效果的几种方法。

## 方法一：
先将 input 初始为 text 类型，当获得焦点时改变类型。
```html
<input placeholder="请选择日期" type="text" onfocus="(this.type='date')" id="date">
```
* 优点：代码量少
* 缺点：需点击两次才展现选择组件

## 方法二：
通过伪元素实现：
```html
<style>
#date:before {
    content: '请选择日期';
    position: absolute;
    top: 9px;
    left: 10px;
    color: #ccc;
}
</style>
<input type="date" id="date">
```
* 优点：-
* 缺点：1.原生 JS 操作伪元素不太方便 2. CSS 压缩后，文案不好修改

## 方法三：
新增一个标签，使用定位将其定位到输入框后面，并将输入框背景色设为透明，使得可以看见后面的提示：

```html
<style>
.date {
    position: relative;
}
.date input {
    width: 150px;
    background-color: transparent;
}
.date .tips {
    position: absolute;
    top: 9px;
    left: 10px;
    font-size: 12px;
    color: #A9A9A9;
}   
</style>
<div class="date">
    <input type="date" id="date">
    <span class="tips">请选择时间</span>
</div>
```
* 优点：原生 JS 操作 DOM 比较容易
* 缺点：代码量多

## 方法四：
通过伪元素配合 attr 方法实现：
```html
<style>
#date:before {
    content: attr(placeholder); // 重点
    position: absolute;
    top: 9px;
    left: 10px;
    color: #ccc;
}
</style>
<input type="date" id="date" placeholder="请选择日期">
```
通过 attr 返回自定义属性里的值并作为伪元素的 content。上面的 placeholder 只是自定义属性，不喜欢也可以写成其它的。
* 优点：原生 JS 操作 DOM 比较容易
* 缺点：代码量少

> 相关文章：
https://davidwalsh.name/css-content-attr 
