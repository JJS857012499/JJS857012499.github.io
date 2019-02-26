---
title: zookeeper初步了解
date: 2019-02-26 10:31:59
category: zookeeper
tags: [zookeeper]
---
## zookeeper是什么？
官方这样解释：
ZooKeeper is a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services. All of these kinds of services are used in some form or another by distributed applications. Each time they are implemented there is a lot of work that goes into fixing the bugs and race conditions that are inevitable. Because of the difficulty of implementing these kinds of services, applications initially usually skimp on them, which make them brittle in the presence of change and difficult to manage. Even when done correctly, different implementations of these services lead to management complexity when the applications are deployed.

> 大概的说，zookeeper是一个分布式框架，主要用于解决分布式应用遇到的一些数据管理问题。如维护配置信息，统一命名，状态同步，集群管理

## 简单点来说，zookeeper就是文件系统+通知监听机制
<!-- more -->

### 文件系统
Zookeeper维护一个类似文件系统的数据结构：
```
 ──root
    ├─Zookeeper
    ├─Config
    ├─NameServer
    │  └─serverA
    │  └─serverB
    │  └─serverC
    ├─App
    │  ├─App1
    │  └─App2
```

每个子目录项如 NameService 都被称作为 znode(目录节点)，和文件系统一样，我们能够自由的增加、删除znode，在一个znode下增加、删除子znode，唯一的不同在于znode是可以存储数据的。

#### 有四种类型的znode：
```
PERSISTENT-持久化目录节点: 客户端与zookeeper断开连接后，该节点依旧存在
PERSISTENT_SEQUENTIAL-持久化顺序编号目录节点: 客户端与zookeeper断开连接后，该节点依旧存在，只是Zookeeper给该节点名称进行顺序编号
EPHEMERAL-临时目录节点: 客户端与zookeeper断开连接后，该节点被删除
EPHEMERAL_SEQUENTIAL-临时顺序编号目录节点: 客户端与zookeeper断开连接后，该节点被删除，只是Zookeeper给该节点名称进行顺序编号
```

### 监听通知机制
客户端注册监听它关心的目录节点，当目录节点发生变化（数据改变、被删除、子目录节点增加删除）时，zookeeper会通知客户端。

