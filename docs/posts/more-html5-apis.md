---
title: 5 个你不知道的 HTML5 API
date: 2016-06-18 00:20:46
categories: 翻译
tags: html5
---
HTML5 革命为我们带来了一些意想不到的 JavaScript API 和 HTML API。
<!-- more -->
其中有一些是我们渴望多年的了，另外一些也能在 mobile 端和 PC 端大施拳脚。而不管这些 API 是否强大，或者我们使用它的目的是什么，只要能帮助我们更好的完成我们的工作就是正确的。我最近和你们分享过的 [5 HTML5 APIs You Didn't Know Existed] 就是希望能够带给你们启发，用于完善你们自己的 web apps(移动网页应用)。我很乐意和你们分享一些我知道的 HTML API。希望你能用得上他们。

## Fullscreen API（全屏）
令人惊奇的 **[Fullscreen API]** 允许开发者们操作浏览器的进入全屏模式。前提得由用户触发：

```javascript
// 针对不同浏览器进行判断
function launchFullScreen(element) {
  if(element.requestFullScreen) {
    element.requestFullScreen();
  } else if(element.mozRequestFullScreen) {
    element.mozRequestFullScreen();
  } else if(element.webkitRequestFullScreen) {
    element.webkitRequestFullScreen();
  }
}

launchFullScreen(document.documentElement); // 整个页面都全屏
launchFullScreen(document.getElementById("videoElement")); // 单独元素
```
任何元素都能控制浏览器进入全屏模式，甚至一个CSS伪类。这个 API 对 **JS** 游戏开发者尤其有用，特别是像 [BananaBread] 的第一人称游戏。

>**[Fullscreen API Demo]**

## Page Visibility API（页面可见性）

**[Page Visibility API]** 提供了一个事件给开发者监听，告诉开发者们用户是在当前标签页还是移动到其它标签页或窗口。

```javascript
// 针对不同浏览器进行判断
var hidden, state, visibilityChange;
if (typeof document.hidden !== "undefined") {
  hidden = "hidden";
  visibilityChange = "visibilitychange";
  state = "visibilityState";
} else if (typeof document.mozHidden !== "undefined") {
  hidden = "mozHidden";
  visibilityChange = "mozvisibilitychange";
  state = "mozVisibilityState";
} else if (typeof document.msHidden !== "undefined") {
  hidden = "msHidden";
  visibilityChange = "msvisibilitychange";
  state = "msVisibilityState";
} else if (typeof document.webkitHidden !== "undefined") {
  hidden = "webkitHidden";
  visibilityChange = "webkitvisibilitychange";
  state = "webkitVisibilityState";
}

// Add a listener that constantly changes the title
document.addEventListener(visibilityChange, function(e) {
  // Start or stop processing depending on state

}, false);
```

如果用的好，开发者可以在用户不在当前页面的时候避免执行一些开销大的任务（类似于 **Ajax** 轮询或动画）。

>**[Page Visibility API Demo]**

## getUserMedia API （媒体获取）

**[getUserMedia API]** 是令人难以置信的有趣，它允许访问媒体设备，就像你的 MacBook's 的相机！结合这个它以及 `<video>` 标签和 **canvas**，你可以在浏览器内做出漂亮的照片。
```javascript
// 事件监听
window.addEventListener("DOMContentLoaded", function() {
  var canvas = document.getElementById("canvas"),
    context = canvas.getContext("2d"),
    video = document.getElementById("video"),
    videoObj = { "video": true },
    errBack = function(error) {
      console.log("Video capture error: ", error.code);
    };

  // 不同内核判断
  if(navigator.getUserMedia) { // 标准写法
    navigator.getUserMedia(videoObj, function(stream) {
      video.src = stream;
      video.play();
    }, errBack);
  } else if(navigator.webkitGetUserMedia) { // WebKit 写法
    navigator.webkitGetUserMedia(videoObj, function(stream){
      video.src = window.webkitURL.createObjectURL(stream);
      video.play();
    }, errBack);
  }
}, false);
```
希望在未来能够能够更好的的使用它。

>**[getUserMedia API Demo]**

## Battery API （电池）
> **Battery API** 一直在更新，看最新规范请戳 [JavaScript Battery API Update]。

**[Battery API]**  显然是一个移动端的 API ,它提供设备电池电量和状态的获取。

```javascript
// 获取电池对象
var battery = navigator.battery || navigator.webkitBattery || navigator.mozBattery;

// 几个有用的方法
console.warn("Battery charging: ", battery.charging); // true
console.warn("Battery level: ", battery.level); // 0.58
console.warn("Battery discharging time: ", battery.dischargingTime);

// 添加事件监听
battery.addEventListener("chargingchange", function(e) {
  console.warn("Battery charge change: ", battery.charging);
}, false);
```
了解电池状态，可以通知 web 应用不使用耗电进程，但是不能读取电池电量。它不是一个突破性的 API ，但至少是有帮助的。
>**[Battery API Demo]**

# Link prefetching （预加载）

**[Link prefetching]** 允许开发者们在项目中静默预先加载内容，使得网站更流畅。无缝的网络体验：

```html
<!--整个页面 -->
<link rel="prefetch" href="https://davidwalsh.name/css-enhancements-user-experience" />

<!-- 单张图片 -->
<link rel="prefetch" href="https://davidwalsh.name/wp-content/themes/walshbook3/images/sprite.png" />
```
请记住这些 API 将会在未来的几年广泛的使用，所以你应该尽早掌握它们，你才能开发出更好的世界级 web 应用。多花些时间去探索这些 API，看你是否能将它们结合使用。
> 本文根据[@David Walsh]的《[5 More HTML5 APIs You Didn’t Know Existed]》所译，全文带有自己的理解，主要目的在于提升英语水平的同时学习新知识。鉴于本人基本靠瞎猜的英语水平，建议阅读原文，以免被误导。

[@David Walsh]:https://twitter.com/davidwalshblog
[5 HTML5 APIs You Didn't Know Existed]:https://davidwalsh.name/html5-apis
[5 More HTML5 APIs You Didn’t Know Existed]:https://davidwalsh.name/more-html5-apis
[Fullscreen API]:https://davidwalsh.name/fullscreen
[BananaBread]:https://github.com/kripken/BananaBread/
[Fullscreen API Demo]:https://davidwalsh.name/demo/fullscreen.php
[Page Visibility API]:https://davidwalsh.name/page-visibility
[Page Visibility API Demo]:https://davidwalsh.name/demo/page-visibility.php
[getUserMedia API]:https://davidwalsh.name/browser-camera
[getUserMedia API Demo]:https://davidwalsh.name/demo/camera.php
[JavaScript Battery API Update]:https://davidwalsh.name/javascript-battery-api
[Battery API]:https://davidwalsh.name/battery-api
[Battery API Demo]:https://davidwalsh.name/demo/battery-api.php
[Link prefetching]:https://davidwalsh.name/html5-prefetch
