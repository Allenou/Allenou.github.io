
---
title: 虚拟 DOM
date: 2021-05-05
---

diff 优势
跨平台
只对改变的 dom 操作

属性：

tag
attrs
chirden


与真实 DOM 优势：

真实 DOM 属性多，
虚拟 DOM 是一个 JS 对象
产生重会少
虚拟 DOM 是数据驱动，真实 DOM 是 手动驱动
大量减少 DOM 多更新
不用维护大量状态
新旧 虚拟DOM 树对比（diff）找出差异以最新多代价更新真实 DOM


vue：
每个组件都有通过 Object.defineProperty 对 data 内对属性建立 getter/setter， watcher，不会遍历所有子组件


react：
setState
fiber 之前从根组件开始遍历所有子组件，在 shouldComponentUpdate 标记了 false 则跳过