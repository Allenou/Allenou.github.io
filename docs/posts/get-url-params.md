---
title: 获取 URL 参数
date: 2019-06-15
---

在 URL 中传参是很常见的跨页面通信方式，获取 URL 中的参数则是该方式中不可缺少的一步。
<!-- more -->
下面是获取 URL 中参数的几种方案：

## Plan A:
URL 中参数部分是 ? 号之后的部分，该方法是通过将多次针对性截取以获得相关参数和值，并将参数和值放入对象然后返回该对象：
```javascript
function searchParams(url) {
	var obj = {}
	if (typeof (url) === 'string' && url.indexOf('?') != -1) {
		var str = url.split('?')[1]
		var arr = str.indexOf('&') != -1 ? str.split('&') : str.split('?')

		if (arr.length > 0) {
			for (var i = 0; i < arr.length; i++) {
				var keys = arr[i].split('=')
				obj[keys[0]] = keys[1]
			}
		}
	}
	return obj
}

var params = searchParams('https://www.toyou.xyz?name=allen&age=18')
parms.name // allen
params.age // 18
```

## Plan B:
该方法是通过 `URLSearchParams` 接口接收一个查询 url（即 ? 号及 ? 号之后的部分），直接返回一个参数对象：
```javascript
var params = new URLSearchParams('?name=allen&age=18')
params.get('name') // allen
params.get('age') //18
```
`URLSearchParams` 接口提供了完整的操作 URL 的方法，鉴于本文只讲获取如何参数，故这里不进行其它拓展，如有兴趣请上 [MDN] 查看相关介绍。

需要注意的是 `URLSearchParams` 目前存在一定的兼容性问题，如果要兼容低版本浏览器还是要慎重考虑。

<Picture name="0.png"></Picture>

其实完全可以自己整一个有相同 API 的 polyfill，当然也可以使用别人的  polyfill，这里推荐 [url-search-params-polyfill]。

[MDN]: https://developer.mozilla.org/zh-CN/docs/Web/API/URLSearchParams
[url-search-params-polyfill]: https://github.com/jerrybendy/url-search-params-polyfill/
