title: 聊天室体验优化(一)：回复框的伸缩
date: 2016-01-08 20:26:56
categories: 厚积
tags:
  - mobile
  - js
---
目前我做的聊天室体验不是很好，现在还在不断的优化着，这里放几篇优化笔记，记录我的优化过程。
<!--more-->

之前看过张鑫旭老师的[div模拟textarea文本域轻松实现高度自适应],所以实现起来并没有遇到多大的障碍。
```html
<div id="edit-area" contenteditable="true" ></div>
```
普通的div加上``contenteditable``属性上就可以像输入类型一样输入文字了。并且还有可输入类型都有的淡蓝色边框。

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/07/8d36a0bde60f20aac457305eb26825fe.png)
自动增长完全可以通过纯css实现。
```css
#edit-area{
    min-height: 30px;
    max-height: 80px;
}
```
只需要设置最小高度和最大高度就行了。
```css
#edit-area{
    overflow-y: auto;
}
```
这样设置是为了当输入的文字超出最大高度后可以隐藏多于的文字的同时并且支持滚动浏览。不过要想达到看得过去的效果，还需要美化一下。
完整代码：
```css
#edit-area {
    overflow-y: auto;
    width: 70%;
    min-height: 25px;
    max-height: 80px;
    padding: 10px;
    outline: none;
    border: 1px solid #0099FF;
    border-radius: 4px;
}
```
效果图：
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/07/b832151372d7282ef75c7c958a13a870.png)

模拟的毕竟是模拟的，无法支持可输入类型特有的``placeholder``属性，不过我们可以模拟这个效果，具体怎么实现我之前有写过《[模拟placeholder]》。
另一大缺点在于可以复制HTML进去，并以解析后的DOM形式展示。like this:
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/09/eb7bd8c816b84eb40f9858f7c86cbee1.png)

你可能以为你复制的只是纯文本，其实你复制的是一堆HTML标签，带内联样式的HTML标签...

![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/09/d0d78c94190c31d5ae6afb41ebe9593d.png)

这个是在是太挫了，有解决办法吗？这家伙和富文本编辑器有什么关联？
我等疏浅，不过张老师最近出了新文《[小tip: 如何让contenteditable元素只能输入纯文本]》，有兴趣的可以深究下。

除了这个问题外，还有其他的吗？
的确有的，后端抱怨过说无法表单提交。我说能不能用隐藏域呢？like this:
```html
<form action="" method="post">
    <div id="contenteditable" contenteditable="true"></div>
    <input type="hidden" name="" id="textarea" />
    <button type="submit" id="send">发送</button>
</form>
```
表单里面放一个隐藏的input。
```javascript
var contenteditable = document.querySelector('#contenteditable'),
    textarea = document.querySelector('#textarea'),
    sendBtn = document.querySelector('#send'),
    contenteditableVal;

function listenVal() {
    contenteditableVal = contenteditable.innerHTML;
    textarea.value = contenteditableVal;
}

sendBtn.addEventListener('click', listenVal);
```
然后点击发送时将模拟的textarea的值赋给隐藏的input。我知道这样是可以的，上家公司就是这样实现的。他说好麻烦...

好吧，不麻烦别人那就麻烦自己吧。

整理下思绪。原生textarea是有滚动条的吧，那能不能实时监测滚动条，把滚动条的高度设为本身的高度呢？试试就知道了。
```css
textarea {
    height: 25px;
    max-height: 80px;
}
```
首先设置最小高度和最大高度。
```javascript
var textarea = document.querySelector('textarea');
function reHeight() {
    textarea.style.height = textarea.scrollHeight + 'px';
}
textarea.addEventListener('input', reHeight);
```
实践证明这样做是可以的，不过高度并不是在输入的时候随着输入的字数增多而逐渐自动增长的，而是在textarea失去焦点时一次性增长的。这样明显不符合需求，问题出在哪也很明显。这是``change``事件的特性。看下有没有其他事件适合这种场景。
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/10/8f417dd2a3a59183854f09faca19af7c.png)
```javascript
textarea.addEventListener('keyup', reHeight);
textarea.addEventListener('keydown', reHeight);
```
这两个事件都是可以的。比较下这两个事件。

**keyup**:
1. 手指离开键盘后才执行，这时文字文字已经输入成功
2. 持续输入一个字符（按着键盘不松），textarea高度在松开键盘时才会一次性增长
3. 输入足够文字符，长按清除键清除，textarea高度只减少一行字符的高度

**keydown**:
1. 手指按下键盘时执行，这时文字还没输入成功
2. 持续输入一个字符，textarea高度持续性增长
3. 输入足够字符，按清除键清除，textarea高度岁字符多少而变化

经过对比，看来使用``keydown``更适合一点。

高度可以自动增长了，但是用户体验还不是那么好。输入的时候，textarea的高度明显有在减小的变化。
![](http://7xopm5.com1.z0.glb.clouddn.com/2016/01/10/b955035b8617e8ce8167044e511e1013.gif)
再限制下最小高度。

```css
textarea {
    height: 25px;
    min-height: 25px;/*new code*/
    max-height: 80px;
}
```
中英文混合输入的时候还是有点小问题。后续优化。

**（-------2016-01-14-------）**

上面的比较结果是PC上测试所得，没有在移动端测试。今天测试了下移动端，到现在心都还是哇凉哇凉的。

哇凉哇凉原因一：输入文字超过1行时，textarea高度没有立刻自动增长，而是再输入几个字才增长
哇凉哇凉原因二：输入文字超过1行后，清除所输入的字符，textarea高度没有收缩
如何是好？

机智如我。
考虑到手机上除了清除按钮，其它键都不能长按，不会出现textarea高度在松开键盘时才会一次性增长的情况，所以改用``keyup``事件试了试，结果就真的解决了哇凉哇凉原因一。
可是哇凉哇凉原因二怎么办呢？有木有办法实时监测textarea，值改变后就立刻能响应的事件呢？我深信是有的。

技术问题问Google，然后真的找到了这个事件。关于``oninput``事件可以看下这篇[文章]。
试了下，非常的爽，PC和MB都没问题，看来好事还是要多磨呀。

```javascript
textarea.addEventListener('input', reHeight);
```
最后的事件绑定代码。

[div模拟textarea文本域轻松实现高度自适应]:http://www.zhangxinxu.com/wordpress/?p=1362
[小tip: 如何让contenteditable元素只能输入纯文本]:http://www.zhangxinxu.com/wordpress/2016/01/contenteditable-plaintext-only/
[模拟placeholder]:http://www.toyou.xyz/2015/11/20/placeholder/
[文章]:http://blog.csdn.net/freshlover/article/details/39050609
> 扩展阅读：http://blog.csdn.net/freshlover/article/details/39050609
