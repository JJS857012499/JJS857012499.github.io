---
title: ActivityMq安装
date: 2017-03-17 15:05:40
category: mq
tags: [ActivityMq]
---
## 下载
```shell
wget http://archive.apache.org/dist/activemq/5.11.1/apache-activemq-5.11.1-bin.tar.gz
```
<!--more-->
## 解压，启动
```shell
./activemq start
```
## 测试启动是否成功（ActiveMQ默认监听61616端口，查此端口看看是否成功启动）
```shell
netstat -an|grep 61616
```
## 登录下管理员页面，看看有木有问题：
URL :http://192.168.203.128:8161/admin
默认账户密码都是admin
## 账户密码修改在conf文件夹下的jetty-realm.properties

