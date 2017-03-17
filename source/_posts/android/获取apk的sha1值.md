---
title: 获取apk的sha1值
date: 2017-03-13 22:37:35
categories: android
tags: [android]
---
直接用打包出来的apk查看签名，具体如下：
1） 将apk修改后缀为 .zip文件后解压；
2） 进入解压后的META-INF目录，该目录下会存在文件CERT.RSA
3） 在该目录下打开cmd，输入命令 ：keytool -printcert -file CERT.RSA 这里将会显示出MD5和SHA1签名。
