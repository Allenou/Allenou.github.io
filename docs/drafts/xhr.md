---
title: XMLHttpRequest
date: 2019-04-23
---
# XMLHttpRequest 
- 描述 XMLHttpRequest 的使用过程（2017-02-24）

浏览器的 XMLHttpRequest API 是实现 Ajax（Asynchronous JavaScript and XML） 技术的核心，发起一个 Ajax 请求有以下几个过程：
<!-- IE5 是第一款引入 XMLHttpRequest 对象的浏览器，后来其它浏览器也提供了相同的实现。由于那时并没有相关的规范，所以各浏览器的实现标准并不一致，单 IE 自己就有好几个标准，本文只讲述标准的 XMLHttpRequest 对象。 -->

## 实例化
实例化 XMLHttpRequest：
```javascript
var xhr =  new XMLHttpRequest(); 
```
XMLHttpRequest 实例对象有如下属性和方法：

<Picture name="0.png"></Picture>

## 监听状态（POST）
XMLHttpRequest 的 onreadystatechange 方法会在 XMLHttpRequest 状态改变时触发，开发者们可以通过 onreadystatechange 方法监听 XMLHttpRequest 的状态进行相应操作：
```javascript
xhr.onreadystatechange = function(){
　　　　if ( xhr.readyState == 4 && xhr.status == 200 ) {
　　　　　　alert( xhr.responseText );
　　　　} else {
　　　　　　alert( xhr.statusText );
　　　　}
};
```
XMLHttpRequest 对象有以下几种状态：
- 0：未初始化。还未调用 open () 方法。
- 1：启动。已经调用 open() 方法，但尚未调用 send 方法。
- 2：发送。已经调用 send 方法，但未接收到响应。
- 3：接收。已经接收到部分响应数据。
- 4：完成。已经接收到全部响应数据。

## 初始化请求
发送请求前应先确定请求细节（初始化请求）：
```javascript
xhr.open(method, url, async, user, password);
```
open 方法可接收 5 个参数，前面两个为必要参数。
- method：请求类型（'get'、'post'）
- url：请求地址
- async：是否异步，默认true。
- user：用户名
- password：密码

## 发送请求
若为 GET 请求则发送请求时 send 方法无需携带数据：
```javascript
xhr.send();
```
若为 POST 请求则发送请求时 send 方法需携带数据：
```javascript
xhr.send();
```

[文档]: https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest


https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/send
https://www.cnblogs.com/xiaohuochai/p/6036475.html
https://wangdoc.com/javascript/bom/xmlhttprequest.html