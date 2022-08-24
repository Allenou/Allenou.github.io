---
title: HTTP
---
# HTTP

- 了解过 HTTP2吗？（2019-04-10）

互联网网络分层架构中比较著名的是 OSI 分层 和 TCP/IP分层，这里我使用传播范围更广的 TCP/IP分层架构进行讲解。
![](/better/browser/http.jpg)

## TCP

HTTP（HyperText Transfer Protocol）是建立在 TCP 连接（传输层）之上的通信协议（应用层）。客户端向服务器发起一个 HTTP 请求需要经过三次握手先与服务器建立一个 TCP 连接，然后在这个连接的基础上客户端才能与服务器通信。

建立一个 TCP 连接，客户端会与服务器进行三次握手:
![](/better/browser/http/0.jpg)
这三次握手主要是为了确立客户端和服务器都能收到对方的响应。一个 TCP 连接上可以发起多次 HTTP 请求。

## HTTP0.9
作为 HTTP 的第一个版本，它只有一个 GET 命令,默认使用 80 端口，服务器只能响应 HTML 页面。客户端和服务器之间不能保持连接，请求响应完成后连接就被关闭。没有 HEADER 、状态码，所以也没有办法区分错误消息和正常的文本。



## HTTP1.0

HTTP/1.0在HTTP/0.9的基础上做了大量的扩充和改进。增加了请求头域和响应头域、状态码，增加了HEAD和POST方法，响应对象不再局限于HTML文本，缓存机制等等

## HTTP1.1

HTTP1.1 支持持久连接，TCP 连接不等同与 HTTP 连接，一个TCP连接可以有多个HTTP连接，pipeline,

有序



web 服务器和应用服务器

## HTTP2

无序

- 头部压缩，节省带宽
- 无需要按顺序返回
- 服务端主动推送


## HTTP3






## HTTPS 
http://www.runoob.com/w3cnote/http-vs-https.html
https://developers.weixin.qq.com/community/develop/article/doc/000046a5fdc7802a15f7508b556413




CSP

NGINX 代理缓存
http://javadocs.wikidot.com/hypertext-transfer-protocol-http1-1