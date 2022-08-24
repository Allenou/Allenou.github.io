title: HTTP
date: 2019-01-12 21:02:14
categories: 厚积
tags:
  - browser
---
Web 内容存储在服务器，客户端获取必须通过 HTTP 协议。

HTTP 是一种分布式，协作式和超媒体信息系统的应用层协议。

超文本传输协议从诞生到至今已经升级了几个版本，这几个版本大部分兼容。客户端在请求的开始告诉服务器它采用的协议版本号，而后者则在响应中采用相同或者更早的协议版本。

## HTTP/0.9

1991 年发布的 0.9 版是 HTTP 的第一个版本，该版本无 HTTP Header（不携带版本号，无响应状态码），默认使用 80 端口，只有一个 GET 方法 :
```
GET /index.html
```
只能用于传输 HTML 文本:
```html
<html>
  <body>Hello World</body>
</html>
```
无法保持连接，服务器响应完成后就关闭 TCP 连接。

## HTTP/1.0

1996 年 5 月发布的 1.0 版在 0.9 版基础上进行了完善，每次通信必须包含 HTTP Header，另外还新增了两个请求方法（HEAD 和 POST）。传输内容格式也大大得到丰富，支持传输文字、图像、视频、二进制文件。

现在的 HTTP Header 内容在 1.0 版本已初步形成：

```
// Request Header
GET / HTTP/1.0
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_5)
Accept: */*

// Response Header
HTTP/1.0 200 OK 
Content-Type: text/plain
Content-Length: 137582
Expires: Thu, 05 Dec 1997 16:00:00 GMT
Last-Modified: Wed, 5 August 1996 15:55:28 GMT
Server: Apache 0.84


```

<!--more-->
https://blog.csdn.net/ccpat/article/details/79413433