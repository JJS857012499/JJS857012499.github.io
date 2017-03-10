---
title: nginx安装
date: 2017-03-10 17:12:49
categories: 云主机
tags:
 - nginx
 - Linux
---
## 安装编译工具及库文件
``` shell
yum -y install make zlib zlib-devel gcc-c++ libtool  openssl openssl-devel
```
## 首先要安装 PCRE
PCRE 作用是让 Ngnix 支持 Rewrite 功能。
### 1、下载 PCRE 安装包，下载地址： [http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz](http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz)
``` shell
wget http://downloads.sourceforge.net/project/pcre/pcre/8.35/pcre-8.35.tar.gz
tar zxvf pcre-8.35.tar.gz
cd pcre-8.35
./configure --prefix=/usr/server/pcre
make && make install
```
备注：./configure 可以添加--prefix指定安装路径
### 2、查看PCRE版本
``` shell
pcre-config --version
```
## 安装 Nginx
### 1、下载 Nginx，下载地址：http://nginx.org/download/nginx-1.6.2.tar.gz
``` shell
wget http://nginx.org/download/nginx-1.6.2.tar.gz
tar zxvf nginx-1.6.2.tar.gz
cd nginx-1.6.2
./configure --prefix=/usr/server/nginx-1.6.2 --with-http_stub_status_module --with-http_ssl_module --with-pcre=/tmp/pcre-8.35
make
make install
```
>备注：configure的--with-pcre指定的参数并非是安装后的pcre路径，而是pcre的安装前的源代码路径
### 2、查看Nginx版本
``` shell
/usr/server/nginx-1.6.2/sbin/nginx -v
```
到此，nginx安装完成。



