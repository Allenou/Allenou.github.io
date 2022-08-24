---
title: 回流和重绘
---
# 回流和重绘
- 了解回流和重绘吗？（2019-4-10）

回流（Reflow）和重绘（Repaint）是浏览器中的渲染概念，了解它们之前先对浏览器的渲染有个基本的概念。浏览器从服务器请求到 HTML 文档后，渲染引擎会按下面的顺序进行渲染：
1. HTML 解析器将 HTML 文档解析成 DOM 树（HTML Parse）
2. CSS 解析器将样式表解析成 CSSOM（CSS Object Model）树（CSS Parse）
3. 合并 DOM 树和 CSSOM 树生成渲染树（render tree）
4. 对渲染树布局（Layout）
5. 绘制渲染树（Paint）
6. 展示（Display）

回流发生在 Layout 阶段，重绘发生在 Paint 阶段，因为重绘发生于发生于回流之后，所以重绘不一定会引起回流，但回流必定会引起重绘。回流和重绘并不是只发生在网页初次渲染时，任何对布局或元素样式的修改都会引起回流或重绘。以下是会触发回流的几种操作：
- 修改 DOM
- 使用脚本获取元素样式
- 修改元素的几何属性（宽高、边框）
- 元素内容改变
- 浏览器窗口尺寸改变

虽然回流和重绘不可避免，但是可以减少不必要的回流和和重绘来提高浏览器的渲染效率。

## 读写分离
现在的浏览器已经比较智能，有规律的读写操作会合并成一次操作：
```javascript
div.style.top = "10px";
div.style.bottom = "10px";
div.style.right = "10px";
div.style.left = "10px";
console.log(div.offsetWidth);
console.log(div.offseHeight);
console.log(div.offsetRight);
console.log(div.offsetLeft);

```
换出数据
## 统一修改
class
cssText

## 离线修改

display:none


## fraument

## CSS3 代替 JS





https://www.cnblogs.com/zichi/p/4720000.html