---
title: 缓存
---
# 缓存
- 浏览器有哪些缓存行为？（2017-02-21）
- 什么时候会出现 304 状态码？（2019-04-10）

[[toc]]
当我们访问一个网站时，浏览器会默认缓存从网站所在的 Web 服务器请求到的静态资源，下次访问这个网站时就不需要再次下载这些资源。虽然这很大程度上提升了网站的加载速度，但是也造成了资源得不到及时更新的问题，所以制定正确的缓存策略就显得尤为重要。制定正确的缓存策略就需要弄明白浏览器的缓存机制，下面我们先来讲解一下浏览器的缓存机制。


我们先来看下和缓存有关的 HTTP 首部字段：
- 通用首部字段：
<Cache type="head"></Cache>

- 请求首部字段：
<Cache type="request"></Cache>

- 响应首部字段
<Cache type="response"></Cache>

- 实体首部字段
<Cache type="body"></Cache>

::: tip
通用首部字段：请求和响应报文中都有的字段。
:::
这些字段大部分是 HTTP1.1 版本中新增的，其中也有些是 HTTP1.0 版本时就有的字段，HTTP1.0 版本的首部字段在 HTTP1.1 版本已经有了不同地实现，浏览器为了兼容使用 HTTP1.0 的 Web 服务器仍旧保留了对 HTTP1.0 缓存策略的支持。
## Pragma
Pragma 只有一个 no-cache 值可设置，当它值为 no-cache 时表示客户端不得缓存任何资源，每次都得向服务器请求这些资源，即使这些资源压根就没更新过。
```
Cache-Control: max-age=86400
Pragma: no-cache
```
当 Pragma 设置不缓存而 Cache-control 设置缓存时，Pragma 的优先级要高于 Cache-control。
## Expires
Expires 和 Pragma 一样也是 HTTP1.0 定义的，其作用是设置资源的过期时间,如果设置了 Expires 则浏览器在所设置的时间之前都会不会请求服务器获取资源，而是使用本地的缓存。Pragma 和 Expires 同时存在时 Pragma 的优先级要高于 Expires。

## Last-Modified
浏览器请求 Web 服务器时，Web 服务器会在响应头中给浏览器返回一个资源的最后更改时间(GMT)：

![](/better/browser/cache/0.png)

下次访问时浏览器会在请求头的 If-Modified-Since 字段上带上这个时间去请求：

![](/better/browser/cache/1.png)

如果所带时间与服务器上资源最后修改时间一致说明资源未更新，则直接返回 304 Not Modified ，浏览器直接从缓存中读取该资源，如果不一致说明资源已经更新过，则返回 200，浏览器就要重新从服务器下载该资源：

![](/better/browser/cache/5.png)

If-Unmodified-Since

## ETag
ETag 是资源的版本签名，需要主动在服务器设置。如果服务器设置了 ETag，则响应头会返回设置好的 ETag：
![](/better/browser/cache/2.png)
ETag 有 If-None-Match 和 If-Match 两个辅助字段，这两个辅助的字段的值都为服务器返回的 ETag 值，带这两个字段时说明已经是二次请求，改请求为条件请求。
### If-None-Match：

该字段用于告知服务器，当 If-None-Match 的 ETag 值想同则直接返回 304 状态码给客户端，客户端则会结束本次请求然后从本地缓存中读取该资源，当 If-None-Match 的 ETag 值不同时返回 200 状态码，客户端则会得到该资源下载完成后才会结束本次请求。
### If-Match：

该字段用于告知服务器，
![](/better/browser/cache/3.png)

## Vary
Vary 用于源服务器告知缓存服务器针对所提供的条件进行缓存。
::: tip
缓存服务器：一般是非源服务器的第三方内容分发网络（CDN）。
:::
Vary 可设置一个或多个首部字段名，这些字段名可以是 HTTP 原有的也可以是自定义的。
- 语法：
```
Vary: *
Vary: <header-name>, <header-name>, ...
```
- 用途：

很多公司都有 PC 站点和 Mobile 站点，当两个站点网址一样时想要在缓存服务器实现不同的缓存效果，就需要与缓存服务器进行协商来达到目的，Vary 就是这个用协商者，所以它常被用来告知缓存服务器针对不同类型的终端进行有区别的缓存：
<!-- 要达到从不同类型的终端访问时渲染出相应的内容，一般是可以要通过在源服务器判断 User-Agent 来实现，当我们不仅想要在客户端进行缓存，还想要在客户端和源服务器中间的缓存服务器实现缓存时，我们就需要通过和缓存服务器进行协商来达到目的，因为缓存服务器并不像源服务器可控性那么高，Vary 就是这个用协商者，所以它常被用来告知缓存服务器针对不同类型的终端进行有区别的缓存。 -->
```
Vary: User-Agent
```
关于上面代码的具体作用请浏览[《HTTP报文头中Vary:User-Agent的作用和配置》]。

## Age
Age 字段描述的是资源缓存了多长时间，这个时间是一个以秒为单位的非负整数，它是当前时间与报文（非缓存）所创建的时间（Date）的差。
- 语法：
```
Age: <delta-seconds>
```
- 用途：
Age 主要是客户端用来判断缓存是否新鲜的一个重要途径。



## Cache-Control
cache-control 是 HTTP1.1加的通用首部字段，它有以下标准值可设置：

### max-age
- 语法：
```
Cache-Control: max-age=<seconds>
```
- 用途：
用于设置缓存的最大存储周期，超过这个时间就意味着缓存过期，需要重新到源服务器去获取。

### max-stale
- 语法：
```
Cache-Control: max-stale[=<seconds>]
```
- 用途：
----------
### min-fresh
- 语法：
```
Cache-Control: min-fresh=<seconds>
```
- 用途：

用于告知服务器在所设置的时间内不缓存任何内容，每次请求都获取最新的内容。

### no-cache
- 语法：
```
Cache-control: no-cache
```
- 用途：



### no-store
- 语法：
```
Cache-control: no-store
```
- 用途：

任何客户端及中间服务器都不得缓存内容。

### no-transform
- 语法：
```
Cache-control: no-transform
```
- 用途：
### only-if-cached
- 语法：
```
Cache-control: only-if-cached
```
- 用途：
### must-revalidate
- 语法：
```
Cache-control: must-revalidate
```
- 用途：
### public
- 语法：
```
Cache-control: public
```
- 用途：

用于告知所有对象都能够缓存，包括代理服务器及 CDN。
### private
- 语法：
```
Cache-control: private
```
- 用途：

用于告知服务器只允许客户端可缓存，任何中间服务器不得缓存。

### proxy-revalidate
- 语法：
```
Cache-control: proxy-revalidate
```
- 用途：

## 实践
迭代周期-过期时间
bugfix - 
cookie 不能缓存

[MDN]: https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control#%E6%8C%87%E4%BB%A4
[《HTTP报文头中Vary:User-Agent的作用和配置》]: http://www.maixj.net/ict/http-vary-user-agent-9017

> 参考文章：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching_FAQ
https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control
https://imweb.io/topic/5795dcb6fb312541492eda8c
https://juejin.im/entry/58ba3302570c350062124f68
https://segmentfault.com/a/1190000009970329
http://javadocs.wikidot.com/hypertext-transfer-protocol-http1-1
https://juejin.im/post/5a6c87c46fb9a01ca560b4d7