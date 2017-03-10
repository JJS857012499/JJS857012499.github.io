---
title: 虚拟机中linux系统启动的tomcat无法在本机访问的问题
date: 2017-03-10 17:00:04
categories: 虚拟机
tags:
 - Tomcat
 - Linux
 - 防火墙
---
背景：在wmware中安装linux后安装好数据库，JDK及tomcat后启动服务，虚拟机中可以访问，但是主机却无法访问，但是同时主机和虚拟机之间可以ping的通。
## 网上查阅资料后
### 第一种解决方法是关闭虚拟机中的防火墙服务。桌面--管理--安全级别与防火墙，将防火墙设置为：disable即可。或用root登录后，执行
``` shell
service iptables stop --停止
service iptables start --启动
```
### 第二种解决方案：
第一种解决方案是相当于端口全部开放，这样难免会降低服务器的安全性。因此，既然可以更改全部端口，则因此也会存在更改局部端口开发关闭的设置。
局部端口的开发。
其实，则就是我们第二种解决方案：
修改Linux系统防火墙配置需要修改 /etc/sysconfig/iptables 这个文件，如果要开放哪个端口，在里面添加一条
``` shell
-A RH-Firewall-1-INPUT -m state --state NEW -m tcp -p tcp --dport 8080 -j ACCEPT
```
就可以了，其中 8080 是要开放的端口号，然后重新启动linux的防火墙服务，
``` shell
/etc/init.d/iptables restart
```
原文：[http://blog.sina.com.cn/s/blog_8e5354210101koo3.html](http://blog.sina.com.cn/s/blog_8e5354210101koo3.html)
