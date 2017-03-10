---
title: ubuntu16的eclipse很卡
date: 2017-03-10 13:51:15
categories: 云主机
tags: [Linux,ubuntu,eclipse]
---
背景：在ubuntu桌面系统下，安装的elipse很卡，查看资源占有也不高，查阅资料后才发现这是elipse与ubuntu桌面程序不兼容导致的
## 解決方法一：
编辑设置 /usr/share/applications/Eclipse.desktop
``` shell
Exec=env SWT_GTK3=0 /opt/eclipse/eclipse #这行加入 env SWT_GTK3=0 就可以了
```
## 解決方法二：
在eclipse.ini文件中的 -vmargs 之前添加(一定要换行)
``` shell
--launcher.GTK_version
2
```
添加后如下：
``` shell
openFile
--launcher.appendVmargs
--launcher.GTK_version
2
-vmargs
-Dosgi.requiredJavaVersion=1.7
-XX:MaxPermSize=512m
-Xms512m
-Xmx2048m
```
来源：[https://bugs.launchpad.net/ubuntu/+source/java-common/+bug/1552764](https://bugs.launchpad.net/ubuntu/+source/java-common/+bug/1552764)