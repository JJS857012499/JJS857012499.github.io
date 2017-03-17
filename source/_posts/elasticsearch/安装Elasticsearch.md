---
title: 安装Elasticsearch
date: 2017-03-17 14:43:41
category: Elasticsearch
tags: [Elasticsearch]
---
## 下载解压elasticsearch
<!--more-->
## 执行如下命令启动
```shell
./bin/elasticsearch
```
## 另外一个终端，查看
```shel
curl 'http://localhost:9200/?pretty'
{
  "name" : "fNc9QZL",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "S2Ac_4AfRpeFeCz6hXj3Ng",
  "version" : {
    "number" : "5.1.2",
    "build_hash" : "c8c4c16",
    "build_date" : "2017-01-11T20:18:39.146Z",
    "build_snapshot" : false,
    "lucene_version" : "6.3.0"
  },
  "tagline" : "You Know, for Search"
}
```
说明安装成功！
## 常见问题
### virtual memory areas vm.max_map_count [65530] is too low
```shell
max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```
这是由于 vm.max_map_count 太小引起的，可以使用
```shell
sysctl -w vm.max_map_count=262144
sysctl -a|grep "vm.max_map_count"
```
可以在sysctl.conf增加 vm.max_map_count=262144，然后使用
```shell
sysctl -p /etc/sysctl.conf
```
做永久调整

### max file descriptors [4096] for elasticsearch process is too low
```shell
max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
```
在Linux的系统中对于进程(Process)会有一些限制，你可以使用 ulimit -Sn 和 ulimit -Hn 查看软硬限制。
使用 root 帐号调整即可
```shell
#使用ulimit 命令可以分别查看软限制和硬限制，方法实在查看的参数前加 S 或 H。例如，查看打开文件数限制
ulimit -Sn #查看的是软限制
ulimit -Hn #查看的是硬限制
```
vim /etc/security/limits.conf
```shell
#添加如下配置
JJS soft nofile 65536
JJS hard nofile 65536
```
编辑后，重新登录JJS用户，通过ulimit查看，发现已经更改成功

### max number of threads [1024] for user [xx] is too low
```shell
max number of threads [1024] for user [JJS] is too low, increase to at least [2048]
```
```shell
#编辑
vim /etc/security/limits.d/90-nproc.conf
```
![logo](307afbba-50a9-49f0-8500-8e362ff809ae.png)
编辑后，重新登录JJS用户
