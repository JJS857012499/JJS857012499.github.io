---
title: ubuntu（firefox、chromium）安装flash插件
date: 2017-03-10 14:18:06
categories: 云主机
tags: [Linux,ubuntu,flash]
---
## firefox
1、首先到adobe官网下载插件[http://www.adobe.com/cn/downloads.html](http://www.adobe.com/cn/downloads.html)
2、然后用tar命令解压该文件
3、打开终端将libflashplayer.so复制到firefox的安装目录
4、重启firefox，安装成功
## chromium
``` shell
sudo apt-get install pepperflashplugin-nonfree
sudo update-pepperflashplugin-nonfree --install
```