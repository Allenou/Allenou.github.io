---
title: 你需要知道的 vertical-align
date: 2016-08-30
categories: 翻译
tags: css
---
# 你需要知道的 vertical-align
我经常需要将相邻的两个元素垂直对齐，CSS 使之变得有可能。有时候我用 `float` 实现，有时候则用 `position: absolute` ，有时候甚至会调节 margins 和 paddings。
<!--more-->
我并不太喜欢这些解决方案。floats 只能实现顶部对齐并且还得手动清除浮动，绝对定位又会让元素脱离正常文档流，使得该元素不会对其周围元素产生影响，而使用固定的 margins 和 paddings 还会破坏布局。

不过，还可以使用另一种方案：`vertical-align` ，我认为它更值得信任。好吧，在技术上使用 `vertical-align` 来布局是可以说是一种 hack 手段，因为这不是当初发明它的原因，而是为了对齐文本和元素旁边的文本。然而你也可以根据实际情况用它来灵活控制元素的对齐方式，且不一定非得知道元素尺寸。因为元素在正常文档流中，所以其它元素能够响应该元素的改变。这使得 `vertical-align` 成为有价值的选择。

## 奇怪的 Vertical-Align

但是，有时候 `vertical-align` 也是一个麻烦的家伙，你会因它而沮丧。它的工作原理也有一点神秘。列如，可能会发生，你只是对其中一个元素应用了 `vertical-align`，但是其它元素却发生了变化!我现在仍然时不时会为它而抓狂。

可惜的是，大部分的资料对这些问题研究地都不够深入，特别是我们想使用 `vertical-align` 布局时。他们的误区在于把精力都放在了一个元素中，且仅对属性进行了基本的介绍，以及在常见的情况下如何对齐，而棘手的部分并没有解决。

所以，我给自己设定的目标就是 **一次澄清 vertical-align 的所有特性** 。我会在遵循 W3C 的 CSS 规范的基础上写几个列子，结果就在这篇文章中。

那么，让我们来弄清游戏规则。

## 使用 Vertical-align 的要求
`vertical-align` 只能对内联级（[inline-level]）元素起作用，这些元素的 `display` 的属性值分别是：
* inline
* inline-block
* inline-table（本文不考虑）

**inline 元素** 基本上就是被标签所包含的文本。
**inline-block 元素** 顾名思义：具有 block 元素特征的 inline 元素。它们有 `width` 和 `height`（大小也可以根据内容多少决定）、以及 `padding` 、`border` 还有 `margin`.

内联级元素是紧邻在同一行内的，一旦它们多到超出了当前行，就会换行排列，所有的这些行都可以称为 **line box**，它们将行内的内容包裹在内，其高度根据内容大小的变化而变化。下图用红色虚线标出了 line box 的上下边界。
<Picture name="0.png"></Picture>

在这些 line box 内，`vertical-align` 属性用于对齐单个元素。**那么，要相对于什么元素来对齐呢？**

## 关于基线和边界

垂直对齐最重要的参考点就是该元素的基线，在一些情况中元素的上下边界同样也很重要。让我们来看下元素的基线和边界在哪：

**inline 元素**
<Picture name="1.png"></Picture>

上图是相邻的三行文本，行高的上下边界由红色虚线表示，文本高度由绿色虚线表示，基线由蓝色虚线表示。可以看出，图一文本的行高和文字高度一致，因此其绿色虚线和红色虚线重叠了;图二文本行高是文本大小的两倍；图三文本行高是文本大小的一半。

**inline 元素的边界** 与其行高的上下边界对齐，就算其行高小于文本高度，外边界仍是上图中的红色虚线。

**inline 元素的基线** 是上图中位于文本下面的蓝色虚线，可以认为，基线低于文本高度的一半，W3C 对其有[详细定义]。

**inline-block 元素**
<Picture name="2.png"></Picture>

由左至右：图一 inline-block 元素的内容（字母‘C’） 处于正常流；图二的 inline-block 元素加了 `overflow:hidden` ,但其仍处于正常流；图三的内容不处于正常流中（但是其内容仍有高度）。图中的红色虚线表示 margin-box 的边界，黄色表示 border，绿色标识 padding，蓝色表示内容，蓝色虚线表示基线。

**inline-block 元素的边界** 就是 [margin-box] 的上下边界，它们在图中用红色虚线表示。

**inline-block 元素的基线** 取决于其内容是否在正常流中：

* 情况一： 处于正常流，其内容的基线由最后一个元素的内容决定（如：图一）。最后一个元素的基线让我们有了标准。
* 情况二： 处于正常流，如果加了 `overflow` 属性并且其值不为 `visible`,其基线就是 margin-box 的底边界。（如：图二）
* 情况三： 不处于正常流，其基线同样也是 margin-box 的底边界（如：图三）。

**line-box**
<Picture name="3.png"></Picture>

这张图我标出了 line box 中的 text box 的上下边界(绿色虚线，关于更多请看下文)以及基线（蓝色虚线），为了突出文本元素我也给它们加了灰色背景。

