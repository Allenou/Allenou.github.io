---
title: 页面可见性
date: 2016-02-22
categories: 厚积
tags: html5
---
# 页面可见性
平时喜欢在[NBA中文网]上看一些精彩时刻。看的时候喜欢啪啪啪，点出一排想看的视频缓冲先。但是比较烦的就是只要视频缓冲的差不多了就会自动播放，这就造成了多个视频同时在播放，就好比好多个人同时在跟你说话，而你只能听一个的人一样，无语也无奈。
<!--more-->

很久没看发现网站已经做出改版，打开多个标签页，视频也不会自动播放了，不会再有之前的困扰了。不过作为技术人员，我们应该有发散的思维。能不能通过判断当前标签的可见状态，来控制视频的播放和暂停呢？这样就免去了我点击播放按钮的多余操作。于是便开始Google，最后发现了Page Visibility API：

**document.hidden**： Boolean值，表示当前标签页是否隐藏。隐藏标准规定只要当前标签变成了背景标签或浏览器被最小化都视为隐藏。

**document.visibilityState**：标签状态
- hidden：隐藏
- visible：显示
- prerender：暂时不知道有什么用
- preview：标签隐藏后，当鼠标移动改标签时，可以预览改标签页（类似win7）

**visibilitychange**：标签页可见状态改变时触发的事件

API看起来挺简单的，实践下吧。
```javascript
document.addEventListener('visibilitychange', function() {
  console.log(Date() + ':' + document.visibilityState);
});
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/02/22/b40cd1225a24cd01753a7352197cfb02.png)

成功鸟，接下来放个视频试试。
```html
<video id="video" controls="" preload="none" src="http://media.w3.org/2010/05/sintel/trailer.mp4" type="video/mp4">
  <p>Your user agent does not support the HTML5 Video element.</p>
</video>
```
不考虑兼容性。
```javascript
function listenerPageState() {
  if (document.visibilityState == 'hidden') {
    video.pause();
    console.log(Date()+':'+'pause');
  } else {
    video.play();
    console.log(Date()+':'+'play');
  }
}
listenerPageState();
document.addEventListener('visibilitychange', function() {
  listenerPageState();
});
```
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/02/22/3515b0d961d84b77120d64597935ae61.png)

也成功鸟~

> 相关文章  
> http://www.zhangxinxu.com/wordpress/2012/11/page-visibility-api-introduction-extend/  
> https://www.nczonline.net/blog/2011/08/09/introduction-to-the-page-visibility-api/
[NBA中文网]: http://china.nba.com
[Page Visibility API]: https://www.nczonline.net/blog/2011/08/09/introduction-to-the-page-visibility-api/
