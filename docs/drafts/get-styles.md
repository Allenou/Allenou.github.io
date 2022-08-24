---
title: JavaScript 学习笔记：获取样式
date: 2016-03-13
categories: 笔记
tags: js
---
使用原生 js 获取元素样式的方式比使用内库什么的有趣多了。
<!--more-->

### 方式一：style

```javascript
element.style.backgroundColor;
```
只能获取指定的内联样式。

### 方式二：[currentStyle]
各浏览器支持情况：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/23/a852bfcad31a05ef20c63ab607387a37.png)


```javascript
element.currentStyle.backgroundColor;
```
也只能获取指定的内联样式。

### 方式三：cssText
> 语法：element.style.cssText;

可以获取全部的内联样式。获取的是原原本本的样式，即未经计算的样式。如：设置的是``width:20rem;``，则返回的也是``20rem``。

在不同ie版本中使用，得到的结果不一样，以下是测试结果：

**IE6/7/8**
1. 所有的属性名都为大写
2. 复合属性全部展开
3. 最末尾没有分号

**IE9+**
1. 属性名小写显示
2. 复合属性简写显示
3. 最末尾有分号(就算内联样式中没有给分号)

### 方式四：[getAttribute]
```javascript
 element.getAttribute('style');
```
通过``getAttribute``方法，获得``style``属性的值，即内联样式。

以下是在不同版本ie的测试结果：
**ie7**
获取到的是一个Object对象。

**ie8**
1. 所有的属性名都为大写
2. 复合属性全部展开
3. 最末尾没有分号

**ie9**
1. 属性名小写显示
2. 复合属性简写显示
3. 最末尾有分号(就算内联样式中没有给分号)

这样的结果是不是很熟悉，没错和``cssText``差不多的家伙。

对于获取特定的属性值，IE 还有另外一种自己独有的获取方式。如下：
```javascript
element.style.getAttribute('background-color');
element.style.getAttribute('backgroundColor');
element.style.getAttribute('backgroundcolor');
```
这种方式ie5都支持。

### 方式五：[getComputedStyle]
各浏览器支持情况如下：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/22/2a623c45cf4fbe86ba9c0b724a53a4d0.png)

``getComputedStyle``方法可以获取所有的有效样式(内联样式、内嵌样式、外部样式)。
> 语法：window.getComputedStyle(element, [pseudoElt]);

1. element：要获取样式的元素
2. pseudoElt：该元素的伪元素

在Gecko2.0 (Firefox 4 / Thunderbird 3.3 / SeaMonkey 2.1)之前版本，参数``pseudoElt``是必要的,不获取伪类样式也要填``null``。

因为获取到的是作用于该元素的所有属性对象，所以即便没有设置相关的css，也会将默认作用于该元素上的样式全部获取出来，并且获取到的颜色的值均为rgb的形式。如下：（chrome49）

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/22/d914f2f45890b0b710ff3830494d9c8b.png)

**获取伪类样式：**
```html
<style>
#king::before {
  content: '';
  width: 50px;
  height: 50px;
}
</style>
<div id="king"></div>
<script>
var king = document.getElementById('king'),
    style =  window.getComputedStyle(king, ':before');
console.log('width:' + style.width,'height:' + style.height);
</script>
```
之前说过``getComputedStyle``方法获取到的是所有的有效样式，因为伪类不是块级元素，所以设置的宽高都无效。如下：（chrome49）

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/22/f0fe32e1a4fb93d90aa1bfdb5506615c.png)

MDN说ie家族只有ie11才支持使用``getComputedStyle``方法获取伪类属性样式：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/22/889e10fb1370fecc4900a4196cd3a1cf.png)

但是我试了试ie9也是支持的。但支持情况实在让人汗颜（ie11也是如此）。
1. 不管设置的宽高是不是有效样式，只要设置了就能获取到。
2. 很多样式即使设置了也获取不到（如：margin、border、border-radius、background...）

有一点令我费解的是，获取到的border值有时比实际设置的要小。如下：（chrome49）
1px->0.8px
2px->1.6px
3px->2.4px
4px->4px
5px->4.8px
6px->5.6px
7px->6.4px
8px->8px
...
相信大家已经发现规律了。
但是使用**toggle device mode**模式查看却又是正常的，这里实在不解，望路过高人指点。

**defaultView：**
>语法：document.defaultView.getComputedStyle(element, [pseudoElt]);

虽然``document.defaultView ``对象也可以调用``getComputedStyle``方法，但是一般不这么调用了，因为``window``对象就可以直接调用。不过在firefox3.6上访问子框架内的样式 (iframe)就不得不用``defaultView``了。

### 方式六：[getPropertyValue]
>语法：window.getComputedStyle(element, [pseudoElt]).getPropertyValue(propName);

当要获取复合属性时，如果使用的是下面的获取方式是不能获取成功的:
```javascript
window.getComputedStyle(element, null).background-color;
```
除非属性名写成驼峰形式：
```javascript
window.getComputedStyle(element, null).backgroundColor;
```
不过使用了``getPropertyValue``后，属性名就可以保持原样了:
```javascript
window.getComputedStyle(element, null).getPropertyValue('background-color');
```
下面的方式也可以调用``getPropertyValue``方法哦。
```javascript
element.style.getPropertyValue(propName);
```

### 方式七：[getDefaultComputedStyle]
> 语法：window.getDefaultComputedStyle(element[, pseudoElt]);

``getDefaultComputedStyle``用来获取属性的默认值。
```html
<div id="king" style="position:absolute"></div>
<div id="style"></div>
<script>
  var king = document.getElementById("king");
  var val = window.getDefaultComputedStyle(king).position;
  document.getElementById("style").innerHTML =val //static
</script>
```
目前只有Firefox支持该方法：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/03/23/a12b0ee3a3e1f4c67e29aef54dfb53fc.png)

### 方式八：[getPropertyCSSValue]
> 语法：window.getComputedStyle(element, null).getPropertyCSSValue(propName);
element.style.getPropertyCSSValue(propName);

浏览器们对``getPropertyCSSValue``的支持情况不是很好。
Opera虽然支持，但是总是引发异常，Firefox则总是返回``null``，就safari还好点。鉴于这样的支持情况，不建议铤而走险。

> 参考文章
http://www.cnblogs.com/snandy/archive/2011/03/11/1980545.html
https://developer.mozilla.org/zh-CN/docs/Web/API/Window/getComputedStyle
http://www.zhangxinxu.com/wordpress/2012/05/getcomputedstyle-js-getpropertyvalue-currentstyle/

> 该文章仅为个人学习笔记，并非教程，如有不对之处还请及时指出。


[currentStyle]: https://msdn.microsoft.com/en-us/library/ms535231(v=vs.85).aspx
[getAttribute]: https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getAttribute
[getComputedStyle]: https://developer.mozilla.org/zh-CN/docs/Web/API/Window/getComputedStyle
[getDefaultComputedStyle]: https://developer.mozilla.org/en-US/docs/Web/API/window/getDefaultComputedStyle
[getPropertyCSSValue]:http://help.dottoro.com/ljjglctt.php
http://www.zhangxinxu.com/wordpress/2012/05/getcomputedstyle-js-getpropertyvalue-currentstyle/