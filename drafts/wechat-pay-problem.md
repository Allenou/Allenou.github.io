title: 微信支付问题
date: 2018-12-20 11:29:39
categories: 厚积
tags: js微信 iOS 版本配置 JSSDK 时对支付路径的要求是 Landing Page 。
---
iOS 微信公众号支付提示 URL 未注册。
<!--more-->
微信浏览器本质上是一个 webview ，webview 中的网页与 app 交互需要一套通讯方式。微信目前提供了 WeiXinJSBridge 和 JSSDK 两种通讯方式。我使用的是 JSSDK，因为 JSSDK [开发文档]比较完善。

使用 JSSDK 需要先配置，
项目中我需要借助 JSSDK 实现公众号支付和扫码功能，在实现支付功能的时候遇到了一个奇怪的问题。iOS 系统第一次进入项目可以在支付页面正常调起支付，在非支付页面刷新后再发起支付却提示 “当前页面 URL 未注册”，在支付页面刷新后又可以正常调起。


原来微信 iOS 版本中的单页应用，URL 不会随着路由改变而改变，其任何页面的 URL 在不刷新的情况下始终为第一次进入项目时的 URL。即使刷新后复制出来的 URL 也不会改变，但是通过 `window.location.href` 得到的 URL 却变了。

查阅了资料后，得知微信 iOS 版本第一次进入项目时的页面称为 Landing Page，在不刷新的情况下 Landing Page 
配置支付时所需要的签名 URL 是 Landing Page。


同时复制出来的链接不是当前页面的而是第一次进入项目时的链接。

> 参考文章： http://get.ftqq.com/8572.get


[开发文档]: https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115
