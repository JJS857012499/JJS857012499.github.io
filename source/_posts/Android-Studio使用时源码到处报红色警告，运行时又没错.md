---
title: Android Studio使用时源码到处报红色警告，运行时又没错
date: 2017-03-13 22:31:10
categories: android
tags: [android,android studio]
---
在AS上开发时，遇到这个问题，打开所有的java源文件，右侧一路标红色，找不到类，到不到方法，因为不能点击跳转，开发时纠结了好久，试了clean、rebuild等各种方法都不起作用，又去网上找大牛们支的招，可能对我不起作用。又有同事只招说换一个AS安装，觉得没必要，太麻烦。后研究参考网上的各种方法，现将自己亲身体验并解决过的一个小技巧附上，以免在遇到此类的问题时慌乱，找错了问题解决的方向。（ps：此方法是参考网上一个问题解决的，但找不到原网址了，所以自己在此写了一下，仅供大家参考）
<!-- more -->

1、点击File->Invalidate Caches / Restart...
2、重启Gradle，清除缓存
3、AS重启后java不再“到处”爆红
相关推荐
C# 引用其他命名空间的公共变量时的警告：由于＂***＂是引用封送类的字段，访问上面的成员可能导致运行时异常
在 ActionScript 3.0 中，无论是在严谨模式下还是在警告模式下编译，都将进行运行时类型检查
引用 Enterprise Library 5.0 时的一个警告和运行时错误及解决办法
QT运行时加载UI文件产生的Designer警告
警告：由于xxx是引用封送类的字段，访问上面的成员可能导致运行时异常
Eclipse在运行时出现的rmi activation 警告
27、获取运行时信息（包括运行时的service、运行任务、正在运行的进程信息）
运行时生成instances+运行时调用methods+运行时变更fields内容
怎样才能让程序在XP启动图形界面之前运行？ 就像启动时自动运行的Chkdsk命令或convert(将C盘转换为NTFS格式时)命令的运行时机一样？

来源： [http://www.07net01.com/program/2016/04/1452749.html](http://www.07net01.com/program/2016/04/1452749.html)