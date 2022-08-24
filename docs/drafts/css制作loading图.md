title: css制作loading图
date: 2015-12-05 16:30:14
categories: 厚积
tags: css
---
css3出来这么久了，我却还没怎么玩过。刚好今天用手机听歌的时候，发现播放器列表加载更多内容时，加载提示中的loading图挺好看的，就想这效果似乎可以用css3动画做，于是就有了这篇。
<!--more-->
因为不知道这图叫什么名，所以在网上没找到类似的图。截下静态图，好让人家知道我在说啥。

![](http://7xopm5.com1.z0.glb.clouddn.com/2015/12/05/158076735d5432ae7471fc7f589a73a3.png)

闲话少说，上码。

```html
<!--html-->
<div id="icon">
    <i class="item item-first"></i>
    <i class="item item-second"></i>
    <i class="item item-third"></i>
    <i class="item item-fourth"></i>
</div>
```
**html** 很简单，四个**i**标签代表四根小柱(请原谅我这样称呼，因为实在不知叫什么)，不同小柱用不同类名区分开。

```css
/*-------css-------*/
.item {
    display: inline-block;
    width: 4px;
    background-color: #333;
}
.item-first {
    height: 30px;
    animation: first 1s infinite ease-in-out;
}

.item-second {
    height: 5px;
    animation: second 1s infinite ease-in-out;
}
.item-third {
    height: 25px;
    animation: third 1s infinite ease-in-out;
}
.item-fourth {
    height: 30px;
    animation: fourth 1s infinite ease-in-out;
}

```
**i** 标签是内联元素，所以要转换为块级元素，否则设置的宽高无效。**inline-block** 具有内联元素和块级元素的共同特性，并且会使元素之间产生空白小间隔，刚好不用设置margin便可达到原图效果。(关于**inline**、**block**、**inline-block** 的区别请看这篇[文章])

**animation** 是**css3**动画的简写属性，用于设置以下六个属性：
* animation-name
* animation-duration
* animation-timing-function
* animation-delay
* animation-iteration-count
* animation-direction

### 属性介绍：
![](http://7xopm5.com1.z0.glb.clouddn.com/2015/12/06/864043af1421becd0f9f64d035ac33a4.png)
### 语法：
animation: name | duration | timing-function | delay | iteration-count | direction

这里我用到了：name | duration | iteration-count | timing-function

(想深入**animation**戳[这里])

静态效果图：

![](http://7xopm5.com1.z0.glb.clouddn.com/2015/12/05/39cc9f52f1221470aa77cec1cbd37f8b.png)

接下来，我们使用自定义动画让小柱子动起来。

```css
@keyframes first {
    0% {
        height: 30px;
    }
    50% {
        height: 10px;
    }
    100% {
        height: 30px;
    }
}
@keyframes second {
    0% {
        height: 5px;
    }
    50% {
        height: 40px;
    }
    100% {
        height: 5px;
    }
}
@keyframes third {
    0% {
        height: 25px;
    }
    50% {
        height: 10px;
    }
    100% {
        height: 25px;
    }
}
@keyframes fourth {
    0% {
        height: 30px;
    }
    50% {
        height: 5px;
    }
    100% {
        height: 30px;
    }
}
```
为了让动画更平滑，这里分了三个阶段，初始值和结束值一致。


最终效果图(感觉怪怪的)：

![](http://7xopm5.com1.z0.glb.clouddn.com/2015/12/05/6e2c6001a4bd919f39a4c488b24b544b.gif)

[文章]:http://www.studyofnet.com/news/334.html
[这里]:http://www.w3cplus.com/content/css3-animation/
