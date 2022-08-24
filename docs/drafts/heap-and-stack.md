title: 堆栈
date: 2019-02-21 19:50:05
categories: 厚积
tags:
 - browser
 - js
thumbnail: /images/heap-and-stack.jpg
---
声名一个变量并赋值可能谁都会，但是要问这些变量和值有什么关系，他们存储在哪里可能就有人答不上了。只知其然肯定没有知其然又知其所以然有竞争力，万千面试大军中脱引而出的必定有其有过人之处。
<!--more-->
<!-- 
# 何为堆栈
Javacript 在运行时会分将程序中的数据（变量、函数等）进行存储，内存空间可分为堆（heap）和栈（stack）。其中栈由浏览器分配，其内存大小是固定的，且会自动释放，而堆则是程序运行时动态分配的，不会自动释放?（闭包）。 -->

# 堆（heap）
操作慢，必须清理，指向它的指针存储在栈（stack）中


# 栈（stack）
（后进先出）
栈溢出
call stack

堆内存中存储的是引用类型的数据（Object、Function、Array），栈内存中存储的是值类型的数据（String、Number、Boolean）以及引用类型数据的访问地址。

将基本类型的值赋给一个变量时会在栈空间（Stack）新开辟一块空间存储，而将引用类型的值赋给一个变量时会开辟一块空间存储在栈内存的是其值在堆内存（Heap）的引用

在对声名的变量赋值时，若值为基本类型是存储在栈内存，访问该变量时按值访问；若值为引用类型则存储在栈内存的是其值在堆内存（Heap）的引用，访问该变量时按引用访问。





https://lucasfcosta.com/2017/02/17/JavaScript-Errors-and-Stack-Traces.html?utm_source=javascriptweekly&utm_medium=email
https://blog.csdn.net/xdd19910505/article/details/41900693
https://itnext.io/how-javascript-works-in-browser-and-node-ab7d0d09ac2f