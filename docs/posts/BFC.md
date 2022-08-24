---
title: BFC
date: 2017-01-18
---
 BFC（Block Formatting Context）译为“块级格式化上下文”，可能很多人对这个名词比较陌生，其实它早就是我们的老朋友了。下面一起来认识下这位最熟悉的陌生人。
<!-- more -->
## 概念
BFC 是浏览器中很重要的一个渲染概念，它拥有自己的渲染区域和渲染规则，其内部的渲结果不影响外部布局。根据 W3C 对 BFC 的描述说明 BFC 有以下特点和触发条件：
- 特点：
1. Box 从父容器顶部依次垂直排列，
2. Box 之间的垂直距离由 margin 属性决定
3. 相邻元素 Box 的垂直距离会重叠

- 触发条件：
1. float 属性值不为 none 时
2. position 属性值不为 static 或 relative 时
3. display 属性值为 table-cell, table-caption, inline-block, flex, inline-flex 其中的一个时
4. overflow 属性值不为 visible 时

从上面的特点可以发现，BFC 的特点似乎和块级元素的特点并无差别，这是因为 BFC 说的就是块级元素的渲染规则。

## 用途
面试中之所以会经常问 BFC 是因为理解 BFC 可以解决很多布局问题。下面是可以通过 BFC 解决的问题。
### 防止 margin 重叠
```html
<section>
    <p style="background-color: #bdc3c7">P1</p>
    <p style="background-color: #bdc3c7">P2</p>
</section>
```
p 在 chrome 下默认有 16px 的上下 margin，照理说下面 p 之间应该是相距 32px，然而并不是：
<template>
    <section>
        <p style="background-color: #bdc3c7">P1</p>
        <p style="background-color: #bdc3c7">P2</p>
    </section>
</template>
之所以会出现这种情况，是因为两个 p 处于同一个 BFC 容器中。根据 BFC 的定义，属于同一个 BFC 的两个相邻块级元素的 margin 会重叠。
碰到这种情况我们就可以将其中一个 p 放入新容器内，并触发该容器的 BFC 来防止重叠。触发方法上面已经说了，我比较习惯将 overflow 设置成 hidden 来触发 BFC。
```html
<section>
    <div style="overflow:hidden">
        <p style="background-color: #bdc3c7">P1</p>
    </div>
    <p style="background-color: #bdc3c7">P2</p>
</section>
```
<template>
    <section>
        <div style="overflow:hidden">
             <p style="background-color: #bdc3c7">P1</p>
        </div>
        <p style="background-color: #bdc3c7">P2</p>
    </section>
</template>
触发 BFC 后，可以看见两个 p 之间的距离明显宽了。

### 清除浮动
```html
<section style="border:2px solid #2c3e50">
    <p style="float:left;background-color: #bdc3c7">P1</p>
    <p style="float:left;background-color: #7f8c8d">P2</p>
</section>
```
使用浮动将两个 p 排在一行后出现了这种情况：

<template>
    <section style="overflow:hidden">
        <section style="border:2px solid #2c3e50">
             <p style="float:left;background-color: #bdc3c7">P1</p>
             <p style="float:left;background-color: #7f8c8d">P2</p>
        </section>
    </section>
</template>

之所以会出现这种情况是因为父元素内的子元素都使用了浮动后父元素塌陷了，变得没有了宽高。这种情况也可以通过触发父元素的 BFC 来解决：
```html
<section style="overflow:hidden;border:2px solid #2c3e50">
    <p style="float:left;background-color: #bdc3c7">P1</p>
    <p style="float:left;background-color: #7f8c8d">P2</p>
</section>
```
<template>
    <section style="overflow:hidden;border:2px solid #2c3e50">
        <p style="float:left;background-color: #bdc3c7">P1</p>
        <p style="float:left;background-color: #7f8c8d">P2</p>
    </section>
</template>
触发父元素的 BFC 后，BFC 内的渲染结果便不会再影响外部，如此便可以清除因子元素浮动带来的副作用。

