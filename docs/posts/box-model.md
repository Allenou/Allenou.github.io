---
title: 盒模型
date: 2019-04-19
---
- IE 盒模型和标准盒模型有哪些区别？
- 什么是盒模型？
<!-- more -->

盒模型是浏览器布局的基础，它由以下几部分组成：


<Picture name="0.gif"></Picture>

常见的盒模型有 IE 盒模型和标准盒模型，它们的区别在于宽高的计算方式不同。

IE 盒模型的宽高由元素的 content、padding 和 border 共同决定，而标准盒模型的宽高只由元素的 content 决定：

<Picture name="1.gif"></Picture>

盒模型的计算规则决定了元素的尺寸，盒模型计算方式的不统一会导致不同浏览器渲染结果不同（怪异模式）。由于浏览器的盒模型计算规则不统一，使得网页开发者们不得不针对不同浏览器编写不同的样式， `box-sizing` 的出现则化解了这种尴尬。

## box-sizing
`box-sizing` 是 CSS3 才出现的用于设置浏览器采用哪种盒模型规则的属性：

<Picture name="3.png"></Picture>

`box-sizing` 有以下几个属性：
- content-box (W3C 标准盒模型)
- border-box (IE 盒模型)
- padding-box：(Firefox 盒模型) 

<Picture name="4.png"></Picture>

`content-box` 和 `border-box` 前面已说明，`padding-box` 表示元素的尺寸由 content、padding 决定，
 需要注意的是 Firefox 50 版本以上已不支持 `padding-box`，请谨慎使用：

<Picture name="5.png"></Picture>

提到 IE 容易想到落伍，但其实 IE 盒模型更受欢迎，因为 IE 盒模型对于开发者更直观:

<template>
  <iframe  frameborder="0" height="380" src="https://interactive-examples.mdn.mozilla.net/pages/css/box-sizing.html" title="MDN Web Docs Interactive Example" width="100%"></iframe>
</template>

<!-- ::: tip
IE 盒模型存在于 IE5.5 以下版本，IE6 以上版本使用的是标准盒模型。
::: -->
<!-- 
```css
*, *:before, *:after {
  box-sizing: border-box;
}
``` -->



<!-- > 参考文章：
[IE各版本CSS Hack（兼容性處理）語法速查表]


[IE各版本CSS Hack（兼容性處理）語法速查表]: https://kknews.cc/zh-tw/education/q2993yb.html 
[autoprefixer]: https://github.com/postcss/autoprefixer
https://zhuanlan.zhihu.com/p/34407286
https://blog.csdn.net/freshlover/article/details/12132801
http://browserhacks.com/#hack-da690292d4fddd94dc7bdd50e38b5713

https://segmentfault.com/a/1190000013069516
https://zhuanlan.zhihu.com/p/24778275
https://www.jianshu.com/p/4bf0b0a9fbab?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation
https://zh.wikipedia.org/wiki/CSS_%E6%BF%BE%E5%99%A8 -->