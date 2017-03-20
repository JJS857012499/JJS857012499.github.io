---
title: 批量删除.svn文件夹、.svn文件
date: 2017-03-20 16:58:34
category: 工具
tags: [svn]
---
　　用svn版本控制的项目，下载下来后，每个文件下都会有一个.svn文件。可以用如下方法删除
<!--more-->

1、Windows环境
将下面的代码保存为 kill-svn.bat文件，放到要删除.svn文件的目录下，双击运行即可
```shell
@echo on
@rem 删除SVN版本控制目录
@rem for /r . %%a in (.) do @if exist "%%a\.svn" @echo "%%a\.svn"
@for /r . %%a in (.) do @if exist "%%a\.svn" rd /s /q "%%a\.svn"
@echo completed
@pause
```
2、在linux下
```shell
find . -type d -name ".svn"|xargs rm -rf
```

来源：[http://blog.csdn.net/xinpo66/article/details/40145529](http://blog.csdn.net/xinpo66/article/details/40145529)