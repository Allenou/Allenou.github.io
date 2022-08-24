---
title: 日期格式与时间戳互转
date: 2016-04-26
categories: 厚积
tags: js
---
一直以为所有的时间都是以日期格式存取，直到后端小伙伴让我把日期格式转换成时间戳，我才知道有时间戳这么一回事。
<!-- more -->

>时间戳是指格林威治时间1970年01月01日00时00分00秒(北京时间1970年01月01日08时00分00秒)起至现在的总秒数。

我不太理解，明明不转换也可以，为什么要多此一举？于是我查了下时间戳和日期格式的区别：

**日期格式**：
1. 受时区影响
2. 字符串类型
3. 精确到秒

**时间戳**：
1. 不受时区影响
2. 数字类型
3. 精确到毫秒

比较了它们的区别后，终于知道为什么用时间戳会比用日期格式好了。当然只凭第一个区别并不能说服我，因为我们的客户还没达到遍布全球的程度，说服我的是后面两个区别。

举例：当活动列表需要按结束时间排序时就需要对比活动的结束时间，而数字类型的对比恰恰是最直接的（字符串类型得先隐式转换），然后涉及到时间自然是越精确越好，所以小伙伴使用时间戳是正确的。

我不明白，**php** 有提供转换方法，为什么非要前端来转换？他说转换后只能得到年份,好吧，那我就试试吧。

## 日期格式转时间戳

对于这种日期和时间都需要的我一般用的是 **HTML5** 新增的 **Datetime Local** 对象，它可以同时选择日期和时间（本地时区）：

```javascript
<input type="datetime-local" id="dateTime"/>
```
选择完后返回的日期格式:`2016-04-24T11:30`，中间的 **T** 是时间（TIME）的意思，~~转换时不能有它,否则转换后的时间戳不准确~~。这里应该不是不准确，而是转换后的时间比转换前的时间快了 8 个小时：

### Date.parse()
```javascript
new Date(Date.parse('2016-04-24T11:30'))//Sun Apr 24 2016 19:30:00 GMT+0800 (中国标准时间)
```
>Date.parse 方法用来解析日期字符串，返回距离1970年1月1日 00:00:00的毫秒数。

而没有 **T** 时是相同的：
```javascript
new Date(Date.parse('2016-04-24 11:30'))//Sun Apr 24 2016 11:30:00 GMT+0800 (中国标准时间)
```
所以可以断定，当参数含有 **T** 时，`Date.parse` 方法会默认其为世界标准时间，并将其转换成本地标准时间，而没有  **T** 时则默认其为本地标准时间，转换时便不会加上本地标准时间与世界标准时间的时间差。检验方法如下：
```javascript
new Date(Date.parse('2016-04-24T11:30')).toUTCString()//"Sun, 24 Apr 2016 11:30:00 GMT"
new Date(Date.parse('2016-04-24 11:30')).toUTCString()//"Sun, 24 Apr 2016 03:30:00 GMT"
```
>toUTCString 方法可以根据世界时，把 Date 对象转换为字符串

如上,因为参数中含有 **T** ，所以转换成世界时就是相等的，而参数中没有 **T** ，则指定的时间被当为中国标准时间，所以转换成世界标准时间时会减去 8 小时。
### Date.UTC()
```javascript
new Date(Date.UTC(2016, 04-1, 24, 11, 30, 0, 0))//Mon Apr 24 2016 19:30:00 GMT+0800 (中国标准时间)
```
>Date.UTC 返回从 1970-1-1 00:00:00 世界标准时（UTC）到指定日期的的毫秒数

可以发现，转换后的时间也比转换前的时间快 8 小时，之所以会出现这种问题也是因时区不一致所导致，检验方法如下：
```javascript
new Date(Date.UTC(2016, 04-1, 24, 11, 30, 0, 0)).toUTCString()//Sun, 24 Apr 2016 11:30:00 GMT
```
将时区统一成世界标准时间后便相同了。

## 时间戳转日期格式
虽然上面也介绍了时间戳转日期格式的方法，但是返回的格式并不雅观,我们想要的是 **2016-04-25** 或 **2016/04/25** 这样的格式，其实现方法如下：

```javascript
var date = new Date('2016-04-24 11:30');
var Y,D,M,h,m,s,ms;

Y = date.getFullYear() + '-';
M = (date.getMonth()+1 < 10 ? '0'+(date.getMonth()+1) : date.getMonth()+1) + '-';
D = date.getDate() + ' ';
h = date.getHours() + ':';
m = date.getMinutes() + ':';
s = date.getSeconds()+ ':';
ms = date.getMilliseconds();

console.log(Y + M + D + h + m + s + ms);//2016-04-24 11:30:0:0
```
上面的方式可以得到想要的日期格式，但太麻烦，我们可以用 `dateObj.toLocaleString` 方法根据所要的格式转换：
```javascript
new Date('2016-04-24 11:30').toLocaleString("zh-CN")//2016/4/24 上午11:30:00
```
中国默认是 12 小时制，24 小时制配置方法如下：
```javascript
new Date('2016-04-24 11:30').toLocaleString("zh-CN",{hour12:false})//2016/4/24 11:30:00
```
仅返回日期或仅返回时间：
```javascript
new Date('2016-04-24 19:30').toLocaleDateString("zh-CN")//2016/4/24
new Date('2016-04-24 19:30').toLocaleTimeString("zh-CN")//下午7:30:00
new Date('2016-04-24 19:30').toLocaleTimeString("zh-CN",{hour12:false})//11:30:00
```
附上各主要时区的 UTC ：http://time.is/UTC

面试被问到 `Date` 对象有什么兼容的问题的时候，我是懵逼的，从来没想过  `Date` 对象会存在兼容问题，同时我又是好奇的，所以回来便上 caniuse 查了查，在 ie 上还真有兼容问题，真是孤陋寡闻。
>参考：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/DateTimeFormat     
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleString
>扩展：http://javascript.ruanyifeng.com/stdlib/date.html   
https://segmentfault.com/q/1010000004279268
