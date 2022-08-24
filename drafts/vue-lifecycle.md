---
title: Vue 实例生命周期
date: 2019-05-08
---
# Vue 实例生命周期
Vue 使用了这么久，对它的生命周期却没有一个系统的概念，今天来了解下 Vue2.x 版本的生命周期。

<Picture name="0.png"></Picture>

上图是 Vue 官网提供的生命周期图，从图中可以看出 Vue 的实例生命周期主要可以分为三个周期（创建->运行->销毁），实例从创建到销毁每到一个阶段都会触发一个相应的生命周期钩子函数：
- 创建阶段：beforeCreate、created、beforeMount、mounted
- 运行阶段：beforeUpdate、updated
- 销毁阶段：beforeDestroy、destroyed

## beforeCreate
> 在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。

此时无法访问 data、computed、methods：
```javascript
new Vue({
    data:{
        x:1
    },
    beforeCreate:function(){
        console.log(this.x) // undefined
    }
})
```
## created
> 在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测 (data observer)，属性和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。

此时可以访问 data、computed、methods，当无法访问 el 属性：
```javascript
new Vue({
    el:'#app',
    data:{
        x:1
    },
    template:'<div ref="demo">1</div>'
    created:function(){
        console.log(this.x) // 1
        console.log(this.$el) // undefined
    }
})
```
<Picture name="1.png"></Picture>

从上图可以看出，在此阶段之后 Vue 编译器会先判断使用哪个模板渲染。如果有 `template` 属性则使用 `template` 定义的模板，否则使用 `el` 属性选中的模板。
## beforeMount
> 在挂载开始之前被调用：相关的 render 函数首次被调用。

<!-- ::: warning
该钩子在服务器端渲染期间不被调用。
::: -->
## mounted
> el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内。


```javascript
new Vue({
    el:'#app'
    data:{
        x:1
    },
    template:'<div id="app">1</div>'
    created:function(){
        console.log(this.x) // 1
        console.log(this.$el.innerText) // undefined
    }
})
```
## beforeUpdate
数据更新前触发
## updated
数据更新后触发
## beforeDestroy
> 实例销毁之前调用。在这一步，实例仍然完全可用。


## destroyed
> Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。




> 参考文章：
[vue 生命周期深入]
https://segmentfault.com/a/1190000008010666
https://blog.niclin.tw/2017/10/08/%E6%90%9E%E6%87%82%E7%94%9F%E5%91%BD%E9%80%B1%E6%9C%9F-lifecycle/

[vue 生命周期深入]: https://juejin.im/entry/5aee8fbb518825671952308c

https://cythilya.github.io/2017/04/11/vue-instance/