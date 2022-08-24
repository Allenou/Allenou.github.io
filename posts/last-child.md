---
title: 关于对 :last-child 的误解
date: 2016-01-12
categories: 厚积
tags: css
---

`:last-child` 是我使用频率最高的一个伪类，我经常用它来清除最后一个元素的 `margin` 和 `border`，令人费解的是在元素上按下鼠标右键后不知道为什么清除效果就失效了。
<!-- more -->

今天我终于知道了失效的原因，也发现了自己以前对 `:last-child` 的理解是错误的。
```css
ul li:last-child{
    margin-right:0;
}
```
上面的代码选择的不是 `ul` 的最后一个 `li` 元素，而是选择 `ul` 的最后一个并且是 `li` 的元素。

```html
<style>
ul li {
    margin-right:30px;
}
ul li:last-child{
    margin-right:0;
}
</style>
<ul>
    <li></li>
    <li></li>
    <p></p>
</ul>
```
上面代码中 `ul` 的最后一个元素不是 `li` 而是 `p`，所以对最后一个 `li` 的特殊处理是无效的。

至于为什么右键鼠标后特殊处理失效是因为我装了广告拦截插件。广告拦截插件会在当你右键鼠标后，会在你所点击的元素后面插入一个 `iframe`。
::: tip
广告拦截插件同时会隐藏类名中含有 `ad` 的元素
:::
```html
<ul>
    <li></li>
    <li></li>
    <iframe></iframe>
</ul>
```
插入后 `iframe` 最后一个元素就不是 `li` 了，所以自然就没有效果了。

那么要如何解决这种问题呢？答案是使用 `:last-of-type`。

```html
<style>
ul li {
    margin-right:30px;
}
ul li:last-of-type{
    margin-right:0;
}
</style>
<ul>
    <li></li>
    <li></li>
    <p></p>
</ul>
```
使用 `:last-of-type` 后即使 `ul` 的最后一个元素不是 `li` 仍有效果。

因为使用 `:last-child` 后如果父元素的最后一个子元素不是指定的子元素就不会有效果，而  `:last-of-type` 则是不管父元素的最后一个子元素是什么，它永远只选择父元素中指定的子元素中的最后一个。

>相关阅读：
- [The Difference Between :nth-child and :nth-of-type]


[The Difference Between :nth-child and :nth-of-type]:https://css-tricks.com/the-difference-between-nth-child-and-nth-of-type/
