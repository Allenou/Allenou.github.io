title: 相对居中
date: 2016-04-10 17:36:11
categories: 厚积
tags: css
---
文字、图片、框框们间的相对居中。
<!--more-->

以前使用 ``vertical-align`` 的时候都是先试遍 ``vertical-align`` 的全部值，然后看哪个能达到效果就用哪个。这次我们一次试完，试出一个规律，下次再碰到就不用这么麻烦了。不过试之前让我们先来了解下 ``vertical-align``，好歹要知个所以然。

>vertical-align：设置元素的垂直对齐方式。

以下是 ``vertical-align`` 全部可选值及相关描述：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/13/12f52d3e69db6a8c2d47fbd2acb098e7.png)
哎呀，这么多文字看的好晕呀！还好盗得一图：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/13/cfbe662ffb59725a03d22fb22b8937fd.jpg)

咋没有 `sub` 和 `super` 呢？容我再盗一图：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/14/d1907aafcad9a0fed8f290465dc3c37b.png)

这样就好理解多了，真是一图胜千文呀~

接下来则开始介绍在不同情况下如何使用 ``vertical-align`` 来实现单行相对居中。

### 文字与单选框
```html
<p>
  <input type="radio"></input><label for="">口</label>
</p>
```
为了看清文字与单选框相对于父元素的位置，先给 `p` 加个背景色。
```css
p{
  text-align: center;
  background-color: lightgreen;
}
```
以下就是默认样式下的样子：（`font-size:16px;`）
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/14/d4692376bb244b391bfa761efc96acf2.png)

有点小了，我放大点（ctrl+前滚轮）。
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/14/6038dc1ac9a4b905159ff739d6700823.png)
可以发现文字和单选框并未对齐，虽然文字看起来是居中于父元素的，但其实并未居中。别问我为什么，因为我量过。

发现单选框有默认有设置 ``margin``：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/14/3ae0cbbe8fd68a01ba0011f7754ec075.png)

重置后是这样的：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/04/14/82870cebf2e3731700b4a502730d2afb.png)
可以发现文字与单选框只是相对于父元素位置变了，而相对于彼此并未发生变化。同样看起来居中于父元素的文字也未真正居中。

为了更明显的看出单选框与文字是否相对居中再改改。
```css
p{
  line-height: 0;
}
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/06/11/928ac9cf79425de1fe0afe1ff13568af.png)

遇到这种情况时，我们到底是调整单选框还是文字呢（对谁使用 `vertical-align`）？
```css
p,label{
  vertical-align: middle;
}
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/06/11/55b02966c56707a59d350ce0f380a48a.png)
### 文字与图片

```css
p {
  display: inline-block;
  vertical-align: middle;
}
```

  > 该图截至 http://christopheraue.net/2014/03/05/vertical-align/ ，中文译文 https://segmentfault.com/a/1190000002668492
>相关文章：
http://www.zhangxinxu.com/wordpress/?s=vertical-align
http://www.zhangxinxu.com/wordpress/?p=61
http://www.w3school.com.cn/tags/att_img_align.asp
http://www.htmer.com/article/780.htm
http://jilu365.com/news/show/1009
http://www.imooc.com/article/4947
http://www.zhangxinxu.com/wordpress/?p=56
http://www.zhangxinxu.com/wordpress/?p=1187
