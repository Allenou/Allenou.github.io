---
title: 判断滚动条滚动方向
date: 2016-01-19
categories: 厚积
tags: js
---
项目中有时需要判断滚动条的滚动方向，下面是我的实现过程。
<!-- more -->

我的思路：获取滚动条距离顶部的两个值，一个是滚动前的，一个是滚动后的。如果第二个值大于第一个就是向下滚，否则就是向上。

思路理好了，但不一定一次就能写好,这需要不断的锻炼和积累。我开始的代码是这样的：
```javascript
var relativePosition,movePosition;
window.addEventListener('scroll', function () {
    relativePosition = document.body.scrollTop；
    movePosition = document.body.scrollTop;
    if (movePosition>relativePosition) {
        console.log('down');
    } else {
        console.log('up');
    }
});
```
这样写``relativePosition``永远是等于``movePosition``的，于是我想应该预先获取``relativePosition``。所以代码又变成这样了：
```javascript
var relativePosition=document.body.scrollTop,//change code
    movePosition;
window.addEventListener('scroll', function () {
    movePosition = document.body.scrollTop;
    if (movePosition>relativePosition) {
        console.log('down');
    } else {
        console.log('up');
    }
});
```
这样写``relativePosition``永远是不会变的，应该把每次变化后的``movePosition``赋给``relativePosition``。然后代码就变成了这样：
```javascript
var relativePosition,movePosition;
window.addEventListener('scroll', function () {
    movePosition = document.body.scrollTop;
    if (movePosition>relativePosition) {
        console.log('down');
    } else {
        console.log('up');
    }
    relativePosition=movePosition;//new code
});
```
磨刀不误砍柴工，真切体会到思路的重要性。
