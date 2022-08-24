---
title: flexible.js 原理
date: 2019-04-24
---
- flexible.js 实现原理。
<!-- more -->

flexible.js 2.0 分支的[源码]并不多，总共不到 50 行，下面先分析每段代码的用处：

::: warning
flexible.js 官方已不在维护，目前推行 [vw] 适配方案，本文只是为了分析它的原理。
:::
```javascript
(function flexible (window, document)){}(window, document))
```
上面代码是一个 IIFE（立即执行）函数，当浏览器解析完这个函数后会立即执行。使用 IIFE 的原因在于隔离作用域，不至于污染全局作用域。IIEF 传入 window 和 document 参数可以减少作用域的查找，同时有也利于代码混淆。关于 IIFE 的更多用处请浏览 [IIFE详解]。

```javascript
var docEl = document.documentElement
```
获取文档的根节点（html 标签）。

```javascript
var dpr = window.devicePixelRatio || 1
```
上面的代码是获取设备的 DPR，如未获取到默认设为 1。DPR 指的是设备物理像素和设备独立像素的比例，DPR >= 2 以上的为视网膜设备。`window.devicePixelRatio` 的支持情况请在 [can i use] 查看。

```javascript
function setBodyFontSize () {
    if (document.body) {
      document.body.style.fontSize = (12 * dpr) + 'px'
    }
    else {
      document.addEventListener('DOMContentLoaded', setBodyFontSize)
    }
}
```
上面的代码从函数名可以看出是用于设置 body 字体大小,这个函数会在 DOM 加载完成后调用，body 字体大小会被设置为 12 倍 DPR 像素。不过个人感觉下面的写法更简洁：
```javascript
function setBodyFontSize () {
    document.addEventListener('DOMContentLoaded', function(){
        document.body.style.fontSize = (12 * dpr) + 'px'
    })
}
```
`DOMContentLoaded` 事件会在 DOM 内容加载完毕时触发，无需等待文档中的外部资源（image、video）载入。关于页面生命周期请浏览[页面生命周期]。

::: warning
从 [issues] 看来设置 body size 似乎会造成怪异现象，我之前也确实出现过字体在一瞬间从大变小的问题。关于这段代码的用途请看官方的[解释]。
:::

```javascript
function setRemUnit () {
    var rem = docEl.clientWidth / 10
    docEl.style.fontSize = rem + 'px'
}
```
上面的代码是将根节点的字体大小设置为浏览器可视区域宽度的 10%，也就是相当于 1rem 的大小。

```javascript
window.addEventListener('resize', setRemUnit)
```
上面的代码会当浏览器窗口大小改变时重新设置根节点的字体大小。

```javascript
window.addEventListener('pageshow', function (e) {
    if (e.persisted) {
      setRemUnit()
    }
})
```
上面的代码是会在浏览器前进后退到当前网页时触发，这样做的目的在于解决浏览器的“往返缓存”问题。关于浏览器的往返缓存请浏览 [浏览器前进/后退缓存（BF Cache）]。

```javascript
if (dpr >= 2) {
    var fakeBody = document.createElement('body')
    var testElement = document.createElement('div')
    testElement.style.border = '.5px solid transparent'
    fakeBody.appendChild(testElement)
    docEl.appendChild(fakeBody)
    if (testElement.offsetHeight === 1) {
      docEl.classList.add('hairlines')
    }
    docEl.removeChild(fakeBody)
}
```
上面的代码用于检测分辨率大于等于 2 的设备中的浏览器是否支持 0.5 px，如果支持则在 html 标签在添加 `hairlines` 类。其思路是通过设置一个 div 元素的 border 为 0.5px,然后获取这个 div 元素的高度，如果高度为 1 则表示当前浏览器支持 0.5px 写法。这段代码的是为了编写 1px 的兼容代码：
```css
.hairlines .bb{
  border-bottom:0.5px !important;
}
```

## 总结
从上面的分析可以看出 flexible.js 主要是通过动态辨别设备的 DPR 设置根节点的字体大小，并搭配能跟随根节点字体大小而改变大小的 rem 单位来实现在不同终端适配的。

[vw]: https://www.w3cplus.com/css/vw-for-layout.html
[源码]: https://github.com/amfe/lib-flexible/blob/2.0/index.js
[页面生命周期]: https://github.com/fi3ework/blog/issues/3
[浏览器前进/后退缓存（BF Cache）]: https://harttle.land/2017/03/12/backward-forward-cache.html
[IIFE详解]: https://suqing.iteye.com/blog/2039010
[can i use]: https://caniuse.com/#search=devicePixelRatio
[解释]: https://github.com/amfe/lib-flexible/issues/163#issuecomment-360340691
[issues]: https://github.com/amfe/lib-flexible/issues/163