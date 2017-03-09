---
title: CentOS6.5安装vncserver实现图形化访问
date: 2017-03-09 17:25:57
categories: 云主机
tags:
 - Linux
 - CentOS
 - vncserver
---
## 安装gnome图形化桌面
``` shell
yum groupinstall -y "X Window System"
yum groupinstall -y "Desktop"
yum groupinstall -y "Chinese Support"
```
## 安装vncserver并配置
### 安装vncserver
``` shell
yum install -y tigervnc-server
```
### 配置vncserver
- 配置为开机自启动
``` shell
chkconfig --level 345 vncserver on
```
- 配置vnc密码
``` shell
vncserver
You will require a password to access your desktop.
Password:
Verify:
```
- 配置为使用gnome桌面
修改 /root/.vnc/xstartup文件，把最后的 twm & 删掉 加上 gnome-session &。
- 配置vncserver启动后监听端口和环境参数
修改/etc/sysconfig/vncservers 文件添加以下内容
``` shell
VNCSERVERS="1:root"
# 桌面号:用户    监听 590* 端口
VNCSERVERARGS[1]="-geometry 1200x800"
```
- 重启vncserver服务
``` shell
service vncserver restart
```
## 允许root访问图形界面和生成新的machine-id
``` shell
sed -i 's/.*!= root.*/#&/' /etc/pam.d/gdm
dbus-uuidgen >/var/lib/dbus/machine-id
 ```
## 关闭selinux和NetworkManager服务
### 检查selinux服务并关闭
``` shell
vi /etc/selinux/config
```
确认里面的SELINUX字段的值是disabled，如果不是则改为disabled
### 关闭NetworkManager服务
``` shell
chkconfig --del NetworkManager
```
