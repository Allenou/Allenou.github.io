---
title: CSS 居中
date: 2016-02-01
categories: 厚积
tags:
  - mobile
  - css
  - js
---
# CSS 居中
- 如何居中一个尺寸不固定的元素？（2019-04-22）

CSS 居中是面试中常出现的问题，也是日常工作中常遇到的问题。可能很多人会觉得这个问题简单，那让你多用几种方式实现能做到吗？固然有一招鲜吃遍天的方式，但多知道几种也绝不是坏事。本文只讲述当元素尺寸不固定时如何水平垂直居中，因为我觉得尺寸不固定都能居中，知道了尺寸再居中也不是难事了。那么开始本文的讲述。

```html
<div class="parent">
    <span class="child"></span>
</div>
```
<template>
    <div class="parent" style="width:400px;height:200px;border:2px solid #000">
      <span class="child" style="background-color:#ccc;"></span>
    </div>
</template>

### 第一种：
```css
.dialog {
    position: fixed;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    width: 300px;
    height: 300px;
    margin: auto;
}
```
这种方法需预先设置元素宽高，或者用 **JS** 临时获取并设置。

### 第二种：
```css
.dialog {
    position: fixed;
    z-index:-1;
    top: 50%;
    left: 50%;
    width: 300px;
    height: 300px;
    margin-top: -150px;
    margin-left: -150px;  
}
```
这种方法也是先设置元素宽高，然后再设置 ``top:50%`` 和 ``left:50%`` 使元素的顶部和左部的距离为视窗宽高的一半，再通过 `margin` 向上和向左移动元素宽高的一半。

### 第三种：
```css
.dialog{
    position: fixed;   
    top: 50%;   
    left: 50%;   
    transform: translate(-50%, -50%);
}
```
>transform 属性向元素应用 2D 或 3D 转换。该属性允许我们对元素进行旋转、缩放、移动或倾斜。

这种方法的原理和第二种一样，只不过这种是用 **CSS3** 的 `transform` 代替了 `margin`，所以无需设置元素的宽高，但在低版本浏览器中会存在兼容问题。

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/06/141431c12c74df8848aaf28b4db7b15c.jpg)

移动端浏览器内核大部分都是 **-webkit-** ，其对 **CSS3** 的支持率是最好的，所以移动端项目仍可以一爽而快。

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/06/cf188f84fbb1b389e01c256cda101d2b.png)


### 第四种：
```html
 <dialog>
     hello world~
 </dialog>
```
`<dialog>` 是 **HTML5** 新提供的元素，其默认为隐藏状态，可通过 ``open`` 属性改为显示：
```html
 <dialog open>
     hello world~
 </dialog>
```
添加 ``open`` 属性后，元素会水平居中在视窗顶部。

还可通过 ``show`` 和 ``showModal`` 方法改为显示，使用 ``show`` 方法后的效果与直接在标签上添加 `open` 属性一样：
```javascript
document.getElementsByTagName('dialog')[0].show()
//等于
<dialog open>hello world~</dialog>
```
``showModal`` 方法则可以直接使元素水平垂直居中：
```javascript
document.getElementsByTagName('dialog')[0].showModal()
```
对于用 ``showModal`` 方法显示的 **dialog**，可以通过以下方式更改遮罩层样式：
```css
dialog::backdrop {
  background: rgba(0,0,0,0.7);
}
```

虽然 `<dialog>` 很方便，但目前其在浏览器的支持情况不是很好：

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/06/b039772efa3fe1db32f24e1f19567c72.png)

>参考文章：      
http://div.io/topic/1406        
http://www.zhangxinxu.com/wordpress/?p=3794       
https://github.com/simaQ/cssfun/issues/1
http://www.zhangxinxu.com/wordpress/?p=2636
http://www.zhangxinxu.com/wordpress/?p=1494
https://www.w3cplus.com/css/simplify-your-stylesheets-with-the-magical-css-viewport-units.html
https://jinlong.github.io/2013/08/13/centering-all-the-directions/