## Zookeeper能做什么
zookeeper功能非常强大，可以实现诸如分布式应用配置管理、统一命名服务、状态同步服务、集群管理等功能，我们这里拿比较简单的分布式应用配置管理为例来说明。
![logo](UKIVH7`HQONREA8$8279.png)
假设我们的程序是分布式部署在多台机器上，如果我们要改变程序的配置文件，需要逐台机器去修改，非常麻烦，现在把这些配置全部放到zookeeper上去，保存在 zookeeper 的某个目录节点中，然后所有相关应用程序对这个目录节点进行监听，一旦配置信息发生变化，每个应用程序就会收到 zookeeper 的通知，然后从 zookeeper 获取新的配置信息应用到系统中。

## 单机操作
下载并解压到当前目录，最直接的是单机模式启动，由于jar包，我们需要java环境
### 启动前，我们需要一个配置文件./conf/zoo.cfg. 这里有个例子：./conf/zoo_sample.cfg

```
tickTime=2000
#initLimit=10
#syncLimit=5
dataDir=/tmp/zookeeper
clientPort=2181
```
> tickTime: 这个是zookeeper与客户端维持心跳的时间间隔（单位：毫秒），也就是每一次tickTime会发送一次心跳
initLimit：这个配置项是用来配置zookeeper用来接受客户端（这里的客户端并非用户连接到zookeeper的客户端，而是zookeeper服务器集群中连接到leaderde的follow服务器）初始化连接时最长能忍受多少个心跳时间间隔数，当已经超过十个心跳时间（tickTime）长度后，zookeeper服务器还没收到客户端的返回信息，那么表明这个客户端连接失败，总的时间长度=10*2000=20秒 （单机可以不用）
syncLimit：这个配置项标识leader与follow之间发送消息，请求与应答时间长度，最长不能超过多少个tickTime长度，总的时间长度=5*2000=10秒（单机可以不用）
dataDir: 缓存内存数据的快照，默认写数据日志文件也在这个目录
clientPort：客户端监听端口号

### 启动
```
bin/zkServer.sh start
bin/zkServer.sh status #查看状态
```

### 连接到zookeeper
```
bin/zkCli.sh -server 127.0.0.1:2181 #连接到zookeeper
[zk: 127.0.0.1:2181(CONNECTED) 29] help  #获取帮助文档
ZooKeeper -server host:port cmd args
        stat path [watch]
        set path data [version]
        ls path [watch]
        delquota [-n|-b] path
        ls2 path [watch]
        setAcl path acl
        setquota -n|-b val path
        history
        redo cmdno
        printwatches on|off
        delete path [version]
        sync path
        listquota path
        rmr path
        get path [watch]
        create [-s] [-e] path data acl
        addauth scheme auth
        quit
        getAcl path
        close
        connect host:port
[zk: 127.0.0.1:2181(CONNECTED) 16] ls /  #查看根节点
[zookeeper]
[zk: 127.0.0.1:2181(CONNECTED) 22] create /config config_data #创建节点
Created /config
[zk: 127.0.0.1:2181(CONNECTED) 24] get /config # 获取节点信息
config_data
cZxid = 0x9
ctime = Mon Feb 25 09:06:21 GMT 2019
mZxid = 0x9
mtime = Mon Feb 25 09:06:21 GMT 2019
pZxid = 0x9
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 11
numChildren = 0
[zk: 127.0.0.1:2181(CONNECTED) 25] set /config config_data1  #更新节点信息（dataVersion会增加，dataLength根据内容变化）
cZxid = 0x9
ctime = Mon Feb 25 09:06:21 GMT 2019
mZxid = 0xa
mtime = Mon Feb 25 09:06:52 GMT 2019
pZxid = 0x9
cversion = 0
dataVersion = 1
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 12
numChildren = 0
```

## 集群模式运行
### 修改配置文件zoo--1.cfg，zoo-2.cfg,zoo-2.cfg,原配置文件里有的，修改成下面的值，没有的则加上

zoo-1.cfg
```
tickTime=2000
syncLimit=5
dataDir=/tmp/zookeeper-1
clientPort=2181
server.1=127.0.0.1:2888:3888
server.2=127.0.0.1:2889:3889
server.3=127.0.0.1:2890:3890
```

zoo-2.cfg
```
tickTime=2000
syncLimit=5
dataDir=/tmp/zookeeper-2
clientPort=2181
server.1=127.0.0.1:2888:3888
server.2=127.0.0.1:2889:3889
server.3=127.0.0.1:2890:3890
```

zoo-3.cfg
```
tickTime=2000
syncLimit=5
dataDir=/tmp/zookeeper-3
clientPort=2181
server.1=127.0.0.1:2888:3888
server.2=127.0.0.1:2889:3889
server.3=127.0.0.1:2890:3890
```
对于serverd,官方文档有这样一句话
```
Every machine that is part of the ZooKeeper ensemble should know about every other machine in the ensemble. You accomplish this with the series of lines of the form server.id=host:port:port. The parameters host and port are straightforward. You attribute the server id to each machine by creating a file named myid, one for each server, which resides in that server's data directory, as specified by the configuration file parameter dataDir.
```
server.id=port1:port2
id : 服务标号标识 （1-255）
port1：follower与leader通讯用的端口号
port2: 选举用的端口号

### 标识server ID
创建三个文件夹/tmp/zookeeper-1，/tmp/zookeeper-2，/tmp/zookeeper-2，在每个目录中创建文件myid 文件，写入当前实例的server id，即1.2.3

### 启动三个zookeeper实例
```
$ bin/zkServer.sh start conf/zoo-1.cfg
$ bin/zkServer.sh start conf/zoo-2.cfg
$ bin/zkServer.sh start conf/zoo-3.cfg
```

### 检查三个实例的状态
```
$ ./bin/zkServer.sh status ./conf/zoo-1.cfg
ZooKeeper JMX enabled by default
Using config: ./conf/zoo-1.cfg
Mode: follower
$ ./bin/zkServer.sh status ./conf/zoo-2.cfg
ZooKeeper JMX enabled by default
Using config: ./conf/zoo-2.cfg
Mode: leader
$ ./bin/zkServer.sh status ./conf/zoo-3.cfg
ZooKeeper JMX enabled by default
Using config: ./conf/zoo-3.cfg
Mode: follower
```

至此，我们对zookeeper就算有了一个入门的了解，当然zookeeper远比我们这里描述的功能多，比如用zookeeper实现集群管理，分布式锁，分布式队列，zookeeper集群leader选举等等

