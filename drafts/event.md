---
title: 事件
date : 2017-02-21
---
# 事件
- IE 事件和标准事件有哪些区别？（2017-02-21）

IE8 后 IE 事件已与标准事件一致，所以区别主要是在 IE 8 和 早期版本的 IE 浏览器。
IE 和 标准事件的区别主要是在 IE 9 之前。标准事件指的是 DOM0 DOM2 DOM3 级事件。
事件是浏览器的重要组成部分，其承担了 JS 和 HTML 之间的桥梁。早起因没有规范，所以各浏览器对事件的实现有所不同。不同之处表现在以下几点：

## 事件流
事件流描述的是页面接收事件的顺序。

### 事件冒泡

IE 的事件流叫事件冒泡，事件冒泡的特点是从里向外传播。

![](/better/browser/event-bubbling.png)


### 事件捕获
网景的事件流叫事件捕获，事件捕获的特点是从外向里传播。

![](/better/browser/event-capturing.png)


### DOM 事件流
标准的 DOM 事件流则是综合了“事件冒泡”和“事件捕获”，它的传播过程是先捕获再冒泡：
![](/better/browser/event-dom-event-flow.png)


## 事件处理程序

### DOM0级事件处理程序
DOM0级处理成程序绑定事件的方式有两种：

```html
<button onclick="say()"></button>
<script>
    function say(){
        console.log("Hello world!")
    }
</script>

<button id="btn"></button>
<script>
    var btn = document.getElmentById('btn')
    btn.onclick = function (){
        console.log("Hello world!")
    }
</script>
```

### DOM2级事件处理程序
“DOM2级事件”新增了两个用于指定和删除事件处理程序的两个方法：addEventListener()和removeEventListener()。

addEventListener() 方法接收 3 个参数，第一个是事件类型，第二个是回调函数，第三个是 Boolean 值，为 true 时采用“事件捕获”，为 false 时采用“事件冒泡”。默认值为 false。
```html
<button id="btn" onclick="say()"></button>
<script>
    var btn = document.getElmentById('btn')
    var say = function () {
        console.log("Hello world!")
    }
    btn.addEventListener('click',say)
    btn.removeEventListener('click',say)
    
</script>
```

### IE 事件处理程序

IE9 以前的 IE浏览器使用的是与标准不一样的事件处理程序。

```html
<button id="btn" onclick="say()"></button>
<script>
    var btn = document.getElmentById('btn')
    var say = function () {
        console.log("Hello world!")
    }
    btn.attachEVent('onclick',say)
    btn.detachEvent('onclick',say)
    
</script>
```
另外处理程序的作用域也不一致，标准事件处理程序是在处理程序内，而 IE9 以前的版本为全局。


```html
<button id="btn" onclick="say()"></button>
<script>
    var btn = document.getElmentById('btn')
  
    btn.attachEVent('onclick',function (){
        console.log(this) // window
    })
    
</script>
```
attachEVent 和 addEventListener 一样可以添加过个事件，但是执行顺序与添加顺序相反。

为了兼容浏览器，我们需要编写跨浏览器的脚步：


## 事件对象
### 标准事件对象
事件对象即事件源：
```html
<button id="btn"></button>
<script>
    var btn = document.getElmentById('btn')
    btn.onclick = function (event){
        console.log(event.type) // click
    }
</script>
currentTarget 和 target
```
停止捕获和冒泡：stopPropagation


阻止默认行为：preventDefault



### IE 事件对象

停止捕获和冒泡:cancelBubble

阻止默认行为：srcElment

## 事件类型
https://developer.mozilla.org/zh-CN/docs/Web/Events