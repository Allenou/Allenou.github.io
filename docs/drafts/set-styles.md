---
title: JavaScript 学习笔记：设置样式
date: 2016-03-18
categories: 笔记
tags: js
---
之前的《[JavaScript学习笔记：获取样式]》说了多种使用原生 js 获取元素样式的方式，其实这几种种方式中有的不仅能获取还能设置。
<!--more-->
今天就来看看怎么使用原生 js 设置元素样式。

### 方式一：style

```javascript
element.style.width=100+"px";
element.style.width="100px";
element.style.backgroundColor="lightgreen";
```
设置内联样式。
设置复合属性中单个属性的值时，属性名应以驼峰形式书写。

如想同时设置多个属性，还可以这样写：
```javascript
element.style="wdith:100px;height:100px;border:1px solid lightgreen";
```
不过这种写法在ie中无效，即使是ie11。



### 方式二：currentStyle

```javascript
element.currentStyle.backgroundColor="lightgreen";
```
只能为指定的单个属性赋值。
>注意：非标准特性，只有IE和Opera支持。

### 方式三：cssText

```javascript
element.style.cssText="width:100px;height:100px;";
```
可以多个样式属性同时设置，相当于是在写内联样式。

在ie中设置生效后的属性顺序并非是设置时书写的顺序。如设置时是这样的顺序：
```javascript
element.style.cssText="width:200px;height:200px;padding:100px;margin:100px;";
```
生效后就变成这样了：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/25/ab932eb2ebb072784067f5c02a37713e.png)

在ie8之前的版本会被拆分开来。但是令我好奇的是``padding``被拆分了，``margin``却没有拆分....

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/25/24e19986a5b9f5ba9f839fe437dac7a9.png)

经测试发现，直接在标签中设置以及使用其它方法设置均如此。
>提示：当前设置的值会覆盖之前设置的值。

### 方式四：[setAttribute]

```javascript
 element.setAttribute('style','width:100px;height:100px;');
```
设置标签属性``style``的值，可以同时设置多个样式属性的值，也相当于是在写内联样式。
>提示:当前设置的值会覆盖之前设置的值。

### 方式五：createElement
通过创建``style``标签，设置``innerHTML``来达到设置样式的目的。如下：
```javascript
var styleTag=document.createElement('style');
styleTag.innerHTML="#element{width:100px;height:100px;background:lightgreen;}"
document.head.appendChild(styleTag);
```

或者创建``link``标签，将``href``属性值改为外部样式表所在路径，以此引入外部样式表。如下：
```javascript
var linkTag = document.createElement('link');
linkTag.href = "./css/master.css"
document.head.appendChild(linkTag);
```
>提示：ie9-版本不支持``createElement``方法。

### 方式六：[styleSheet]
浏览器提供了与样式表直接交互的对象——``styleSheet ``，可以通过该对象直接操作样式表。如下：
```javascript
var styleTag = document.getElementsByTagName ("style")[0],
    sheet = styleTag.sheet ? styleTag.sheet : styleTag.styleSheet,
    rules = sheet.cssRules ? sheet.cssRules : sheet.rules,
    firstRule = rules[0];
    firstRule.style.fontSize = "30px";
```
因为ie9之前的版本不支持``sheet``对象和``cssRules``方法，而是支持``styleSheet``对象和``rules``方法，因此要注意兼容处理。

``rules[0]``选择的是第一个样式规则，就和选择数组的第一个元素一样，如果数组为空是会报错的。
``styleSheet ``只是一个对象，它还提供了[其它方法]操作样式，这里不再记录。


> 相关文章  
http://javascript.info/tutorial/styles-and-classes-getcomputedstyle
http://www.cnblogs.com/snandy/archive/2011/03/12/1980444.html
https://developer.mozilla.org/en-US/docs/Web/API/CSS_Object_Model/Using_dynamic_styling_information

> 该文章仅为个人学习笔记，并非教程，如有不对之处还请及时指出。

[JavaScript学习笔记：获取样式]:http://www.toyou.xyz/2016/03/13/get-styles/
[setAttribute]:https://developer.mozilla.org/zh-CN/docs/Web/API/Element/setAttribute
[styleSheet]:http://help.dottoro.com/ljpatulu.php
[其它方法]:http://help.dottoro.com/ljpatulu.php
