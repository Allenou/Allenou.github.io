---
title: 语义化
date : 2017-02-24
---
# 语义化
- HTML5 新增了哪些语义化标签？（2017-02-21）

语义化就是一看就知道是干什么的，使用语义化同时有利于 SEO。不过在我看来最大的好处是不用为起名而苦恼了，要知道大部门的编程时间不是用在功能实现上而是用在怎么起名上了。

HTML5 增加的标签有点多，下面我只列举我经常用到的：

## 章节   
`<section>`：定义文档中的一个章节  
`<nav>`：定义只包含导航链接的章节  
`<article>`：定义可以独立于内容其余部分的完整独立内容块   
`<aside>`：定义和页面内容关联度较低的内容——如果被删除，剩下的内容仍然很合理   
`<header>`：定义页面或章节的头部。它经常包含 logo、页面标题和导航性的目录  
`<footer>`：定义页面或章节的尾部。它经常包含版权信息、法律信息链接和反馈建议用的地址   
`<main>`：定义文档中主要或重要的内容

关于这部分可能用一张图说明更容易理解：

![](/better/html/html5-tags.png)

## 文字形式
`<time>`：代表日期 和时间 值；机器可读的等价形式通过 datetime 属性指定   

## 嵌入内容
`<video>`： 代表一段视频 及其视频文件和字幕，并提供了播放视频的用户界面    
`<audio>`：代表一段声音 ，或音频流    
`<source>`：为 `video` 或 `audio` 这类媒体元素指定媒体源  
`<canvas>`：代表位图区域 ，可以通过脚本在它上面实时呈现图形，如图表、游戏绘图等   
`<svg>`：定义一个嵌入式矢量图    


## 表单
`<datalist>`：代表提供给其他控件的一组预定义选项   

<template>
  <section>
    <label for="search">三剑客:</label>
    <input list="list" id="search" />
    <datalist id="list" >
        <option value="HTML">HTML</option>
        <option value="CSS">CSS</option>
        <option value="Javascript">Javascript</option>
    </datalist> 
  </section>
</template>

`<progress>`：代表进度条    

<template>
  <section>
    <progress value="22" max="100"></progress> 
  </section>
</template>

`<meter>`：代表滑动条

<template>
  <section>
    <meter value="3" min="0" max="10">3/10</meter>
  </section>
</template>

## 交互元素

`<menuitem>`：代表一个用户可以点击的菜单项     
`<menu>`：代表菜单