从上图可知，line box 的 **上边界** 对齐于最顶部元素的上边界，**下边界** 对齐于最底部元素的下边界，它们在上图中用红色虚线表示。

**line box 的基线** 是可变的：

>W3C CSS 2.1 没有定义 line box 基线的位置。

这或许是用 `vertical-align` 时最让人困惑的地方。它表明，基线的位置在哪需要满足所有条件，比如说 `vertical-align` 属性，还有 line box 的最小高度，就像方程中的未知数。

因为 line box 的基线是看不见的，所有不太好确定它在哪，但是，你可以让它可见变得简单。只要在行头增加一个字符，比如我在上图是增加了一个 “x”。如果这个字符不以任何方式排列，那么它就在默认的基线上。

包含着基线的 line box 我们或许可以称它为 **text box**。text box 可以被认为是没有与任何元素对齐的 inline 元素，它的高度取决于父元素的文本大小，因此 text box 只包含没有格式化的文本，它在上图用绿色虚线标出。因为 text box 和基线的关系，所以当基线位置改变的时候它也改变。（边注：text box 在 W3C标准中被称为 [strut]）

简概如下：
* line box 有基线、text box 还有上下边界，它是对齐操作所发生的区域。
* inline 元素也有基线和上下边界，它们是需要被对齐的对象。

## Vertical-Align 的取值

**对齐于元素的基线相当于对齐 linx box 的基线**
<Picture name="4.png"></Picture>

* `baseline`： 元素的基线正好是 line box 的基线
* `sub`：元素的基线移动到 line box 的基线之下
* `super`：元素的基线移动到 line box 的基线之上
* `<percentage>`：元素的基线相对于 line box 的行高以百分比移动
* `<length>`：元素的基线相对于 line box 的行高以固定值移动

**对齐于元素的边界相当于对齐 linx box 的基线**
<Picture name="5.png"></Picture>

* `middle`：中线在元素的上下边界之间，且相对于 line box 的基线加上 x 文本的一半高度处对齐。

**对齐于元素的边界相当于对齐 linx box 的 text box**
<Picture name="6.png"></Picture>

也可以列出以下两种相对于 line box 的基线对齐的情况，因为文本框的位置是由基线确定的。

* `text-top`：元素的上边界对齐于 line-box 的 text box 的上边界
* `text-bottom`：元素的下边界对齐于 line-box 的 text box 的下边界

**对齐于元素的边界相当于对齐 linx box 的边界**
<Picture name="7.png"></Picture>
* `top`：元素的上边界对齐于 line-box 的上边界
* `bottom`：元素的下边界对齐于 line-box的下边界

这些[定义]来源于 W3C 规范。

## 为什么 Vertical-Align 会有这样的行为

现在来仔细看看在某些情况下的垂直对齐，并且解决可能会发生的状况。

**对齐一个图标**
一直有一个疑问：有一个图标，我想让它与一行文字居中对齐，便给图标设置了 ` vertical-align: middle`,但是看起来并没有居中对齐，如：

<Picture name="8.png"></Picture>
```html
<!-- 左图标签 -->
<span class="icon middle"></span>
Centered?

<!-- 右图标签 -->
<span class="icon middle"></span>
<span class="middle">Centered!</span>

<style type="text/css">
  .icon   { display: inline-block;}
  .middle { vertical-align: middle; }
</style>
```
在上图基础上加了辅助线后：
<Picture name="9.png"></Picture>
我们基本可以看出问题所在，左图的文本没有对齐，它一直在基线上。可以这么解释，用了 `vertical-align: middle` 后我们对齐的只是小写字母,而没有在此基础上再增加 x 高度的一半，所以文字超出了顶部。

右图，我们同样让全部文字都与中线对齐，文字的基线略低于 line box 的基线，从而达到了文字与图标对齐的效果。

## 会移动的 line box 基线

`vertical-align` 的常见问题：line box 基线受所在行的所有元素影响。假设：一个元素以某种方式对齐了，但是这种方式会改变 line box 基线位置，又因为大多垂直对齐方式（除了顶部和底部）是相对于基线的，所以就导致了该行其他元素的位置也被改变了。
>译者：所以这就是为什么未应用 `vertical-align` 的元素的位置也发生了变化的原因。

如：
* 有一个比较高的元素，其高度占满了整个 line box ，它不受 `vertical-align` 影响，因为没有多余的空间让其移动，但是 `vertical-align` 的特性会使 line box 的基线移动。左图比较高的元素使用 `text-bottom`，右图的则使用 `text-top`，可以看到基线带着比较短的元素跳上去了。

<Picture name="10.png"></Picture>

```html
<!-- 左图标签 -->
<span class="tall-box text-bottom"></span>
<span class="short-box"></span>

<!-- 右图标签 -->
<span class="tall-box text-top"></span>
<span class="short-box"></span>

<style type="text/css">
  .tall-box,
  .short-box   { display: inline-block;}
  .text-bottom { vertical-align: text-bottom; }
  .text-top    { vertical-align: text-top; }
</style>
```
相同的行为发生在当比较高的元素使用 `vertical-align` 属性的其它值时。

