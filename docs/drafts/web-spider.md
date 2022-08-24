title: 电影网站(番外)：数据抓取
date: 2016-07-20 22:45:25
categories: 笔记
tags:
   - nodejs
   - mongodb
   - mongoose
---
这章本来是要做分页的，但是分页需要很多数据，手动收入太麻烦，之前一直想了解数据抓取这方面的东西，正好趁这个机会学习下。为了理解的更深刻，决定一步步深入，故本章不在原电影项目中演示，而是独立了一个新项目，下面是这次学习的笔记。
<!--more-->

>因为对自己的东西比较熟悉，所以首先用的自己的 blog 做测试。

首先新建项目：
```javascript
mkdir spider  //新建项目
cd spider    //进入项目
npm init    //初始化项目配置
```
然后新建 `spider.js`，输入以下代码：
```javascript
var http = require('http');//加载 http 模块
var html = '';
http.get('http://www.toyou.xyz', function(res) {
  res.on('data', function(chunk) {// chunk 存储的是请求成功后返回的 html 文档
    html += chunk;
  });
  res.on('end', function() {
      console.log(html); //在命令行打印获取到的数据
    })
  });
});
```
> **http** 模块是 **Nodejs** 的内置模块，无需安装直接加载即可
关于 **http** 模块,[阮老师]在其 [JavaScript 标准参考教程（alpha）] 中的 [Nodejs] 篇中的 [Http模块] 一节中讲的很明白。

然后运行命令 `node spider.js`，就可以在命令行看到获取到的数据了。获取到的数据是整个首页整个页面的 **html** 文档，我们的目的在这些文档中提取对我们有用的数据，这就要用到 **html** 解析工具。我用的的解析工具是 [cheerio],它被称为服务端的 **jquery** 。

>既然是服务端的 **jquery** ，其语法自然也是和 **jquery** 差不多的。

**cheerio** 为独立模块，使用前需先安装：
```javascript
npm install cheerio --save
```
我想要获取的是文章的标题，通过浏览器检查结构可以发现，文章是在 `article` 下的，而文章的标题则是在 `article` 下的 `article-title` 下的，如图：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/08/14/464f02b9d4d6f8b30977df8ee59e1b64.png)

所以使用了 **cheerio** 后的代码是这样的：
```javascript
var http = require('http');
var cheerio = require('cheerio');

http.get('http://www.toyou.xyz', function(res) {
  html = '';
  res.on('data', function(chunk) {
    html += chunk;
  });
  res.on('end', function() {
    var $ = cheerio.load(html);
    $('article').each(function() {//循环 article
      var title = $('.article-title', this).text();//获取 title
      console.log(title);
    });
  });
});
```
此时再次运行命令 `node spider.js`，就可以看到获取到的 **title** 了。但这只是第一页的内容，做分页这点数据是不够的，所以要下一步是要爬取多页的内容。

爬取多页和爬取一页没多大区别，一般像这种分页的网址都是有规律的，一般这种规律可以通过访问非第一页找出。我 blog 的第二页地址是 `http://www.toyou.xyz/page/2/`，所以可以推断出第三页的地址应该是 `http://www.toyou.xyz/page/3/`，只要改变 page 后面的数字就可以获取到不同页的内容，所以可以通过以下方式获取多页内容：
```javascript
var Spider = (function() {
    var http = require('http'),
        cheerio = require('cheerio'),
        html = '';
    function spider(index) {
      var baseUrl = 'http://www.toyou.xyz/',spiderUrl = '';
      if (index === 0) {
        spiderUrl = baseUrl;
      } else {
        spiderUrl = baseUrl + index + '/';
      }
      http.get(spiderUrl, function(res) {
        res.on('data', function(chunk) {
          html += chunk;
        });
        res.on('end', function() {
          var $ = cheerio.load(html);
          $('article').each(function() {
            var title = $('.article-title', this).text();
            console.log(title);
          });
        });
      });
    }
    return {
      start: function(max) {
        var i = 0
        while (i < max) {
          spider(i);
          i++;
        }
      }
    }
}())

Spider.start(4)  //我的 blog 只有 4页
```
因为通过访问 `http://www.toyou.xyz/page/1/`，拿不到第一页的内容，所以上面对 index 参数做了个判断。如果是 0 ，则使用基本路径 `http://www.toyou.xyz/`。

```javascript
var http = require('http'),
  cheerio = require('cheerio')

var options = {
  baseUrl: 'http://www.toyou.xyz/',
  pageCount: 4
}

function getPages(options) {
  var baseUrl = options.baseUrl
  var i = 1,
    pagesUrl = '',
    html = ''
  while (i <= options.pageCount) {
    if (i === 1) {
      spiderUrl = baseUrl;
    } else {
      spiderUrl = baseUrl + 'page/' + i + '/';
    }
    getLinks(spiderUrl)
    i++
  }
}

function getLinks(spiderUrl) {
  console.log(spiderUrl)
  var html = ''
  http.get(spiderUrl, function(res) {
    res.on('data', function(chunk) {
      html += chunk;
    });
    res.on('end', function() {
      var $ = cheerio.load(html);
      $('article').each(function() {
        var title = $('.article-title', this).text();
        console.log(title);
      });
    });
  });
}
getPages(options)

```
分页是可以做了，但点进去总得有东西吧，所以还得获取每篇文章的详情。



内容中文字符乱码讨论：https://cnodejs.org/topic/545de1e1a68535a174fe51b5
内容中文字符乱码解决方案：http://www.99css.com/nodejs-request-chinese-encoding/
>参考文章:
http://kuka.im/2016/05/16/nodejs-spider/
http://www.cnblogs.com/coco1s/p/4954063.html

[阮老师]:http://www.ruanyifeng.com/blog/
[JavaScript 标准参考教程（alpha）]:http://javascript.ruanyifeng.com/
[Nodejs]:http://javascript.ruanyifeng.com/#nodejs
[Http模块]:http://javascript.ruanyifeng.com/nodejs/http.html
[cheerio]:https://github.com/cheeriojs/cheerio
