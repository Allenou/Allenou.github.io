---
title: 超出文本显示为省略号
date: 2016-02-28
categories: 厚积
tags: css
---
css 实现单行文本和多行文本的文字截断，超出的内容显示为省略号。
<!--more-->
**单行**
```css
.single-row {
    display: block;
    white-space: nowrap;
    overflow: hidden;
    width: 50%;
    text-overflow: ellipsis;
}
```

**多行**
```css
.multi-row {
    overflow: hidden;
    display: -webkit-box;
    width: 50%;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    text-overflow: ellipsis;
}
```
``text-overflow``是css3新增的属性，语法：``text-overflow: clip|ellipsis|string``。
> clip：修剪文本，即截断文本。
  ellipsis：修剪文本，被截断的部分以省略号显示。
  string：修剪文本，被截断的部分以制定字符显示。

#### clip：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/09/ff87c8678c8e7026baff326c65da6d8b.png)

#### ellipsis：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/09/5a82aa40c066beaa9dfa71e059b66f88.png)

#### string：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/09/39fa890967d1e781dbfdf4662459377b.png)
为何？

> -webkit-line-clamp: 一个不规范的属性，没有出现在css规范草案中，只有webkit内核的浏览器支持，用于限制文本显示行数
  -webkit-box-orient：设置或检索伸缩盒对象的子元素的排列方式(vertical:是从上到下)

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/09/c5d4125bfb87030e69374079e6f0ecdc.png)



> 扩展阅读： http://www.zhangxinxu.com/wordpress/?p=230
