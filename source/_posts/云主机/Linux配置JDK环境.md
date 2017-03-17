---
title: Linux配置JDK环境
date: 2017-03-09 17:44:03
categories: 云主机
tags:
 - jdk
 - Linux
---
## 解压配置安装
``` shell
tar zxvf jdk-7u79-linux-x64.gz  jdk1.7.0_79
mv /alidata/backup/jdk1.7.0_79  /alidata/jdk/java7
```
<!-- more -->
## 添加jdk7.0到系统环境变量
``` shell
cp /etc/profile /etc/profile.bak #备份
vi /etc/profile #编辑,在最后添加下面的内容
```
``` shell
export JAVA_HOME=/alidata/jdk/java7
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=$PATH:${JAVA_HOME}/bin
```
## 使配置文件生效
``` shell
source /etc/profile 　　　#使配置文件立即生效
```