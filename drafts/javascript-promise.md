title: JavaScript 学习笔记：Promise 
date: 2017-03-25 13:54:29
tags:
---
<!--more-->
做图片预览的时候遇到一个问题:
```html
<img src="" id="uploadPreview">
<input type="file" id="uploadImage">
```
```javascript
var uploadImage = document.querySelector('#uploadImage'),
    uploadPreview = document.querySelector('#uploadPreview')

uploadImage.addEventListener('change', function(e) {
    var stream=e.currentTarget.files[0]
    var reader = new FileReader()

    reader.onload = function() {
        var result = this.result
        uploadPreview.setAttribute('src', result)
    }
    reader.readAsDataURL(stream)
})
```
上面的代码虽然可以实现预览，但是复用性太差，怎样提高复用性呢？我想到的第一个办法：
```javascript
function imagePreview(stream,image){
 var reader = new FileReader()
    reader.onload = function() {
        image.setAttribute('src', this.result)
    }
    reader.readAsDataURL(stream)
}
var uploadImage = document.querySelector('#uploadImage'),
    uploadPreview = document.querySelector('#uploadPreview')

uploadImage.addEventListener('change', function(e) {
    var stream=e.currentTarget.files[0]
    imagePreview(stream,uploadPreview)
})
```
复用性是提高了，但是还是太过耦合，于是我想到把结果返回：
```javascript
function imagePreview(stream){
 var result 
 var reader = new FileReader()
    reader.onload = function() {
        result = this.result
    }
    reader.readAsDataURL(stream)
    return result
}
imagePreview() //undefined
```
然而因为 `onload` 方法是异步执行的原因，返回的是 `undefined`。我实在没辙了，便请教了小伙伴，小伙伴说可以用  **Promise**。

 **Promise** 是我下一步的学习计划，虽然之前瞄过两眼，但基本已经忘完了，那就趁这个机会提前学了吧。

 **Promise** 出现的目的就是为了解决此类问题，它最早出现在 E 语言中，JS 则需要通过一些三方库才能使用，所幸 ES6 已提供原生支持。
 
 **Promise** 有以下三种状态：

* pending: 初始状态，未履行或拒绝。
* fulfilled: 意味着操作成功完成。
* rejected: 意味着操作失败。

使用时需要实例化 **Promise** 对象，实例化 **Promise** 对象接收一个回调函数，回调函数接收两个参数，分别是 `resolve` 和 `reject`,它们的作用分别是将 **Promise** 对象的状态从 *pending*  改为 *fulfilled* 和 *rejected* ，如下：

### then
```javascript
var promise = new Promise(function(resolve, reject) {
	if(/* 操作成功 */) {
		resolve('Success!');
	}
	else {
		reject('Failure!');
	}
});

promise.then(function(result) { 
	/* 根据返回结果处理相应业务 :)*/
})

```
`resolve` 和 `reject` 都是返回结果，它们有什么区别呢？我们看一个队列的列子：
```javascript
fn1().then(fn2).then(fn3).then(function(result){
    console.log('result:'+result);
})
function fn1(){
    return new  Promise(function (resolve,reject){
        resolve('fn1')
    })
}
function fn2(){
    return new  Promise(function (resolve,reject){
        reject('fn2')
    })
}
function fn3(){
    return new  Promise(function (resolve,reject){
        resolve('fn3')
    })
}
// output->error:fn2
```   



可以看出，`resolve` 返回结果后，下一个函数仍能执行，而 `reject` 返回结果后，下一个函数不再执行，其结果会在 `catch` 打印出来，有点 `return` 的感觉。还有另一种写法：
```javascript
fn1().then(fn2).then(fn3).then(function(result){
    console.log('result:'+result);
},function(error){
    console.log('error:'+error);
})
```
### all
``all`` 方法接收一个 **Promise** 对象数组，其特点是在需要等所有的 **Promise** 对象的状态从 *pending* 变为 *fulfilled* ，才会进入处理。（假设所有状态均为 *fulfilled*）
```javascript
var p1 = new Promise(function(resolve, reject) {
      setTimeout(resolve, 500, "one");
  });
  var p2 = new Promise(function(resolve, reject) {
      setTimeout(resolve, 100, "two");
  });

  Promise.race([p1, p2]).then(function(value) {
    console.log(value); // ["one", "two"]
  });

```
### race
``race`` 方法和``all`` 方法一样，都接收一个 **Promise** 对象数组，其特点是只要有一个 **Promise** 对象的状态从 *pending* 变为 *fulfilled* 和或 *rejected*，下一个 **Promise** 对象便不再执行，直接进入处理。
```javascript
var p1 = new Promise(function(resolve, reject) {
      setTimeout(resolve, 500, "one");
  });
  var p2 = new Promise(function(resolve, reject) {
      setTimeout(resolve, 100, "two");
  });

  Promise.race([p1, p2]).then(function(value) {
    console.log(value); // "two"
  });

```
### catch

```javascript
  function imagePreview(stream) {
            return new Promise (function(resolve, reject) {
                var reader = new FileReader()
                var result
                reader.onload = function() {
                    resolve(this.result)
                }
                reader.readAsDataURL(stream)
            })
        }
        var uploadImage = document.querySelector('#uploadImage')
        var uploadPreview = document.querySelector('#uploadPreview')
        uploadImage.addEventListener('change', function(e) {
            var stream = e.currentTarget.files[0]
            imagePreview(stream).then(function(result) {
              uploadPreview.setAttribute('src', result)
            })
        })
```


https://github.com/vuejs/vue-devtools/issues/62
https://davidwalsh.name/Promise s
http://javascript.ruanyifeng.com/advanced/Promise.html
http://www.jianshu.com/p/410f9722546e
http://jsfiddle.net/2bjt7Lon/
https://developers.google.com/web/fundamentals/getting-started/primers/Promise s
http://aiyouu.net/data-uris-explained/
http://www.jianshu.com/p/17d7e5ddf10a
https://developers.google.com/web/fundamentals/getting-started/primers/promises?hl=zh-cn
http://www.html-js.com/article/Learn-JavaScript-every-day-to-understand-what-JavaScript-Promises
https://wohugb.gitbooks.io/promise/content/usage/race.html