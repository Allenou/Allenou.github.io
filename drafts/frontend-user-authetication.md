title: MSUI 填坑记
date: 2016-07-28 21:14:11
categories: 厚积
tags: msui
---
**MSUI** 是淘宝 UI 框架 [SUI] 的移动端版本，这篇文章记录了我在使用 **SUI** 中遇到的一些问题（坑）。
<!--more-->

## 坑1：通过路由功能载入的页面，事件失效

关于这个问题，官方的解释是这样的：
>由于浏览器安全性考虑的限制以及可能的 js 重复执行或覆盖的问题，目前是不支持运行 ajax 载入的页面里面的 js 的，参考 [#120]。

但是我发现其它页面的脚本没有被加载进来，或许是因为不能运行所以直接不加载了吧...

针对这个问题官方给出了这样的解决方法：
>所有页面都引用相同的 js，而这个 js 里面包含了所有的逻辑，事件部分使用委托来绑定。

对于这种解决方法我是不满意的，我宁愿不用路由也不会用这种方法，谁叫我有代码洁癖呢 > <。
## 坑1填法：
#### 填法1：禁用部分页面路由

官方 API 中 [router] 一节中有写，可以通过给跳转至新页面的超链接上加 `external` 类或  `external` 属性，而禁用该页面的路由功能。如下：
```html
<a class="tab-item" href="./member.html" class="external">  //加 external 类
    <span class="icon icon-me"></span>
    <span class="tab-label">我</span>
</a>
或
<a class="tab-item" href="./member.html" external>  //加 external 自定义属性
    <span class="icon icon-me"></span>
    <span class="tab-label">我</span>
</a>
```

#### 填法2：完全禁用页面路由

也许你想到了完全禁用路由的方法，那就是给页面所有 `a` 标签加上`external` 类或  `external` 。如下：
```javascript
$('a').attr('external','external');
```
但大可不必这样，官方提供了完全禁用路由的方法。如下：
>如果需要禁用路由功能,那么可以在 zepto 之后, msui 之前使用 script $.config = {router: false} 来禁用.

禁用路由太暴力了，正是因为它的路由功能我才用它，如果禁用了就完全没用下去的欲望了，咱再另寻他法。

#### 填法3：移动脚本位置

API 中 [router] 一节中有写,路由的前提是将页面内容写在 `page-group` 类里，并且可以切换的块必须带有 `page` 类，所以可以断定 **MSUI** 的路由就是通过这样特定的结构以 **ajax** 异步请求的方式来实现不同页面间的切换的，所以直接把脚本写在 `page` 类里就可以解决这个问题了。如下：

```html
<div class="page-group">
    <div class="page">
        <script src='//g.alicdn.com/sj/lib/zepto/zepto.min.js'></script>
        <script src='//g.alicdn.com/msui/sm/0.6.2/js/??sm.min.js,sm-extend.min.js'></script>
        <script >
          // 事件写在这
        </script>
    </div>
</div>
```
### 坑2：通过路由功能载入的页面,title 保持载入之前页面的 title 不变

前面说过， **MSUI** 的路由是替换的只是包含在 `page` 下的内容，所以 `title` 不会改变。

这个问题有人在 [issues] 里问过了，官方的回答是这样的：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/07/27/8d158d92e78ced1a3cdf630d2e5541c6.jpg)

我不知道要如何实现动态改变，不过我这样尝试过：
```javascript
$(document).on('pageInit', '#member', function() {
      var title = $('title').text();
      $('title').text(title);
})
```
可惜并不行，获取的 `title` 一直是跳转之前的。

## 坑2填法：

不得已，只能这样为之：
```javascript
function setTitle(page, title) {
      $(document).on("pageInit", page, function(e, pageId, $page) {
        $('title').text(title);
      });
}

setTitle($('#member'),'会员中心');
```
这种方法虽然蠢了一点，倒也还能用，并且不用担心返回时 `title` 又保持跳转之前的不变。


## 坑3：幻灯片没有效果

幻灯片属于 **MSUI** 的拓展组件，其实就是 [Swiper]。

按照官方 [API] 的说法,幻灯片会自动初始化，但是我使用时并没有效果，如下：

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/07/22/169ddd7946362c2255edc5fa850f7ce9.gif)

## 坑3填法：

#### 填法1：手动初始化

既然你不自动，只能我手动了。
```html
<div class="page-group">
    <div class="page">
      <script src='//g.alicdn.com/sj/lib/zepto/zepto.min.js'></script>
      <script src='//g.alicdn.com/msui/sm/0.6.2/js/??sm.min.js,sm-extend.min.js'></script>
      <script>
          $.init();//手动初始化
      </script>
    </div>
</div>
```
#### 填法2：使用 Swiper

**Swiper** 不会出现此问题，但是如果这么做了，要么就是在打自己的脸，要么就是在 **MSUI** 的脸，而我谁的脸都不想打，所以我选择的是第一种填法。

### 坑4：幻灯片滑动顺序错乱

因为选择了框架自带的幻灯片，所以不可避免的出现了这个问题。

滑动首页的幻灯片至非第一张，然后进入其它页面，再返回首页滑动幻灯片。第一次滑动失效，幻灯片并没有切换，但算作一次滑动，导致第二次滑动时，幻灯片跳过了第二张直接到第三种，如下：

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/07/24/c5051e9d4baa07781a61ddbcb609eade.gif)

### 坑4填法：
同坑 1 填法 3

使用 **MSUI** 时间不长，以上为目前所遇到的坑及其填法，如遇新坑后续更新。

[MSUI]:http://m.sui.taobao.org/
[SUI]:http://sui.taobao.org/sui/docs/
[Swiper]:http://www.swiper.com.cn/
[API]:http://m.sui.taobao.org/extends/
[#120]:https://github.com/sdc-alibaba/SUI-Mobile/issues/120#issuecomment-160417276
[router]:http://m.sui.taobao.org/components/#router
[issues]:https://github.com/sdc-alibaba/SUI-Mobile/issues/49
