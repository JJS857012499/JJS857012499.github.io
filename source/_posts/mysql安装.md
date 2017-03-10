---
title: mysql安装
date: 2017-03-10 17:31:27
categories: 云主机
tags:
 - mysql
 - Linux
---
## 一、安装编译工具及库文件
``` shell
yum -y install gcc gcc-c++ make autoconf libtool-ltdl-devel gd-devel freetype-devel libxml2-devel libjpeg-devel libpng-devel openssl-devel curl-devel bison patch unzip libmcrypt-devel libmhash-devel ncurses-devel sudo bzip2 flex libaio-devel
```
## 二、 安装cmake 编译器
cmake 版本：cmake-3.1.1。
1、下载地址：http://www.cmake.org/files/v3.1/cmake-3.1.1.tar.gz
``` shell
wget http://www.cmake.org/files/v3.1/cmake-3.1.1.tar.gz
tar zxvf cmake-3.1.1.tar.gz
cd cmake-3.1.1
./bootstrap
make && make install
```
## 三、安装 MySQL
MySQL版本：mysql-5.6.15。（注意：这里下载的应该是源码）
1、下载地址： http://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.35.tar.gz
``` shell
wget http://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.35.tar.gz
tar zxvf mysql-5.6.15.tar.gz
cd mysql-5.6.35
cmake -DCMAKE_INSTALL_PREFIX=/usr/server/mysql/ -DMYSQL_UNIX_ADDR=/tmp/mysql.sock -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DWITH_EXTRA_CHARSETS=all -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DWITH_INNODB_MEMCACHED=1 -DWITH_DEBUG=OFF -DWITH_ZLIB=bundled -DENABLED_LOCAL_INFILE=1 -DENABLED_PROFILING=ON -DMYSQL_MAINTAINER_MODE=OFF -DMYSQL_DATADIR=/usr/server/mysql/data -DMYSQL_TCP_PORT=3306
make && make install
```
2、查看版本
``` shell
/usr/server/mysql/bin/mysql --version
```
到此，mysql安装完成。

