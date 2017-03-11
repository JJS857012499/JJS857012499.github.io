---
title: apache php环境搭建
date: 2017-03-11 11:01:21
categories:
tags:
 - apache
 - php
 - Linux
---

## 准备工作-下载所需软件
* Apache  httpd-2.2.22-win32-x86-openssl-0.9.8t.msi
* PHP       php-5.3.10-Win32-VC9-x86.zip
## 安装软件
### 安装Apache:
双击安装，与安装其他Windows软件没有什么区别，在填Server Infomation时，并没有特殊规定，只要输入的信息符合格式即可。
安装完成之后，在浏览器输入http://localhost，如果显示It Works!，表示Apache安装成功。
### 安装PHP:
将php-5.3.10-Win32-VC9-x86.zip解压到一个目录即可
## 整合Apache+PHP
### Apache : 首先修改Apache的配置文件，让Apache支持解析PHP文件。Apache配置文件在Apache安装目录的conf目录下的httpd.conf。
1. 让Apache可以解析php文件，在配置文件中找到
``` shell
LoadModule vhost_alias_module modules/mod_vhost_alias.so
```
在下一行添加 (文件的位置是根据PHP的所在目录而定的)
```
#php5_module start
LoadModule php5_module "D:/work/soft/phptools/php-5.4.45/php5apache2_2.dll"
PHPIniDir "D:/work/soft/phptools/php-5.4.45"
AddType application/x-httpd-php .php .html .htm
#php5_module end
```
2. 在配置文件中找到
```
DirectoryIndex index.html
```
 改为
 ```
DirectoryIndex index.php index.html
```
3. 修改Apache站点目录，在配置文件中找到(Apache安装的目录不同，显示的值不一样)
```
DocumentRoot "D:/work/soft/phptools/Apache2.2/htdocs"
```
改为
```
DocumentRoot "D:/work/soft/phptools/www"　
```
再找到
```
<Directory "D:/work/soft/phptools/Apache2.2/htdocs">
```
改为
```
<Directory "D:/work/soft/phptools/www">　
```
### PHP : 把php.ini-development改名为php.ini，作为PHP的配置文件。修改php.ini
 1. 设置PHP扩展包的具体目录，找到
```
; On windows:
; extension_dir = "ext"
```
改为 (值是ext文件夹的目录)
```
; On windows:
extension_dir = "D:\work\soft\phptools\php-5.4.45\ext"
```
2. 开启相应的库功能，找到需要开启的库的所在行
```
;extension=php_curl.dll
;extension=php_gd2.dll
;extension=php_mbstring.dll
;extension=php_mysql.dll
;extension=php_xmlrpc.dll
```
去掉前面的分号(注释)，即改为
```
extension=php_curl.dll
extension=php_gd2.dll
extension=php_mbstring.dll
extension=php_mysql.dll
extension=php_xmlrpc.dll ;这个配置文件没有，dll文件存在，rpc协议用到
```
3. 设置时区，找到
```
;date.timezone =
```
 改为
```
date.timezone = Asia/Shanghai
```
配置完成，检测一下配置是否成功。重启Apache，在站点目录下新建文件index.php，输入内容：
```
<?php
phpinfo();
?>
```
打开浏览器输入http://localhost，测试。
