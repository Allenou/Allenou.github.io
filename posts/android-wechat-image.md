---
title: 安卓端微信点击图片被放大问题
date: 2019-06-09
---
最近的抽奖活动没有基于框架开发，在使用同事的安卓手机在微信调试活动页面时，不小心点到了背景图，结果图片被放大了，有点类似于点击公众号文章图片后触发的图片浏览效果。
<!-- more -->

<!-- <Picture name="0.jpg"></Picture> -->

该问题只在安卓端微信存在，解决方案如下：

## Plan A：
使用 js 阻止点击图片后的默认行为：
```javascript
$('.none-events').on('click',function(e) { 
    e.preventDefault(); 
}) 
```
该方法可以解决问题但是会导致点击 a 标签内的图片不能跳转：
```html
<a href="https://www.toyou.xyz">  <!--无法跳转-->
    <img src="https://www.toyou.xyz/images/android-wechat-image/0.jpg" alt="">
</a>
```
因为事件的触发顺序是由里向外的，若阻止了子元素的默认行为，则父元素的默认行为便不会生效。

## Plan B：
使用 css 移除图片的事件：
```css
img.none-events {
  pointer-events: none;
}
```
当元素 `pointer-events` 属性值为 none 时表示，该元素不会成为鼠标事件的 target：

```html
<style>
div{
    display:inline-block;
    padding:20px;
    background-color:blue;
    pointer-events: none; 
}
</style>
 <div onclick="alert('div')"></div>
```
<template>
    <div style="pointer-events: none;display:inline-block;padding:20px;background-color:blue; " onclick="alert('div')">
    </div>
</template>


若父元素的 `pointer-events` 属性值为 none ，但子元素的 `pointer-events` 属性值不为 none，则子元素事件有效并且会在冒泡阶段触发父元素的事件：

```html 
<style>
div{
    display:inline-block;
    padding:20px;
    background-color:blue;
    pointer-events: none; 
}
p{
    width:50px;
    height:50px;
    background-color:red;
    pointer-events: auto; 
}
</style>
 <div onclick="alert('div')">
    <p  onclick="alert('p')"></p>
</div>
```
<template>
    <div style="pointer-events: none;display:inline-block;padding:20px;background-color:blue; " onclick="alert('div')">
        <p style="pointer-events:auto;width:50px;height:50px;background-color:red;" onclick="alert('p')"></p>
    </div>
</template>

该方案虽然简单，但是也会导致 a 标签无法跳转，图片 hover 事件失效。

## 总结
两种方案各有优缺点，怎么选用还是要看具体情况。

 