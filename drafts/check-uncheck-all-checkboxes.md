---
title: checkbox 全选反选
date: 2016-05-24
categories: 厚积
tags: js
---
一般做的比较灵活的购物车页面，可以增加、减少购买数量，以及可选择单件商品或多件商品购买。以下内容是关于如何实现对商品的选择。
<!--more-->
先分析购物车中对于商品有几种选择情况：

1. 全部商品勾选（全选框勾选）
2. 全部商品未勾选（全选框未勾选）
3. 部分商品勾选（全选框未勾选）

```html
<input type="checkbox" class="checkItem">
<input type="checkbox" class="checkItem">
<input type="checkbox" id="checkAll">
```
首先是简单的 HTML.

### 全选
```javascript
//attr
$('#checkAll').change(function(){
    $('.checkItem').attr('checked',true);
});

//prop
$('#checkAll').change(function(){
    $('.checkItem').prop('checked',true);
});
```
上面展示了两种实现全选的方法,根据实际情况我们应该选第二种（**prop**）。至于为什么，请戳 [jQuery 中 attr() 和 prop() 方法的区别]。

### 反选
```javascript
$('#checkAll').change(function(){
    $('.checkItem').prop('checked',false);
});
```
### 部分选
已经实现了情况 1 和 2，接下来是 情况 3（部分商品勾选）。

情况 3 比较特殊，它是包含了情况 1 和 2。

我的思路是每次勾选后都判断已勾选的商品个数,如果已勾选个数等于全部商品个数则说明已经全部勾选，反之则未全部勾选。如下：

```javascript
var checkItem= $('.checkItem');
var checkItemLength=checkItem.length;
var checkAll= $('#checkAll');
var checkedAmount;

function selectAmount(){
  checkedAmount=0
   for (var i = 0; i < checkItemLength; i++) {
       if (checkItem.eq(i).prop('checked')) {
           checkedAmount++;
         }
   }
}

checkItem.change(function(){
  selectAmount();
  if (checkedAmount == checkItemLength) {
      checkAll.prop('checked', true);
  } else {
      checkAll.prop('checked', false);
  }
});
```
显然这种方法要比上面单独控制全选和反选的方法科学，所以全选和反选应该使用同一个方法控制。

### 全选&&反选

```javascript
checkAll.change(function(){
    selectAmount();
    if (selectAmount == checkItemLength) {
        checkItem.prop('checked', false);
        checkAll.prop('checked', false);
    } else {
        checkItem.prop('checked', true);
        checkAll.prop('checked', true);
      }
});
```
最终效果：![](http://7xopm5.com1.z0.glb.clouddn.com/2016/05/24/9d60f1eded212cbce205887498061c1e.gif)

[jQuery 中 attr() 和 prop() 方法的区别]:https://github.com/JChehe/blog/blob/master/posts/jQuery%20%E7%9A%84%20attr%20%E4%B8%8E%20prop%20%E7%9A%84%E5%8C%BA%E5%88%AB.md
