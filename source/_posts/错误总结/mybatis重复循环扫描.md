---
title: mybatis重复循环扫描
date: 2017-06-26 15:51:16
category: 错误总结
tags: [mybatis,循环扫描]
---
今天遇到一个奇葩问题，项目跑起来，永无止境的循环扫描，扫描jar，解析dao，mapping文件等信息
最后发现是两个mapper文件的namespace重复了
![logo](KRU1MDPSH2JWHOM2Z68I.png)
借鉴：(http://orange2015.blog.163.com/blog/static/2458750472015029114652894/)[http://orange2015.blog.163.com/blog/static/2458750472015029114652894/]
