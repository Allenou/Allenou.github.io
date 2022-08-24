---
title: 子页面传值给父页面
date: 2016-05-09
categories: 厚积
tags: js
---

一般项目的数据交互都是在本页面，但是当页面结构比较复杂时，就不得不分多一个页面了，如此便出现了不同页面间如何进行数据交互的问题。要如何解决这个问题呢？下面是我试过的几个方法。
<!-- more -->

## 方法一：
将子页面的值存入 **cookie** ,然后回到父页面，父页面再读取 **cookie** 的值。

至于怎么回到父页面，有以下几种方法可以选择：
1. **window.loaction**
2. **window.open**
3. **window.history.go(-1)**

其中前面两种不足之处在于不会保存父页面的数据，如表单数据及浏览位置。由此看来，还是使用第三种比较省事。但是读取 **cookie** 也是一件比较麻烦的事，咱们再看看其它方法。

## 方法二：
通过 `window.open` 方法打开子页面，然后通过 `window.opener` 方法返回对父页面 Window 对象的引用，由此实现父子页面的数据交互。用法如下：

```html
<!--parent page-->
<script>
function getCity(cityVal){
  document.querySelector("#city").value=cityVal;
}
</script>

<input type="text" id="city"  onclick="window.open('child.html');">
```
父页面预设带参的 `getCity()` 方法，用来接收子页面传过来的值。
```html
<!--child page-->
<script>
var select=document.querySelector('select'),cityVal;
function citySelect(){
  cityVal=select.value;
  window.opener.getCity(cityVal);
  window.close();//关闭子页面
}
</script>

<select id="cityList" onchange="citySelect()" value="this.selectIndex.text">
  <option>北京</option>
  <option>上海</option>
  <option>广州</option>
  <option>深圳</option>
</select>
```
子页面获得父页面对象后直接通过该对象访问父页面 `getCity()` 方法，并把值做参传入。
::: warning
`window.close()` 方法只能关闭由 `window.open()` 方法打开的页面（窗口）
:::
## 方法三：
通过 **iframe** 引入子页面：
```html
<iframe src="child.html"></iframe>

<script>
window.addEventListener('load',function(){
  var child=document.querySelector("iframe").contentWindow;//获取 **iframe** 对象
  var childSelect=child.document.querySelector('#cityList');
  var cityVal= document.querySelector("#city");

  childSelect.addEventListener('change',function(){
     cityVal.value=childSelect.value;
   });
})
</script>
```
需要注意的是，并不能直接获取 **iframe** 中子页面的元素，必须先获取 **iframe** 对象，再通过该对象访问子页面元素。各浏览器对获取 **iframe** 对象的方法及支持程度并不一样，有兴趣深入戳[这里]。

这种做法的好处就是既保持了页面的整洁性，又使得页面间的数据交互变得不再复杂。

最后，说下特殊情景：**微信**

微信不支持 `window.open()`、 `window.opener()`、 `window.close()` ，但它有自支持的 **close** 方法：
```javascript
<input type="button" value="close" onclick="WeixinJSBridge.call('closeWindow');" />
```
然而它并不是关闭当前页面而是关闭整个 **webview** ，对我并没什么卵用，所以第一种方法基本没戏。微信下唯有使用方法二了。


[这里]:http://blog.csdn.net/tomysea/article/details/6688892

>参考文章：
- [How to pass value from child window to parent window control]

[How to pass value from child window to parent window control]: http://forums.asp.net/t/1856036.aspx?How+to+pass+value+from+child+window+to+parent+window+control
