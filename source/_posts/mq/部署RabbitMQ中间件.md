---
title: 部署RabbitMQ中间件
date: 2018-05-14 11:48:31
category: mq
tags: [rabbitMQ]
---
## 安装编译工具
```jshelllanguage
yum install -y ncurses ncurses-base ncurses-devel ncurses-libs ncurses-static ncurses-term ocaml-curses ocaml-curses-devel
yum install -y openssl-devel zlib-devel
yum install -y make ncurses-devel gcc gcc-c++ unixODBC unixODBC-devel openssl openssl-devel
```

## 安装erlang
下载erlang：http://erlang.org/download/otp_src_20.0.tar.gz
```jshelllanguage
tar -zxvf otp_src_20.0.tar.gz

cd otp_src_20.0

./configure --prefix=/usr/local/erlang --with-ssl -enable-threads -enable-smmp-support -enable-kernel-poll --enable-hipe --without-javac

make && make install

ln -s /usr/local/erlang/bin/erl /usr/local/bin/erl

vi ~/.bashrc

ERLANG_HOME=/usr/local/erlang
PATH=$ERLANG_HOME/bin:$PATH

source ~/.bashrc

erl
```
## 安装rabbitmq
```jshelllanguage
http://www.rabbitmq.com/releases/rabbitmq-server/v3.6.12/rabbitmq-server-generic-unix-3.6.12.tar.xz

yum install -y xz

xz -d rabbitmq-server-generic-unix-3.6.12.tar.xz

tar -xvf rabbitmq-server-generic-unix-3.6.12.tar

mv rabbitmq_server-3.6.1 rabbitmq-3.6.12
```

### 开启管理页面的插件
```jshelllanguage
cd rabbitmq-3.6.1/sbin/
./rabbitmq-plugins enable rabbitmq_management
```

### 后台启动rabbitmq server
```jshelllanguage
./rabbitmq-server -detached
```

### 关闭rabbitmq server
```jshelllanguage
./rabbitmqctl stop
```

### 添加管理员账号
```jshelllanguage
./rabbitmqctl add_user rabbitadmin 123456
./rabbitmqctl set_user_tags rabbitadmin administrator

```
### 进入管理页面

15672端口号，输入用户名和密码


