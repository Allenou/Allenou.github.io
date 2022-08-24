---
title: 'flexbox 制作骰子'
drafts: true
date: 2016-05-10
categories: 翻译
tags: css
---
你能在几分钟内能建立复杂的 ``CSS`` 布局吗？ ``Flexbox`` 是一个新的 ``CSS`` 布局规范，它可以很容易地构建动态布局。用 ``flexbox`` 实现垂直居中等复杂布局简直就是小菜一碟。

<!--more-->
说现在还不是使用 ``flexbox `` 的最佳时机，这种说法是不对的。**现在 93% 的人使用的浏览器都支持 ``flexbox``**，支持程度甚至好过于 HTML5 的 ``<video>``。

在这篇文章中，我会用基础的flexbox演示如何制作骰子面。现在，我们不考虑不同内核浏览器的支持情况、旧版的flexbox语法等令人头疼的问题。让我们马上开始这堂课。

# 第一面
一个骰子有六个面，每个面的圆点由每个面的数值决定。第一面只有一个小圆点，它在正中间。

首先是HTML。
```html
<div class="first-face">
  <span class="pip"></span>
</div>
```
为了直接进入主题，我已经给它加了基本的样式。现在这一面是这样的：

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/21/43028aed9245ddde31a19638e9b79ff5.png)

第一步是告诉浏览器，让这一面变成一个flexbox容器。

```css
.first-face {
  display: flex;
}
```

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/21/43028aed9245ddde31a19638e9b79ff5.png)

表面看起来并没有什么不同，但是在浏览器看来已经发生了变化。

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/21/ab6aa6c88918a6323b338fa35a53a92e.png)

第一面容器现在有了一条水平主轴。flexbox 容器的主轴可以水平也可以垂直，但是默认是水平的。如果我们再在这一面增加一个圆点，它会显示在第一个点的右边。这个容器也就有了一条侧轴了。侧轴是一直垂直于主轴的。

``justify-content`` 属性设置沿主轴方向对齐。既然我们想让这个圆点沿主轴居中，所以我们应该使用``center``。
```css
.first-face {
  display: flex;
  justify-content: center;
}
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/30/64798158bf741f6d83821681159cf739.png)

哇，酷。因为主轴是水平的，这个点现在已经居中于父元素了。

``align-items``属性设置元素沿着侧抽方向布局。因为我们想让这个圆点沿侧轴居中，所以我也要使用``center``。
```css
.first-face {
  display: flex;
  justify-content: center;
  align-items: center;
}
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/30/97794bde77109dbf22d3d4108874942f.png)

就这样，这个圆点就居中了。

# 玩点复杂的

在骰子的第二面，第一个圆点在左上角，第二个圆点在右下角。这用 flexbox 也是非常容易的。

同样地，先写好基本的标签和样式。
```html
<style>
.second-face {
  display: flex;
}
</style>

<div class="second-face">
  <span class="pip"></span>
  <span class="pip"></span>
</div>
```

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/07/09422b31f79a8c0dffb43ddfd62bd97f.png)
现在我们有了两个相邻的圆点。这时候想让这两个圆点彼此相对在骰子的边上。``justify-content`` 属性的 ``space-between`` 刚好可以做到这点。

``space-between`` 属性可以在应用了 ``flex`` 的元素上均匀地分配空间。因为这里只有两个圆点，所以它们会远离彼此。
```css
.second-face {
  display: flex;
  justify-content: space-between;
}
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/07/e3775ed7c8f6b7cbb283b8da3ac824cb.png)

这里我们遇到问题了。不像之前，我们不能设置成 ``align-items``，因为这会影响这两个圆点。幸运的是，flex 还有 ``align-self``。这个属性会让单个元素在 flex 容器中沿着侧轴布局。它的值我们需要设置成 ``flex-end``。
```css
.second-face {
  display: flex;
  justify-content: space-between;
}
.second-face .pip:nth-of-type(2) {
  align-self: flex-end;
}
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/07/a028e91cfe17b80329314bae24196d90.png)
看起来还不错！

# 水平和垂直一起
跳过第三面，直接讲第四面。这一面要比其它面棘手，因为我们需要两列，每列两个圆点。

关于 ``flexbox`` 有两个特性：``flex`` 容器可包含水平或者垂直的内容，并且容器之间可以嵌套。

不同于之前，我们的标签中现在含有列。

```html
<div class="fourth-face">
  <div class="column">
    <span class="pip"></span>
    <span class="pip"></span>
  </div>
  <div class="column">
    <span class="pip"></span>
    <span class="pip"></span>
  </div>
</div>
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/11/e6c658d9c2bbe6cbe2cc7b268cb92f5f.png)

因为需要将两列对立，所以可以继续用 ``justify-content: space-between`` ，就像之前那样。
```css
.fourth-face {
  display: flex;
  justify-content: space-between;
}
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/11/02b88e82c89bbbce3db4ef039643248b.png)

接下来，我们需要制作两列 ``flex`` 容器。它看起来是已经准备好了，但是请记住我们还没设置 ``display: flex`` 呀。我们可以使用 ``flex-direction`` 属性去设置主轴上的列的排序方向。
```css
.fourth-face {
  display: flex;
  justify-content: space-between;
}

.fourth-face .column {
  display: flex;
  flex-direction: column;
}
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/11/02b88e82c89bbbce3db4ef039643248b.png)

它看起来并未发生改变，但是现在这些列已经是 ``flex`` 容器了。可以在里面再创建一个 ``flex`` 容器么。当然可以！``Flexbox`` 不用担心嵌套问题。

最后一步是让列里面的圆点隔开。既然主轴是垂直的，我们可以再次使用 ``justify-content``。
```css
.fourth-face {
  display: flex;
  justify-content: space-between;
}
.fourth-face .column {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/11/e261e803c19112224bdd0bf3a949b6a7.png)


# 总结
完成了三面，还有三面。重点是，无论如何你都应该试做下另外三面。你们的家作就是用 **CSS** 完成剩下的几面。

【The End】

原文 Demo：https://codepen.io/LandonSchropp/full/KpzzGo/
原文评论区炫酷 Demo：http://codepen.io/shug0/full/XJJGeW/

> 本文根据[@Landon Schropp]的《[Getting Dicey With Flexbox]》所译，全文带有自己的理解，主要目的在于提升英语水平的同时学习新知识。鉴于本人基本靠瞎猜的英语水平，建议阅读原文，以免被误导。

[@Landon Schropp]:https://twitter.com/LandonSchropp
[Getting Dicey With Flexbox]:https://davidwalsh.name/flexbox-dice
[免费的flexbox初级课程]:https://unravelingflexbox.com/subscribe
