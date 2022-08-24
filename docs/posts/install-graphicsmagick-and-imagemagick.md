---
title: 安装 GraphicsMagick 和 ImageMagick
date: 2019-10-31
---
[gm](https://github.com/aheckmann/gm) 是 nodejs 的一个图片处理模块，它依赖于 GraphicsMagick 和 ImageMagick。
<!-- more -->
下面是我在 Windows 和 Centos 中安装这两个工具的步骤：

## Windows
Windows 中的安装步骤比较简单，到 [GraphicsMagick] 和 [ImageMagick] 官网下载相应的安装包后进行默认安装，需要注意的是安装成功后一定要重启电脑。

## Centos
Centos 中的安装步骤稍微复杂点。

### GraphicsMagick
找到[相应版本]的安装包后下载：

```html
wget https://sourceforge.net/projects/graphicsmagick/files/graphicsmagick/1.3.33/GraphicsMagick-1.3.33.tar.xz
```

下载成功后先解压：
```
cd GraphicsMagick-1.3.33
tar -xvf ./GraphicsMagick-1.3.33.tar.xz
```

然后配置：
```
./configure && make && make install
```

最后查看是否安装成功：
```
gm version
```
这一步如果没安装 gcc 则会提示：
```
configure: error: no acceptable C compiler found in $PATH 
See `config.log' for more details.
```
解决办法是安装 gcc：
```
yum install gcc
```
运行查询命令（`gm version`）后出现以下信息则说明安装成功：
<Picture name="0.png"></Picture>

### ImageMagick

ImageMagick 的安装步骤和 GraphicsMagick 差不多，首先下载安装包：
```
wget https://www.imagemagick.org/download/ImageMagick.tar.gz
```

下载成功后解压：
```
tar -xvf ./ImageMagick.tar.gz
```

然后配置：
```
cd ImageMagick
./configure && make && make install
```

最后查看是否安装成功：
```
magick -version
```

若安装成功则会显示如下信息：
<Picture name="1.png"></Picture>



[ImageMagick]: https://imagemagick.org/script/download.php#windows

[相应版本]: https://sourceforge.net/projects/graphicsmagick/files/graphicsmagick/