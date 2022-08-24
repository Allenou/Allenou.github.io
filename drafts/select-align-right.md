---
title: select 右对齐
date: 2016-03-01
categories: 厚积
tags: css
---
项目中经常有用到``select``，有时需要将其右对齐，但是用``text-align:right``在 **chrome** 并无效果(不开心，**firefox** 都有效果)。
<!--more-->
**chrome** 下可以通过以下方式实现：

**方法一：模拟select**

使用``ul``模拟。``ul``用``text-align``总不会失效了吧。但是这样做就等于统一了在不同系统上的样式展示，不过这也不算坏事吧。

**方法二：direction**

```html
<select dir="rtl"></select>
```
``dir``是direction（方向）的简写，``rtl``则是文本方向从右到左（right-to-left）的意思。所以``ltr``就很好理解了，但是因为默认就是从左到右，所以并没有什么卵用。

也可以在样式里设置：
```css
.select{
  direction: rtl;
}
```
``direction``还有另一个妙用。

大家都知道内联元素右侧显示需要用``text-align:right;``,块级元素右侧显示则需要要用``float:right;``。``text-align:right;``还好，不会破坏文档流造成布局塌陷，``float:right;``就不同了，它造成了布局塌陷，用了后还要清除浮动，实在麻烦。

但是``direction``后就不用考虑什么内联和块级了，它都能起效果。它和``text-align``一样，需要在父级设置起作用，不像``float``自己设置也能有效。需要注意的是，如果有同级元素存在，就不适合使用``direction``了,否则两元素会互换位置。不过这也要看需求啦，如果需求要求互换呢，那使用``direction``就再适合不过了。

>-------2016-05-08-------

有时候也需要将 **select** 居中对齐，虽然没有 `direction:center` 这一写法，但是我们可以用 `text-align-last: right` 实现。

不过相比 `direction` ，`text-align-last` 只能对齐未展开的之前的 **select**，像这样：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/05/08/bcd6d3e3517d6c01c4d908f4c7008476.png)

展开后就会发现 **option** 并没有对齐，像这样：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/05/08/23e0dc7061fecbc9a1533bbab24e01f2.png)

这也算解决了部分需求吧，毕竟 `text-align-last` 本就不是为 **select** 设计的。

>相关文章：http://www.zhangxinxu.com/wordpress/2016/03/css-direction-introduction-apply/
