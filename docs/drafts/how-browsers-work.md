title: 浏览器是怎么工作的
date: 2018-09-09 21:55:01
categories: 厚积
tags: browser
---
作为一个前端，浏览器的渲染原理多少还是要知道些的。
<!-- more-->
![](http://7xopm5.com1.z0.glb.clouddn.com/2018/05/15/ddac7c17d8de058785ac796d1f9c5e70.png)

上图是浏览器的主要组成部分。

* 用户界面（User Interface）：用户能直观看到且可操作的的界面
* 浏览器引擎（Browser engine）：传送用户界面指令给渲染引擎
* 渲染引擎（Rendering engine）：负责解析及渲染文档内容
* 网络（Networking）：负责网络调用
* 用户界面后端（UI backend）：调用操作系统用户界面方法，显示浏览器界面组件
* JS 解析器（JavaScript interpreter）：负责解析和执行 JS 代码
* 数据存储（Data storage）：负责数据的持久存储

## 用户界面
1. 用户在地址栏输入网址

## 网络
1. 用户输入域名访问目标网站，域名解析系统（DNS）会解析这串网址并得到服务器的 IP 地址
2. 浏览器请求 IP 地址，得到服务器返回的资源

## 渲染引擎
![](http://7xopm5.com1.z0.glb.clouddn.com/2018/05/12/1f244e42b4eda15df5bc2b2cd883e62a.png)

上图展示的是浏览器渲染的大概流程：
* HTML 解析器将 HTML 文档解析成 DOM 树（HTML Parse）
* CSS 解析器将样式表解析成 CSSOM（CSS Object Model）树（CSS Parse）
* 合并 DOM 树和 CSSOM 树生成渲染树（render tree）
* 对渲染树布局（Layout）
* 绘制渲染树（Paint）
* 展示（Display）

渲染流程：解析->布局->渲染->绘制
解析流程：词法、语法解析->解析树（Parse tree）->编译成机器代码
词法解析过滤无效标签，解析器再根据语法规则生成解析树（异步）
渲染过程是由上至下异步渲染的。


具体的流程并未体现，HTML 和 CSS 的解析是有先后顺序的。
* 渲染引擎从上往下解析服务器返回的 HTML 文档并构建 DOM 树（JS 操作DOM）
* 解析文档过程中遇到外部资源会另开一个线程去异步加载
  若外部资源为 CSS 则会停止解析 JS
  若外部资源为 JS 则会停止解析 HTML 
* 解析过程中遇到样式，则解析并生成 CSSOM（CSS Object Model）树

构建DOM树过程中，如果元素 display 属性为 none，则元素不会显示在render tree中。
文档解析完后开始解析 defined 脚本
<!-- 我们知道，浏览器渲染引擎（render engine）从上往下解析 HTML 文档,并将各标记逐个转化成“内容树”上的 DOM 节点，形成 “DOM 树”。 同时碰到外部样式也会同时解析 样式表 并产生 CSSOM（CSS Object Model），并和 “DOM 树” 组成“渲染树”（render tree）。 -->

边解析绘制，如果遇到 js 脚本则会立即解析并执行，遇到外部 js 脚本文件，则先请求加载完并解析后再继续解析文档。如果不想停止解析则家 defer 标记。
因为样式不会更改DOM 结构，所以加载样式时不会停止解析文档。如有新样式则覆盖老样式，否则使用浏览器默认样式。
构建CSS Object Model（CSSOM)会阻塞JavaScript的执行。JavaScript的执行也会阻塞DOM的构建。
解析 HTML 文档过程中，遇到内部 css 和 js 会直接调用相应解析器解析，如果遇到外部资源会通过网络层下载后再解析，此时文档会停止解析。

内敛样式怎么解析
css 前缀

CSS 选择器解析顺序

文本换行

https://aotu.io/notes/2017/04/11/GPU/
How browsers work：http://taligarsiel.com/Projects/howbrowserswork1.htm

https://juejin.im/entry/5a123c55f265da432240cc90
https://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/
https://coolshell.cn/articles/9666.html
https://www.zybuluo.com/yangfch3/note/671516

https://www.zhihu.com/question/30218438
http://www.ruanyifeng.com/blog/2015/09/web-page-performance-in-depth.html
http://blog.codingplayboy.com/2017/03/29/webpage_render/
https://github.com/skyline75489/what-happens-when-zh_CN
http://fex.baidu.com/blog/2014/05/what-happen/
http://www.cnblogs.com/yuezk/archive/2013/01/11/2855698.html


https://blog.fundebug.com/2019/01/03/understand-browser-rendering/
https://www.paulirish.com/2013/webkit-for-developers/