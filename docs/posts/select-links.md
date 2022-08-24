---
title: select 实现超链接效果
date: 2016-05-07
categories: 厚积
tags: js
---

- select 怎么设置值？
<!-- more -->

很多政府类网站中底部都有一个 select 友接，选中 option 就会跳到相应的网站，这有点类似于超链接效果。


<Picture name="0.png"></Picture>
## 失败
我喜欢用最少的代码实现效果，先试一下能否用 a 标签替换 option 实现：

```html
<select >
  <option value="#">---友情链接---</option>
  <a href="https://www.toyou.xyz">
    Better
  </a>
</select>
```
<template>
  <select >
    <option value="#">---友情链接---</option>
    <a href="https://www.toyou.xyz">
      Better
    </a>
  </select>
</template>

a 标签替换 option 实验失败，select 中的 a 未能显示，这么说 a 包裹 option 也不行，那么再试试 option 包裹 a 标签：

```html
<select >
  <option value="#">---友情链接---</option>
  <option value="https://www.toyou.xyz">
    <a href="https://www.toyou.xyz">
      Better
    </a>
  </option>
</select>
```
<template>
  <select >
    <option value="#">---友情链接---</option>
    <option value="https://www.toyou.xyz">
      <a href="https://www.toyou.xyz">
        Better
      </a>
    </option>
  </select>
</template>

虽然 option 包裹 a 标签，a 标签能够正常显示，但是不能实现跳转效果，如此以来只能借助 Javascript 实现了。

## 成功
用 Javascript 实现的思路是监听 select 的 change 方法，当值改变时通过 Javascript 重定向到目标网页。

### location
Javascript 中重定向方法有以下几种：
- window.location
- window.location.href
- window.location.assign()
- document.location
- document.location.href
- self.location
- self.location.href
- top.location
- top.location.href

```html
<select onchange="window.location.href=this.options[selectedIndex].value">
  <option value="#">---友情链接---</option>
  <option value="http://www.toyou.xyz">Better</option>
</select>
```
<template>
  <select onchange="window.location.href = this.options[selectedIndex].value">
    <option value="#">---友情链接---</option>
    <option value="https://www.toyou.xyz">Better</option>
  </select>
</template>

使用重定向的缺点在于会在本标签页打开网址，这和 a 标签的默认形式一样，要实现 a 标签的 `_blank` 效果，重定向并不适用。

### open
要想实现在新标签打开网页只能使用 `window.open`：
```html
<select onchange="window.open(this.options[selectedIndex].value)">
  <option value="#">---友情链接---</option>
  <option value="http://www.toyou.xyz">Better</option>
</select>
```
<template>
  <select onchange="window.open(this.options[selectedIndex].value)">
    <option value="#">---友情链接---</option>
    <option value="https://www.toyou.xyz">Better</option>
    <option value="https://www.lred.me">lred</option>
  </select>
</template>

::: tip
使用 `window.open` 在 chrome 中第一次会被拦截，选择允许弹出后下次就不会再拦截了。
:::

## 总结
select 实现超链接的缺点在于无法实现更好的 SEO 效果，因为大部分搜索引擎无法解析 Javascript 代码。同时也要考虑如果用户禁止 Javascript 后的后果，不过在我看来这根本不需要考虑，因为现在很大部分的网站都基于 Javascript 开发，用户已经离不开 Javascript 了。

<!-- https://bijian1013.iteye.com/blog/2168888
http://www.runoob.com/w3cnote/js-redirect-to-another-webpage.html
https://www.cnblogs.com/Janejxt/p/9240440.html
https://my.oschina.net/justdo/blog/118391 -->