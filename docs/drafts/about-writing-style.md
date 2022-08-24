---
title: 浏览器渲染原理
date: 2018-09-09 21:55:01
---
写样式不难，却有讲究。
<!-- more-->
好的样式书写习惯不但有利于项目维护且可以提升浏览器的渲染性能。
先看一张图：
![webkitflow]( 7xopm5.com1.z0.glb.clouddn.com/2018/05/12/1f244e42b4eda15df5bc2b2cd883e62a.png "webkitflow")
先了解下浏览器的页面呈现原理，浏览器从获取到 HTML 文档到页面呈现分为以下几步：
. 解析 HTML 文档构建成 DOM 树
. 解析 CSS 样式表构建成 CSSOM（CSS Object Model）树
. 合并 DOM 树和 CSSOM 树生成渲染树（render tree）
. 对渲染树布局
. 绘制渲染树
我们知道，浏览器渲染引擎（render engine）从上往下解析 HTML 文档,并将各标记逐个转化成“内容树”上的 DOM 节点，形成 “DOM 树”。 同时碰到外部样式也会同时解析 样式表 并产生 CSSOM（CSS Object Model），并和 “DOM 树” 组成“渲染树”（render tree）。
### 选择器
浏览器解析 CSS 文件时，是从右往左的顺序的，举个列子：
 
```css
body div p a {
  color: #212121;
}
```
上面的样式的解析过程是 a->p->div->body，CSS 解析器会先



### 书写顺序

提升性能？从何谈起？先看一段示例代码：

```html
<style>
  section main div 
</style>
<body>
    <section>
        <main>
          <div>
            
          </div>
        </main>
    </section>
</body>


```
这要说到浏览器的渲染机制。
浏览器展示文档分为两个阶段，如下图所示：
![来源于 MDN]( 7xopm5.com1.z0.glb.clouddn.com/2018/05/12/b39e6dfee88edf46540e2f715faebdd1.svg "来源于 MDN")
加载完 HTML后解析 HTML，直接形成 DOM 树。然后加载 CSS，然后解析 CSS
css 前缀
How browsers work：http://taligarsiel.com/Projects/howbrowserswork1.htm
https://my.oschina.net/anna153/blog/377259
https://juejin.im/entry/5a123c55f265da432240cc90
https://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/
https://coolshell.cn/articles/9666.html
https://www.zybuluo.com/yangfch3/note/671516


http://www.ruanyifeng.com/blog/2015/09/web-page-performance-in-depth.html


https://csstriggers.com/