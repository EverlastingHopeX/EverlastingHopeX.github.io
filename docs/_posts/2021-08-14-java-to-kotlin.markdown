---
layout: post
title:  "Java to Kotlin"
date:   2021-08-14 15:04:41 +0800
categories: Java Kotlin
---

上学期间最常用的就是Java，对于Kotlin的了解仅限于使用Android Studio对Java直接转换成Kotlin。现在由于工作原因要学习Kotlin，就记录一下学习过程中遇到的比较好的文档等资源，和自己学习过程中的想法。

[Kotlin Documentation](https://kotlinlang.org/docs/home.html)

>Kotlin的官方文档，适合细致全面的了解，以及查漏补缺。可以用来验证网上可能存在错误的信息，或者当网上相关资讯较少时作为参考。

[菜鸟教程的Kotlin教程](https://www.runoob.com/kotlin/kotlin-tutorial.html)

>很方便的文档，提供了示例代码，适合用来初步了解Kotlin，不过对有其他语言基础的会显得比较繁琐。

[From Java to Kotlin](https://fabiomsr.github.io/from-java-to-kotlin/)

>适合有Java基础的人。从基础，函数和类三个方面用示例代码进行简单直观的比较。需要注意的是，有些Java和Kotlin示例并不能完全一一对应。

基于我初步的了解，Kotlin相对Java加了一些语法糖，使得程序更简洁。在这方面，Kotlin有一点Python的感觉，尤其是变量类型自动判断。

Kotlin相对Java除了语法的细微改变，有值得注意的几点。
1. Null检查机制。Kotlin的空安全设计（Null Safety）提供使用 `?` 和 `!!` 限定类型是否可为空，更好的处理了NullPointerException。
2. 可选参数（optional arguments）。Kotlin的可选参数避免了不必要的重载（overload），而在Java中没有可选参数。
3. 解构（destructuring）。Kotlin也提供了Java目前也不支持的解构，可用于将一个对象解构成多个变量来方便的获取对象的属性。
4. 类修饰符Open。Open是相对于final定义的，Kotlin中类默认是final的，即不可继承。Java中不加final则默认可以继承。
5. 单例（singleton）。使用object关键字来定义一个单例。