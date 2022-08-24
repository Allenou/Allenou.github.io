---
title: icon 显示不全问题
date: 2015-12-02
categories: 厚积
tags:
 - mobile
 - css
---

使用 flexible.js 时遇到过背景 icon 显示不全问题：
<!-- more -->

::: tip
图片截图于 [rem 产生的小数像素问题]。
:::

<Picture name="0.png"></Picture>

淘宝 FED 的 [rem 产生的小数像素问题] 介绍了问题产生的原因，并且给出了以下两种解决方案：
- 使用 iconfont；
- 如需使用 background-image，尽量为背景图设置一定的空白间隙

这个两个解决方案就算有效局限性也太高。首先实际项目中很多 icon 是无法在 [iconfont] 上找到的，又因为 iconfont 的绘制规则限制，即使是自己制作也只能做一些简单的，无法完全满足产品要求，其次设置一定的空白间隙，那么多大的间隙合适呢，是不是还得一次次去调试，还不能保证所有分辨率下适用。

无奈之下我去手淘看了下，发现实际上他们使用的是 `background-size: contain`，我在我的项目上试用后发现确实好用，所有分辨率下都没有问题：
<Picture name="1.png"></Picture>

不能单纯地拿来即用，用完即跑，就算好用也要明白其原理。``contain`` 和 ``cover`` 的区别在于在元素拥有宽高的情况下，`contain` 会将背景图像扩展至最大尺寸，只要高度或宽度任意一方达到了最大限值便不再扩大，图像能够完整显示，而 `cover` 是将背景图像铺满整个元素，图像有可能不能完整显示。

>拓展阅读：
- [background-size]
- [CSS Contain&Cover 的数学公式]

[rem 产生的小数像素问题]:https://fed.taobao.org/blog/2015/11/05/mobile-rem-problem/
[iconfont]:http://www.iconfont.cn/
[CSS Contain&Cover 的数学公式]:https://github.com/riskers/blog/issues/10
[background-size]: https://developer.mozilla.org/zh-CN/docs/Web/CSS/background-size