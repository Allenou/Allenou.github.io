---
title: 浏览器兼容
date: 2019-04-20
---
# 浏览器兼容
面试中老被问到遇到过什么兼容问题怎么解决的，这些问题老让我无话可说。一方面我移动端比较多，基本没什么兼容问题，另一方面没太在意这些问题，解决就解决了不会去特意去记录。本文就是用于记录这些兼容问题，以免下次遇到又去寻找解决方案。

## CSS Hack
IE 是最特立独行的浏览器，所以大部分的 hack 方法都是针对 IE的。常见的 hack 方法有“条件注释法”、“属性前缀法”、“选择器前缀法”。
### 条件注释法
条件注释是指使用特定的条件语法使得在条件内的样式表只能被指定的浏览器或指定的浏览器版本识别：
```html
<style>
	<!--[if IE]>
	    只在 IE 才生效
	<![endif]-->
	
    <!--[if !IE]>
	    非 IE 浏览器才生效
	<![endif]-->

	<!--[if IE 6]>
	    只在 IE6 生效
	<![endif]-->

	<!--[if !IE 6]>
	    非 IE6 版本才生效
	<![endif]-->

	<!--[if gte IE 6]>
	    只在 IE6 以上版本才生效
	<![endif]-->
</style>
```
### 选择器前缀法
选择器前缀法是在 css 选择器前添加不同的特殊符号以达到区分浏览器的目的。hack 选择器用法与常见的 id 和 类选择器用法并无区别，之所以 hack 是因为它只能被少部分浏览器识别。下面是常见的 hack 选择器：
```css
/* IE 6 */
* html .ie6 {
	property:value;
}
/* IE 7 */
*+html .ie7 {
	property:value;
}
/* IE 6 and 7 */
@media screen\9 {
  .ie67 {
		property:value;
	}
}
/* IE 6, 7 and 8 */
@media \0screen\,screen\9 {
	.ie678 {
		property:value;
	}
}
/* IE 8 */
html>/**/body .ie8 {
	property:value;
}

@media \0screen {
	.ie8 {
		property:value;
	}
}
/* IE9 */
@media screen and (min-width:0\0) and (min-resolution: .001dpcm) {
	.ie9{
		property:value;
		}
}
/* IE9+ */
@media screen and (min-width:0\0) and (min-resolution: +72dpi) {
	.ie9up{
		property:value;
	}
}
/* IE 8,9 and 10 */
@media screen\0 {
	.ie8910 {
		property:value;
	}
}
/* IE10+ */
@media screen and (-ms-high-contrast: active),(-ms-high-contrast: none){
    .ie10up {
		property:value;
	}
}
```

### 属性前缀法
属性前缀法和选择器前缀法差不多，区别在于属性前缀法是将特殊符号添加在 css 属性前。下面是针对 IE 不同版本的 hack：
```css
.hack{
	property:value\0; /* ie 8/9*/
	property:value\9\0; /* ie 9*/
	*property:value; /* ie 7*/
	_property:value; /* ie 6*/
	property /*\**/: value\9; /* IE 8 Standards Mode Only */
}
```
在 CSS3 发展初期，各浏览器对 CSS3 的实现也需要添加前缀，这些前缀与上面针对 IE 的前缀不一样，CSS3 的前缀是浏览器内核前缀，下面是主流浏览器内核前缀：
- -moz-： IE 
- -ms-：Firefox
- -o-：Opera
- -webkit-：Chrome,Safari
::: tip
Chrome 28.0 版本后采用了 blink 引擎，bink 引擎是基于 webkit 引擎开发的，所以 webkit 前缀仍能在 blink 生效。  
:::
```css
.box {
    -moz-border-radius: 10px; /* IE */
    -ms-border-radius: 10px; /* Firefox */
    -o-border-radius: 10px; /* Opera */
    -webkit-border-radius: 10px; /* Chrome,Safari */
    border-radius: 10px; 
}
```
在 WEB 工程化时代，已经不需要手动添加内核前缀了，这些都可以通过 CSS 处理器完成，[autoprefixer] 是比较成熟的方案。


## JS Hack
<!-- 随着各浏览器渲染规范的统一，上面的大部分 hack 已没有掌握的必要，工作中基本已经用不着这些上古技巧了。 -->