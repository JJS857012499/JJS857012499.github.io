---
title: Linux下解压xz格式文件
date: 2017-03-17 17:55:48
categories: 云主机
tags:
 - 下载
 - Linux
---
安装node的时候，下载到时xz格式的文件。记录下xz解压姿势
<!--more-->
```shell
xz -d ***.tar.xz
tar -xvf  ***.tar
```
 可以看到这个压缩包也是打包后再压缩，外面是xz压缩方式，里层是tar打包方式。
补充：目前可以直接使用 tar xvJf  ***.tar.xz 来解压