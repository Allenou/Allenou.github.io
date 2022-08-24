title: 使用 FormData 上传图片
date: 2017-03-13 23:00:11
categories: 厚积
tags: 
- HTML5
- js
---
周六加班整了一天的图片上传，踩的坑有必要记录一下。
<!--more-->
图片上传，我在上家公司有做过，之前是将图片转换成 **base64** 后再用 **ajax** 上传的：

```javascript
export function photoCompress(photo, fileType) {
  let canvas = document.createElement("canvas");
  let ctx = canvas.getContext('2d')

  const width = photo.width,
    height = photo.height

  canvas.width = width
  canvas.height = height

  ctx.fillStyle = "#fff"
  ctx.fillRect(0, 0, canvas.width, canvas.height)
  ctx.drawImage(photo, 0, 0, width, height)

  const compressedBase64 = canvas.toDataURL(fileType, 0.2)
  canvas = ctx = null
  return compressedBase64
}

photoHandle(event) {
      let currentFile = event.currentTarget
      let file = currentFile.files[0]
      if (!/\/(?:jpeg|jpg|png)/i.test(file.type)) {
        this.toast.text = '不支持该格式文件'
        this.toast.show = true
        return
      }
      let reader = new FileReader()
      let vm = this
      let maxSize = 200 * 1024
      reader.onload = function() {
        let result = this.result

        if (result.length <= maxSize) {
          vm.photoPreview(currentFile, result) //预览
          return
        }
        let photo = new Image()
        photo.src = result
        photo.onload = function() {
          let compressedResult = photoCompress(photo, file.type); //压缩
          vm.photoPreview(currentFile, compressedResult); //预览
          // photo = null
        }
      }
      reader.readAsDataURL(file)
    },

```
这次因为后端接口只接收文件流，所以我是直接减不知为什么怎么也上传不成功，而直接在 **Swagger** 是可以的，
```html
<form id="uploadForm">
    <input type="file" (change)="upload($event)" />
</form>       
```
```javascript

upload(e){
 let data= e.targe.files[0]
  this.http.post(url,data).toPomise().then()
}
```