### 防止文字环绕
```html
<section>
     <span style="float:left;width:50px;height:50px;background-color:#2c3e50"></span>
     <p style="background-color:#ecf0f1;">Floats, absolutely positioned elements, block containers (such as inline-blocks, table-cells, and table-captions) that are not block boxes, and block boxes with ‘overflow’ other than ‘visible’ (except when that value has been propagated to the viewport) establish new block formatting contexts for their contents.</p>
</section>
```
出现文字环绕：
<template>
   <section>
        <span style="float:left;width:50px;height:50px;background-color:#2c3e50"></span>
        <p style="background-color:#ecf0f1;">Floats, absolutely positioned elements, block containers (such as inline-blocks, table-cells, and table-captions) that are not block boxes, and block boxes with ‘overflow’ other than ‘visible’ (except when that value has been propagated to the viewport) establish new block formatting contexts for their contents.</p>
   </section>
</template>
出现文字环绕的原因在于被环绕的元素使用了浮动，使得后面的元素跟在其后面，又因为被环绕的元素的尺寸没有后面元素的尺寸大，便出现了以上文字环绕的情况。要解决这种问题可以通过触发文字元素的BFC：
```html
<section>
     <span style="float:left;width:50px;height:50px;background-color:#2c3e50"></span>
     <p style="overflow:hidden;background-color:#ecf0f1;">Floats, absolutely positioned elements, block containers (such as inline-blocks, table-cells, and table-captions) that are not block boxes, and block boxes with ‘overflow’ other than ‘visible’ (except when that value has been propagated to the viewport) establish new block formatting contexts for their contents.</p>
</section>
```
<template>
   <section>
        <span style="float:left;width:50px;height:50px;background-color:#2c3e50"></span>
        <p style="overflow:hidden;background-color:#ecf0f1;">Floats, absolutely positioned elements, block containers (such as inline-blocks, table-cells, and table-captions) that are not block boxes, and block boxes with ‘overflow’ other than ‘visible’ (except when that value has been propagated to the viewport) establish new block formatting contexts for their contents.</p>
   </section>
</template>
另一种解决办法是将图片或者文字元素的 BFC 独立：

```html
<section>
     <p>
        <span style="float:left;width:50px;height:50px;background-color:#2c3e50"></span>
     </p>
     <p style="overflow:hidden;background-color:#ecf0f1;">Floats, absolutely positioned elements, block containers (such as inline-blocks, table-cells, and table-captions) that are not block boxes, and block boxes with ‘overflow’ other than ‘visible’ (except when that value has been propagated to the viewport) establish new block formatting contexts for their contents.</p>
</section>
<!-- or -->
<section>
     <span style="float:left;width:50px;height:50px;background-color:#2c3e50"></span>
     <section>
        <p style="overflow:hidden;background-color:#ecf0f1;">Floats, absolutely positioned elements, block containers (such as inline-blocks, table-cells, and table-captions) that are not block boxes, and block boxes with ‘overflow’ other than ‘visible’ (except when that value has been propagated to the viewport) establish new block formatting contexts for their contents.</p>
     </section>
</section>
```

<template>
   <section>
        <p>
            <span style="float:left;width:50px;height:50px;background-color:#2c3e50"></span>
        </p>
        <p style="overflow:hidden;background-color:#ecf0f1;">Floats, absolutely positioned elements, block containers (such as inline-blocks, table-cells, and table-captions) that are not block boxes, and block boxes with ‘overflow’ other than ‘visible’ (except when that value has been propagated to the viewport) establish new block formatting contexts for their contents.</p>
   </section>
</template>

## 番外
之前同事遇到一个问题，后台返回的文章是经过富文本编辑的，所以如果文章里有图片， 图片有可能在 `p` 里面也有可能在 `p` 标签外面：
```html
<!-- 情况一-->
<p></p>
<img>

<!-- 情况二-->
<p>
    <img>
</p>
```
如此便不知把 `margin` 设置给 `p` 标签还是图片。同事抱怨给前端负责人后，负责人说 `p` 标签和图片都设置 `margin` 。他一说出口我就立马想到了 BFC 的 `margin` 重叠特性，所以我知道这个方案是可行的，但是当时我并没有想到水能载舟亦能覆舟，而是想到可以用 JS 解决，惭愧...

## 总结
如此看来布局中处处都有用到 BFC ，之所以以前不了解不过是因为没有概念罢了。

> 参考文章：    
- [BFC神奇背后的原理]
- [block formatting]


[block formatting]: https://www.w3.org/TR/CSS2/visuren.html#block-formatting
[BFC神奇背后的原理]: https://github.com/melonHuang/melonhuang.github.io.bak/blob/master/_posts/2014-01-10-the-magic-bfc.md


<!-- http://www.cnblogs.com/lhb25/p/inside-block-formatting-ontext.html
https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context -->



