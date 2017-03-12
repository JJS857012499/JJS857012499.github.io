---
title: CentOS清理内存、内存回收释放及内存使用查看的相关命令
date: 2017-03-10 17:43:24
categories: 云主机
tags:
 - CentOS
 - Linux
---
在清理前内存使用情况
```shell
free -m
```
<!-- more -->
用以下命令清理内存
```shell
echo 1 > /proc/sys/vm/drop_caches
```
清理后内存使用情况再用以下命令看看。
```shell
free –m
```
多出很多内存了吧。

查看内存条数命令：
```shell
dmidecode |grep -A16 "Memory Device$"
```
