---
title: 1px 问题
date: 2015-12-08
categories: 厚积
tags:
  - mobile
  - css
---
移动端 Retina 屏网页中 border 为 1px 时，实际渲染出来并非1px，dpr 为 2 的 Retina 屏显示为 2px，dpr 为 3 时显示为 3px。
<!-- more -->

UI 之前跟我反应过这个问题，我忽悠着说已经是最细了，然后她只是让我把颜色调淡就放过我了。今天重新来面对这个问题其实是一种自我检讨。

这其实是一个业界难题，先看几个解决方案。

### 唯品会：

![](http://7xopm5.com1.z0.glb.clouddn.com/2015/12/08/08310be4763965c32c50065fc3039f0a.png)
唯品会是通过将伪类边框缩放0.5倍实现的。

模拟了下上边框：

![](http://7xopm5.com1.z0.glb.clouddn.com/2015/12/20/90bcc2562cc5501d66c7a98c3e6e15e9.jpg)

**transform-origin: 0 0** 是什么鬼？一定要加吗？不加放大瞧瞧：

![](http://7xopm5.com1.z0.glb.clouddn.com/2015/12/20/028c7c165627d0e57819aa36083ead78.png)

加了后再放大瞧瞧：

![](http://7xopm5.com1.z0.glb.clouddn.com/2015/12/20/b293282e08e57b74c64a91c2e3e9cc37.png)

很明显的看到多了一条比较浅的线。
加与不加，缩放后的边框并不是直接在主体元素的上面，而是和主体隔了1px，类似与``margin-top:-1px``的效果。
那能不能通过``top:1px``或者``margin-top:1px``来解决呢？很可惜，试了下并不能。

优点：
* 实现单边框比较简单
* 颜色可以随心更改

缺点：
* 实现四条边框比较复杂（伪类只有:before、after...）
* 不能实现圆角（四条拼起来的线真不知道怎么用border-radius...）
* 可能引起类冲突（如果用了伪类来实现清除浮动效果的话）

### 京东：

![](http://7xopm5.com1.z0.glb.clouddn.com/2015/12/08/4c52463330e9cd6e66b8ab944dda3ee0.png)
京东是设置base64背景图实现的。
没试过，也不想试。

优点:
* 总算达到了目的

缺点:
* 加大css文件（如果编辑器自动换行的话，不知道要排多少行）
* 颜色不好随意更改

### 天猫：

![](http://7xopm5.com1.z0.glb.clouddn.com/2015/12/08/10cfdea9e73d47777ec00a29b67d9a81.png)
天猫将背景设为灰色，然后设置元素的外边距为1px，但最后一个元素的边距没有清除掉。这样做并未解决在Retina屏的显示问题，但也算是get一招。

### 淘宝：

淘宝是通过判断不同dpr下，输出不同的viewport。

**dpr=2**
![](http://7xopm5.com1.z0.glb.clouddn.com/2015/12/09/1da64336ad5cb8059fd7ab0232d54492.png)

**dpr=3**
![](http://7xopm5.com1.z0.glb.clouddn.com/2015/12/09/08b5817040853ac25d1cba32c71995f7.png)

这种做法正是现在手淘正在推行的移动端适配方案[Flexible]中的做法，相信很多人都已在各大社区见过了大漠匆忙的身影。

只想说之前怎么找都找不着，现在出现了我已无多大欲望了，以后再get吧。

除了以上方式外还有下面几种。

**background-image**
```css
.border-bottom {
    background: linear-gradient(transparent 0%, transparent 97%, #dedede 50%, #dedede 100%);
}
```
不想说话...

**box-shadow**

```css
.border-bottom{
    -webkit-box-shadow:0 1px 1px -1px rgba(0, 0, 0, 0.5);
}

```
优点：
* 代码简单

缺点：
* 颜色单调
* 有阴影（加上```transform: scaleY(0.5)```可以去除阴影，但是元素内容也会被缩放）


> 相关文章  
> http://efe.baidu.com/blog/1px-on-retina/
> http://jinlong.github.io/2015/05/24/css-retina-hairlines/


[Flexible]:http://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html?utm_source=tuicool&utm_medium=referral
