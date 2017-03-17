---
title: maven安装
date: 2017-03-10 13:45:56
categories: 云主机
tags: [Linux,maven]
---
## 下载maven
[http://maven.apache.org/download.cgi](http://maven.apache.org/download.cgi)
<!-- more -->
## 解压
``` shell
tar -xzvf apache-maven-3.0.5-bin.tar.gz
```
## 配置环境变量
``` shell
sudo vim /etc/profile
```
## 添加
``` shell
#maven环境
export M2_HOME=/usr/java/maven/apache-maven-3.3.3
export M2=$M2_HOME/bin
export PATH=$PATH:$M2
#maven环境
```
## 保存后，生效
``` shell
source /etc/profile
```