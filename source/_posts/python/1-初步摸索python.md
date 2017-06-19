---
title: 1 初步摸索python
date: 2017-06-19 10:55:26
category: python
tags: python
---
    如今python越来越火了，我也来玩玩python
<!-- more -->
## 首先安装python
这里就不累赘了，直接百度。我是下载解压包，然后把文件路径添加到path下即可
## 写个小demo运行
- 新建文件，后缀为.py,内容如下
``` python
print('Hello Python')
print('大家好，我叫小仙女')
```
- 运行如下
![logo](Q.png)
## 没错用命令行运行有点累，我用sublime编辑器，可以用ctrl+B快捷键运行；或者用PyCharm这个工具
- 我用sublime运行就报[Decode error - output not utf-8]这个错误，解决如下：打开Python.sublime-build文件,并添加"encoding":"cp936"这一行,保存即可,[解决方案来源](http://blog.csdn.net/wanzhuan2010/article/details/19168709)
- 运行截图
![logo](57G3PTFC9YBV`2CH9A.png)

