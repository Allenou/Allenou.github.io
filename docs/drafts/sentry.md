title: 聊天室体验优化(二)：显示最新记录
date: 2016-01-11 14:16:26
categories: 厚积
tags:
  - css
  - js
---
显示最新记录说得通俗易懂点就是保持滚动条在最底部。
<!--more-->

首先分析一下，需要将滚动条保持在最底部有哪几种情况。
- 进入聊天室后：进入聊天室，应该显示最新的聊天消息
- 弹出虚拟键盘时：弹出虚拟键盘时，虚拟键盘不应遮挡住聊天消息
- 发送消息时：输入的文字超过一行时，变高的回复框不应挡住消息
- 发送消息后：发送新信息后，虚拟键盘未消失，并且显示新发的消息

知道有哪些应用场景后，再来分析如何实现。

** 1.进入聊天室**

让滚动条到顶部距离等于文档高度。
```javascript
window.scrollTo(0, document.body.scrollHeight);
//or
document.body.scrollTop = document.body.scrollHeight;
```
这样一来进入聊天室后，滚动条就到最底部了，显示的也就是最新的消息了。不过有一点我不明白的就是，在聊天室页面刷新，滚动条到最底部后又会回到最顶部，重其它页面进入聊天室就不会出现这种问题。这个疑问，纠结了很久，始终不明白是为什么。

** 2.弹出虚拟键盘**

如何不让虚拟键盘挡住最新的消息也是同样的原理，只要当输入框获得焦点时将滚动条滚到最底部就行了。
```javascript
document.querySelector('textarea').addEventListener('focus', function(){
  document.body.scrollTop = document.body.scrollHeight;
});
```

不过直接这样写是没有效果的。因为虚拟键盘弹出来也需要时间，所以要用``setTimeout``延迟下。
```javascript
setTimeout(function () {
  document.body.scrollTop = document.body.scrollHeight;
},200);
```
直接设置200毫秒确实有点不科学，因为可能不同机型下虚拟键盘弹出耗时并不统一，如果太慢就很有可能达不到想要的效果，但又想不到其它办法了，确实有点无奈。

然而又不能每次只要输入框获取焦点后都滚动到最后，只有当已经是页面最底部时输入框再获得焦点才滚，所以还要加个判断。
```javascript
var scrHeight = document.body.scrollHeight,//整个文档高度
    cliHeight = document.body.clientHeight,//网页可见区域高
    scrTop = document.body.scrollTop;//滚动条滚动距离

if (cliHeight + scrTop == scrHeight) {
  setTimeout(function() {
    document.body.scrollTop = document.body.scrollHeight;
  }, 200);
}
```
这样翻阅聊天记录时回复，就不会跑到底部了。

** 3.发送消息时**

因为回复框是用定位固定在底部的，在文档中不占据空间，为了不让它挡住聊天消息，是设置聊天室的容器内底边距``padding-bottom``来避免的。就像这样:

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/02/28/740b0cfe29114ee175fa231fb1c7d72b.png)

如果所输入的文字超过两行时，不想高度增加后的回复框挡住消息，就要实时改变``padding-bottom``的值。
直接判断行数我是不会的，但是我相

** 4.发送消息后**

发送消息后文档高度又变高了，所以在发送消息后还需要调用一遍``inToMsgView``函数。
