---
title: Flutter原理介绍
date: 2019-08-19 11:00:00
tags: 
    - iOS
    - flutter
category:
    - iOS
    - flutter
---

### Flutter原理篇


导读：对于开发者而言，Flutter的出现极大提高了开发效率，其跨平台的特性，深的广大开发者的青睐，那么Flutter究竟是如何工作的，是如何实现多平台的呢？接下来我们一起来探讨下Flutter的原理。


<!--[TOC]-->

#### 原理介绍

![](/image/flutter_theory.jpg)

这是Flutter官网提供的架构图，分为三层：Framework，Engine和Embedder。

__Flutter__是使用[Dart](https://baike.baidu.com/item/DART/22500518?fr=aladdin)语言实现的，Dart是谷歌开发的计算机编程语言，Dart是面向对象的、类定义的、单继承的语言。Dar语法简单，上手容易，建议初学者可以稍微学习下Dart语言，然后通过简单的Demo直接上手敲。

`Framework`包括系统库和第三方依赖库,使用dart实现，系统提供了支持2种UI风格，包括Material Design风格的Widget,Cupertino(针对iOS)风格的Widgets,以及文本/图片/按钮等基础Widgets、渲染、动画、手势等。
此部分的核心代码是：flutter仓库下的flutter package，以及sky_engine仓库下的io,async,ui(dart:ui库提供了Flutter框架和引擎之间的接口)等package。

`Engine`是通过C++实现的，主要涵盖Skia,Dart和Text。

* Skia是开源的二维图形库，提供了适用于多种软硬件平台的通用API。其已作为Google Chrome，Chrome OS，Android, Mozilla Firefox, Firefox OS等其他众多产品的图形引擎，支持平台还包括Windows7+,macOS 10.10.5+,iOS8+,Android4.1+,Ubuntu14.04+等。

* Dart部分主要包括:Dart Runtime，Garbage Collection(GC)，如果是Debug模式的话，还包括JIT(Just In Time)支持。Release和Profile模式下，是AOT(Ahead Of Time)编译成了原生的arm代码，并不存在JIT部分。
    

* Text即文本渲染，其渲染层次如下：衍生自minikin的libtxt库(用于字体选择，分隔行)。HartBuzz用于字形选择和成型。Skia作为渲染/GPU后端，在Android和Fuchsia上使用FreeType渲染，在iOS上使用CoreGraphics来渲染字体


`Embedder`是嵌入层，就是把Flutter部署到各个平台上，如iOS,安卓等，这一层的主要工作包括渲染Surface设置,线程设置，以及插件等。Flutter在这一层来处理平台的嵌入，降低平台相关层，平台依赖度底，实现了非常好的跨平台的一致性，如相当于iOS，原生只提供了一个画布，剩余所有的UI渲染、动画、布局、手势等等逻辑都由Flutter来完成。



