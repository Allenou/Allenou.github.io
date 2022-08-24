title: JavaScript 如何在浏览器和 Node 中运行？
date: 2019-02-22 22:26:20
categories: 翻译
tags: js
thumbnail: /images/how-javascript-works-in-browser-and-node.png
---
聊聊 Javscript 的运行时、回调队列、事件循环以及 Web Api。
<!--more-->

有许多热情的开发人员，在前端或后端工作，致力于保护JavaScript的领域。JavaScript非常容易理解，是前端开发的重要组成部分。但与其他编程语言不同，它是单线程的。这意味着，代码执行将一次完成一个。由于代码执行是按顺序执行的，因此任何需要较长时间执行的代码都会阻塞任何需要执行的代码。因此，有时您在使用Google Chrome时会看到以下屏幕。
![chrome页面无响应对话框](0.png)

当您在浏览器中打开网站时，它使用单个JavaScript执行线程。该线程负责处理所有事情，例如滚动网页，在网页上打印内容，收听DOM事件（例如用户点击按钮时）以及执行其他操作。但是当JavaScript执行被阻止时，浏览器将停止执行所有这些操作，这意味着浏览器将会冻结并且不会响应任何内容。

你可以看到使用下面永恒 `while` 循环的行动。
```javascript
while(true){}
```
上面语句之后的任何代码都不会被执行，因为while循环将无限循环，直到系统资源不足。这也可以在无限递归函数调用中发生。

感谢现代浏览器，并非所有打开的浏览器选项卡都依赖于单个JavaScript线程。相反，他们每个标签或每个域使用单独的JavaScript线程。对于Google Chrome，您可以打开包含不同网站的多个标签，并在永久while循环之上运行。这只会冻结执行该代码的当前选项卡，但其他选项卡将正常运行。任何具有从同一域/同一网站打开的页面的选项卡也将冻结，因为Chrome实现了每站点一个进程的策略，并且一个进程使用相同的JavaScript执行线程。

为了可视化JavaScript如何执行程序，我们需要了解JavaScript运行时。
![JavaScript运行时环境](1.png)

与任何其他编程语言一样，JavaScript运行时具有一个堆栈和一个堆存储。我不打算更多地解释堆，你可以在这里阅读。我们感兴趣的是堆栈。堆栈是LIFO（后进先出）数据存储器，其存储程序的当前功能执行上下文。当我们的程序加载到内存中时，它从第一个函数调用开始执行foo()。

因此，第一个堆栈条目是foo()。由于foo函数调用bar函数，第二个堆栈条目是bar()。由于bar函数调用baz函数，第三个堆栈条目是baz()。最后，baz函数调用console.log，第四个堆栈条目是console.log('Hello from baz')。

直到函数返回某些东西（当函数正在执行时），它才会从堆栈中弹出。一旦该条目（函数）返回某个值，堆栈将逐个弹出条目，并且它将继续等待函数执行。
![堆栈帧](2.gif)

在每个条目中，堆栈的状态也称为堆栈帧。如果给定堆栈帧的任何函数调用产生错误，JavaScript将打印堆栈跟踪，这只是该堆栈帧上代码执行的快照。
```javascript
function baz（）{ 
   throw new Error（'Something failed。'）; 
}
function bar（）{ 
   baz（）; 
}
function foo（）{ 
   bar（）; 
}
FOO（）;
```
在上面的程序中，我们从baz函数中抛出错误，JavaScript将打印在堆栈跟踪下面，以找出出错的地方和位置。
![chrome控制台中的错误堆栈跟踪](3.png)
由于JavaScript是单线程的，因此它只有一个堆栈和一个堆。因此，如果任何其他程序想要执行某些操作，则必须等到上一个程序完全执行。

这对任何编程语言都不好，但JavaScript被设计用作通用编程语言，而不是用于非常复杂的东西。

所以让我们想一个场景。如果浏览器发送HTTP请求以通过网络加载某些数据或加载图像以在网页上显示，该怎么办？浏览器会冻结，直到该请求得到解决？如果是这样，那么对用户体验来说非常糟糕。

浏览器附带一个JavaScript引擎，提供JavaScript运行时环境。例如，Google Chrome使用由他们开发的V8 JavaScript引擎。但是猜猜看，浏览器使用的不仅仅是JavaScript引擎。这就是引擎盖下的浏览器。
![chrome控制台中的错误堆栈跟踪](4.png)

