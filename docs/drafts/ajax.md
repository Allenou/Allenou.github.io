title: Ajax
date: 2017-01-10 08:22:03
categories: 笔记
tags: js
---
认识  。
<!--more-->
**Ajax** 即 “Asynchronous Javascript And XML”（异步 JavaScript和 XML），它的历史我就不讲了，提到它人们就会想到异步刷新，它发展到现在一共经历了以下三个阶段：

第一阶段：提出

**Ajax** 的核心是 **XMLHttpRequest** 对象，**IE5** 是第一款引入 **XMLHttpRequest** 对象的浏览器，后来其它浏览器提供商相应提供了相同的实现。由于那时并没有相关的规范，所以各浏览器的实现标准并不一致，就 IE 自己家就有三个标准（MSXML2.XMLHttp、MSXML2.XMLHttp3.0、MSXML2.XMLHttp6.0），要想在各个浏览器使用，我们需要编写一个函数：

```javascript
function createXHR() {      
    var xhr;  
    if (window.ActiveXObject)  
    {  
        var arr=["MSXML2.XMLHttp.6.0","MSXML2.XMLHttp.5.0", "MSXML2.XMLHttp.4.0",  
ML2.XMLHttp.3.0","MSXML2.XMLHttp","Microsoft.XMLHttp"];  
        for(var i=0;i<arr.length;i++) {  
            try {  
                xhr = new ActiveXObject(arr[i]);  
                return xhr;  
            }  
            catch(error) { }  
        }  
    } else {  
        try {  
            xhr=new XMLHttpRequest();  
            return xhr;  
        }  
        catch(otherError) { }  
    }   
}   
```

xmlhttprequest
第二代 AJAX
xmlhttprequest_level_2

第三代 AJAX
fetch
http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html
https://davidwalsh.name/fetch
https://developer.mozilla.org/zh-CN/docs/AJAX/Getting_Started
https://zh.wikipedia.org/wiki/AJAX