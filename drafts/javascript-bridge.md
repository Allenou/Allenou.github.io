---
title: javascript bridge
---
# javascript bridge
- WebView 如何和 Native 通信?(2019-04-11)

Hybird App 中，网页要与 native 通信需要 native 在 webview 中暴露相应的交互方法。

## iOS
iOS 目前有 UIWebView 和 WKWebView 两中类型，iOS 8 后出的 WKWebView 相比之前的 UIWebView 性能更好，本文也主要讲解 WKWebView。

JavaScriptCore 是 iOS 中 native 和 webview 通信的桥梁，它在 Object-c 和 swift 中都可以使用。

JS 调用 Native
其实 JS 调用 iOS Native 也分为两种实现方式：

假 Request 方法(定义schema)
JavaScriptCore 方法

## Android

对于Android调用JS代码的方法有2种： 
1. 通过WebView的loadUrl（） 
2. 通过WebView的evaluateJavascript（）

对于JS调用Android代码的方法有3种： 
1. 通过WebView的addJavascriptInterface（）进行对象映射 
2. 通过 WebViewClient 的shouldOverrideUrlLoading ()方法回调拦截 url 
3. 通过 WebChromeClient 的onJsAlert()、onJsConfirm()、onJsPrompt（）方法回调拦截JS对话框alert()、confirm()、prompt（） 消息

https://zhuanlan.zhihu.com/p/31368159