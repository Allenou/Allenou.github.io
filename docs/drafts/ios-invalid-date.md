---
title: iOS invalid date
date: 2019-06-28
---
在 iOS 浏览器（微信、Safi、Chrome）查看自己写的[日历组件]时，发现效果和安卓浏览器相差甚远。原来是因为 new Date('2019-09-14') 类似的日期在 iOS 上的浏览器中会识别不到，变为 NAN, new Date('NaN') 自然会变成 invalid date。

我突然就想起了之前面试被问过 new Date() 有什么兼容性的问题。

https://stackoverflow.com/questions/4310953/invalid-date-in-safari
https://blog.csdn.net/weixin_35955795/article/details/71178643
[日历组件]: https://www.toyou.xyz/vue-marker-calendar/