看起来很复杂，但很容易理解。JavaScript运行时实际上包含2个以上的组件即。事件循环和回调队列。回调队列也称为消息队列或任务队列。

除了JavaScript引擎之外，浏览器还包含不同的应用程序，可以执行各种操作，例如发送HTTP请求，侦听DOM事件，使用setTimeout或延迟执行setInterval，缓存，数据库存储等等。浏览器的这些功能帮助我们创建丰富的Web应用程序

但想想看，如果浏览器必须使用相同的JavaScript线程来执行这些功能，那么用户体验将会非常糟糕。因为即使用户只是滚动网页，在后台也会发生很多事情。因此，浏览器使用低级语言C++来执行这些操作并提供干净的JavaScript API。这些API称为Web API。

这些Web API是异步的。这意味着，您可以指示这些API在后台执行某些操作并在完成后返回数据，同时我们可以继续执行JavaScript代码。在指示这些API在后台执行某些操作时，我们必须提供回调函数。回调函数的责任是在Web API完成它的工作后执行一些JavaScript。让我们了解所有部分如何协同工作。

因此，当您调用函数时，它会被推送到堆栈。如果该函数包含Web API调用，JavaScript将使用回调函数将其控制权委托给Web API，并移至下一行直到函数返回某些内容。一旦函数命中return语句，该函数将从堆栈中弹出并移动到下一个堆栈条目。同时，Web API正在后台完成它的工作，并记住与该作业相关的回调函数。作业完成后，Web API会将该作业的结果绑定到回调函数，并使用该回调将消息发布到消息队列（AKA 回调队列）。事件循环的唯一工作是查看回调队列，一旦回调队列中有待处理的事情，就将该回调推送到堆栈。一旦堆栈为空，事件循环一次将一个回调函数推送到堆栈。之后，stack将执行回调函数。

让我们看一下使用setTimeoutWeb API 如何一步一步地工作。setTimeoutWeb API主要用于在几秒钟后执行某些操作。一旦程序中的所有代码都执行完毕（堆栈为空），就会执行此执行。setTimeout功能的语法如下。
```javascript
setTimeout（callbackFunction，timeInMilliseconds）;
```
callbackFunction是一个将在之后执行的回调函数timeInMilliseconds。让我们修改我们之前的程序并使用这个API。
```javascript
function printHello（）{ 
    console.log（'Hello from baz'）; 
}
function baz（）{ 
    setTimeout（printHello，3000）; 
}
function bar（）{ 
    baz（）; 
}
function foo（）{ 
    bar（）; 
}
FOO（）;
```
对程序进行的唯一修改是，我们将console.log执行延迟了3秒。在这种情况下，堆栈将继续像堆栈一样foo() => bar() => baz()。一旦baz开始执行并点击setTimeoutAPI调用，JavaScript将把回调函数传递给Web API并移动到下一行。因为，没有下一行，堆栈将弹出baz，bar然后是foo函数调用。同时，Web API正等待3秒钟通过。一旦传递3秒，它就会将此回调推送到回调队列，并且由于堆栈为空，因此事件循环会将此回调放回堆栈，此时将执行此回调。

Philip Robers创建了一个令人惊叹的在线工具，可以直观地了解JavaScript的工作原理。我们的上述示例可在此链接中找到。
![堆栈帧](5.gif)

说到Node.js，它必须做更多，因为节点承诺更多。在浏览器的情况下，我们仅限于在后台可以做的事情。但是在节点中，我们几乎可以在后台完成大部分工作，即使它是简单的JavaScript程序。但是，这是如何工作的？

Node.js使用Google的V8引擎来提供JavaScript运行时，但不仅仅依赖于它的事件循环。它使用libuv库（用c编写）与V8事件循环一起工作，以扩展后台可以完成的工作。Node遵循与Web API相同的回调方法，并以与浏览器类似的方式工作。
![](6.jpg)

如果将浏览器图与上述节点图进行比较，则可以看到相似之处。整个右侧部分看起来像Web API，但它还包含事件队列（回调队列/消息队列）和事件循环。但是V8，事件队列和事件循环在单个线程上运行，而工作线程负责提供异步I / O操作。这就是Node.js被称为非阻塞事件驱动的异步I / O架构的原因。

本文根据@Landon Schropp的《Getting Dicey With Flexbox》所译，全文带有自己的理解，主要目的在于提升英语水平的同时学习新知识。鉴于本人基本靠瞎猜的英语水平，建议阅读原文，以免被误导。