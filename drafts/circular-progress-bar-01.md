---
title: 环形进度条
date: 2016-03-21
categories: 厚积
tags: css
---
使用拼接法实现环形进度条。
<!--more-->
html分三层。
```html
<style>
.circular {
  width: 100px;
  height: 100px;
  border: 20px solid transparent;
}
</style>
<div class="circular-progress">
  <div class="wrapper right">
    <div class="circular right-circular"></div>
  </div>
  <div class="wrapper left">
    <div class="circular left-circular"></div>
  </div>
</div>
```
首先用相邻的``border``组成两对直角。

**组合方式一：**

**右半环**：``border-top`` + `` border-right``    
**左半环**：``border-top`` + `` border-left``

```css
.right-circular {
  border-top: 20px solid lightgreen;
  border-right: 20px solid lightgreen;
}
.left-circular {
  border-bottom: 20px solid lightgreen;
  border-left: 20px solid lightgreen;
}
```
效果图：

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/17/2daa4a800f90245f9f1a832dd71bf3b0.png)


**组合方式二：**

**右半环**：``border-top`` + `` border-left``    
**左半环**：``border-top`` + `` border-right``

```css
.right-circular {
  border-top: 20px solid lightgreen;
  border-left: 20px solid lightgreen;
}
.left-circular {
  border-bottom: 20px solid lightgreen;
  border-right: 20px solid lightgreen;
}
```
效果图：

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/17/4c19aab08c8fae6740ede3d11baa9db9.png)


选择哪种方式不重要，只要组成直角就行了。只不过到时旋转的时候两种方式的初始旋转角度和最终选择角度刚好相反。考虑到一般环形进度条都是从90°的位置开始，所以我选择的是方式二。

接下来把直角变成圆弧。
```css
.circular {
  border-radius: 50%;
}
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/17/2d88fa09f264c3c1259d4d516927ee07.png)

为了方便计算旋转角度和辨别，调整下位置，更改下颜色。
```css

.circular {
  position: absolute;
  top: 0;
  transform:rotate(-45deg);
}
.right-circular {
  border-top: 20px solid lightgreen;
  border-right: 20px solid lightgreen;
}
.left-circular {
  border-bottom: 20px solid lightblue;
  border-left: 20px solid lightblue;
}
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/17/6814640f3514621fb1ec1666b4c71dab.png)

接下来让半环们动起来。
还是因为环形进度条从90°开始，所以先让右半环先动起来。

这里用到了css3动画，关于css3动画属性介绍可以戳我之前写的[css制作loading图]。
```css
.right-circular{
  animation: circularAnimation 3s linear infinite;
}
@keyframes circularAnimation {
  0% {
    -webkit-transform: rotate(-45deg);
  }

  100% {
    -webkit-transform: rotate(135deg);
  }
}
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/18/f2359f7df858f1b03df20c7ac9771e90.gif)

什么没感觉？

```css
.wrapper{
  overflow: hidden;
  position: absolute;
  top: 0;
  width: 70px;
  height: 140px;
}
.right-circular{
  right: 0;
}
```
``wrapper``定宽70，刚好是``circular-progress``的一半，是为了将半环隐藏，然后再通过旋转呈现。

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/18/20d7fe9feb5a02b8bbd0941f52fdff76.gif)

这下有了吧。

好吧，废话不多说了，左右一起上吧。

```css
.circular-progress {
  position: relative;
  width: 140px;
}
.right {
  right: 0;
}
.left {
  left: 0;
}
.left-circular{
  animation: circularAnimation 3s linear infinite;
}
```
``circular-progress``的宽之所以是140px，是因为``circular``的宽是100px,边框是20px。由盒子模型可得其宽应为140px，否则半环定位后不能组成圆环。

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/18/364284290e8b6616e5746bed6cf63365.gif)

但是这样并不是期望的效果。预期应当右半环到达180°的位置后，左半环才开始旋转。

```css
.right-circular{
  animation: rightCircular 3s linear infinite;
}
.left-circular{
  animation: leftCircular 3s linear infinite;
}
@keyframes rightCircular {
  0%,50% {
    -webkit-transform: rotate(-45deg);
  }
  100% {
    -webkit-transform: rotate(135deg);
  }
}
@keyframes leftCircular {
  0%,50% {
    -webkit-transform: rotate(-45deg);
  }
  100% {
    -webkit-transform: rotate(135deg);
  }
}
```
为什么最终旋转角度是135°？

因为该进度条的原理就是把左边的半环旋转到右边，右边的旋转到左边,刚好旋转了180°，所以最终应停在-45+180=135的位置上。

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/18/e0e6b233c1c96b64b248687bb7e60592.gif)

终于看起来像那么回事了，但是还没完。因为它现在还不是进度条呢。

```html
<div class="circular-progress">
  ...
  <em class="percent">80%</em>
</div>
```
加入进度百分比。
```css
.circular-progress {
  text-align:center;
  line-height:140px;
}
.percent{
  font-size:30px;
  font-style:normal;
}
```
接下来让圆环旋转角度和进度保持一致。

```javascript
var percent_val = parseInt(document.querySelector('.percent').innerText),
    right_circular = document.querySelector('.right-circular'),
    left_circular = document.querySelector('.left-circular'),
    rotate_reg = 0;

  if (percent_val <= 50) {
    rotate_reg = -45 + percent_val * 3.6;
    right_circular.style.cssText = "transform: rotate(" + rotate_reg + "deg)";
  } else {
    rotate_reg = -45 + percent_val * 3.6 - 180;
    right_circular.style.cssText = "transform: rotate(135deg)";
    left_circular.style.cssText = "transform: rotate(" + rotate_reg + "deg)";
  }
```
进度大小分两种情况：
**小于或等于50%**：只旋转右半环
**大于50%**：右半环、左半环都要旋转

3.6哪里来的？
圆环（360°）分两半，每半环各占50%（180°）,每百分之一分得180/50%=3.6。

最终demo：https://jsfiddle.net/allenou/mv84hqot/

> 参考文章 http://www.xiabingbao.com/css/2015/07/27/css3-animation-circle/

[蚊子]:http://www.xiabingbao.com/
[css制作loading图]:http://www.toyou.xyz/2015/12/05/css%E5%88%B6%E4%BD%9Cloading%E5%9B%BE/