* 设置 `vertical-align` 值为 `bottom` (左图) 和 `top` (右图) 时同样会移动基线。这是很奇怪的，因为这种情况应该是不会影响到基线的。

<Picture name="11.png"></Picture>

```html
<!-- 左图标签 -->
<span class="tall-box bottom"></span>
<span class="short-box"></span>

<!-- 右图标签 -->
<span class="tall-box top"></span>
<span class="short-box"></span>

<style type="text/css">
  .tall-box,
  .short-box { display: inline-block;}
  .bottom    { vertical-align: bottom; }
  .top       { vertical-align: top; }
</style>
```
* 放置两个比较大的元素在同一行中，并且让它们相对于基线对齐，然后可以发现 line box 的高度变高了（左图）。再增加第三个元素，因为它的对齐方式，它没有超出 line box 的边界（中图）。如果第三个元素超出了 line box 的边界，line box 的高度和基线会再次被调整。在这个列子中，前两个元素是比较低的（右图）。
<Picture name="12.png"></Picture>

```html
<!-- 左图标签 -->
<span class="tall-box text-bottom"></span>
<span class="tall-box text-top"></span>

<!-- 中图标签 -->
<span class="tall-box text-bottom"></span>
<span class="tall-box text-top"></span>
<span class="tall-box middle"></span>

<!-- 右图标签 -->
<span class="tall-box text-bottom"></span>
<span class="tall-box text-top"></span>
<span class="tall-box text-100up"></span>

<style type="text/css">
  .tall-box    { display: inline-block; }
  .middle      { vertical-align: middle; }
  .text-top    { vertical-align: text-top; }
  .text-bottom { vertical-align: text-bottom; }
  .text-100up  { vertical-align: 100%; }
</style>
```
## 在 inline 元素之间有一个条小间隔

可以看到，如果你试着在列表的 `li` 中试着用 vertical-align

<Picture name="0.png"></Picture>

```html
<ul>
  <li class="box"></li>
  <li class="box"></li>
  <li class="box"></li>
</ul>

<style type="text/css">
  .box { display: inline-block;}
</style>
```
如你所见，列表中的元素都在基线上,基线下面有一小间隙。解决方案是什么呢？只要移动基线，比如让列表中的元素应用 `vertical-align:middle`:
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/08/25/b78abfa31c88a7291177391f0a202977.png)

```html
<ul>
  <li class="box middle"></li>
  <li class="box middle"></li>
  <li class="box middle"></li>
</ul>

<style type="text/css">
  .box    { display: inline-block; }
  .middle { vertical-align: middle; }
</style>
```
这种情况不会在 inline-block 元素内有文本时发生，因为文字将基线移上去了。

## inline 级别元素之间的间距破坏了布局

主要的问题在于 inline 元素本身，但是它是 vertical-align 起效的根本，所以还是有必要了解下它。

可以看到前面例子中列表元素之间的间距，间距来源于inline 元素之间的空白。所有 inline 元素之间都会有这么一块空白的空间。这块空间在某些情况下就是个坏事的家伙。比如你想让两个元素相邻并给它们设置了 `width:50%`（如下列子），因为间距的存在，不够空间分成两个 50%，所以就会形成两行（左图）。想要移除间距，就得移除空白（右图）。

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/08/25/1fbbe6b18ec84967db8a257d861c5258.png)
```html
<!-- 左图标签 -->
<div class="half">50% wide</div>
<div class="half">50% wide... and in next line</div>

<!-- 右图标签 -->
   <div class="half">50% wide</div>
   <div class="half">50% wide</div>

<style type="text/css">
  .half { display: inline-block;width: 50%; }
</style>
```
## Vertical-Align 揭秘

是的，就是这样，一旦你知道了规则后就会发现也不是很复杂。如果 `vertical-align` 不起作用，只要自己问这些问题：
* line box 的基线和上下边界在哪里？
* inline 元素的基线和上下边界在哪？

这几乎是所有此类问题的解决方案。


> 本文根据[@christopheraue]的《[Vertical-Align: All You Need To Know]》所译，全文带有自己的理解，主要目的在于提升英语水平的同时学习新知识。鉴于本人基本靠瞎猜的英语水平，建议阅读原文，以免被误导。

[@christopheraue]:https://twitter.com/christopheraue
[Vertical-Align: All You Need To Know]:http://christopheraue.net/2014/03/05/vertical-align/
[inline-level]:http://christopheraue.net/2014/03/05/vertical-align/
[详细定义]:https://www.w3.org/TR/CSS2/visudet.html#leading
[margin-box]:https://www.w3.org/TR/CSS2/box.html#x17
[W3C规范]:https://www.w3.org/TR/CSS2/visudet.html#line-height
[strut]:https://www.w3.org/TR/CSS2/visudet.html#strut
[定义]:https://www.w3.org/TR/CSS2/visudet.html#propdef-vertical-align
