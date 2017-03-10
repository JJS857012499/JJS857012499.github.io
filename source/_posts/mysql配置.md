---
title: mysql配置
date: 2017-03-10 17:33:59
categories: 云主机
tags:
 - mysql
 - Linux
---
## 1、创建mysql运行使用的用户mysql：
``` shell
/usr/sbin/groupadd mysql
/usr/sbin/useradd -g mysql mysql
```
## 2、创建binlog和库的存储路径并赋予mysql用户权限
``` shell
mkdir -p /usr/server/mysql/binlog /www/data_mysql
chown mysql.mysql /usr/server/mysql/binlog/ /www/data_mysql/
```
## 3、创建my.cnf配置文件
将/etc/my.cnf替换为下面内容
``` shell
[client]
port = 3306
socket = /tmp/mysql.sock
[mysqld]
replicate-ignore-db = mysql
replicate-ignore-db = test
replicate-ignore-db = information_schema
user = mysql
port = 3306
socket = /tmp/mysql.sock
basedir = /usr/server/mysql
datadir = /www/data_mysql
log-error = /usr/server/mysql/mysql_error.log
pid-file = /usr/server/mysql/mysql.pid
open_files_limit = 65535
back_log = 600
max_connections = 5000
max_connect_errors = 1000
table_open_cache = 1024
external-locking = FALSE
max_allowed_packet = 32M
sort_buffer_size = 1M
join_buffer_size = 1M
thread_cache_size = 600
#thread_concurrency = 8
query_cache_size = 128M
query_cache_limit = 2M
query_cache_min_res_unit = 2k
default-storage-engine = MyISAM
default-tmp-storage-engine=MYISAM
thread_stack = 192K
transaction_isolation = READ-COMMITTED
tmp_table_size = 128M
max_heap_table_size = 128M
log-slave-updates
log-bin = /usr/server/mysql/binlog/binlog
binlog-do-db=oa_fb
binlog-ignore-db=mysql
binlog_cache_size = 4M
binlog_format = MIXED
max_binlog_cache_size = 8M
max_binlog_size = 1G
relay-log-index = /usr/server/mysql/relaylog/relaylog
relay-log-info-file = /usr/server/mysql/relaylog/relaylog
relay-log = /usr/server/mysql/relaylog/relaylog
expire_logs_days = 10
key_buffer_size = 256M
read_buffer_size = 1M
read_rnd_buffer_size = 16M
bulk_insert_buffer_size = 64M
myisam_sort_buffer_size = 128M
myisam_max_sort_file_size = 10G
myisam_repair_threads = 1
myisam_recover
interactive_timeout = 120
wait_timeout = 120
skip-name-resolve
#master-connect-retry = 10
slave-skip-errors = 1032,1062,126,1114,1146,1048,1396
#master-host = 192.168.1.2
#master-user = username
#master-password = password
#master-port = 3306
server-id = 1
loose-innodb-trx=0
loose-innodb-locks=0
loose-innodb-lock-waits=0
loose-innodb-cmp=0
loose-innodb-cmp-per-index=0
loose-innodb-cmp-per-index-reset=0
loose-innodb-cmp-reset=0
loose-innodb-cmpmem=0
loose-innodb-cmpmem-reset=0
loose-innodb-buffer-page=0
loose-innodb-buffer-page-lru=0
loose-innodb-buffer-pool-stats=0
loose-innodb-metrics=0
loose-innodb-ft-default-stopword=0
loose-innodb-ft-inserted=0
loose-innodb-ft-deleted=0
loose-innodb-ft-being-deleted=0
loose-innodb-ft-config=0
loose-innodb-ft-index-cache=0
loose-innodb-ft-index-table=0
loose-innodb-sys-tables=0
loose-innodb-sys-tablestats=0
loose-innodb-sys-indexes=0
loose-innodb-sys-columns=0
loose-innodb-sys-fields=0
loose-innodb-sys-foreign=0
loose-innodb-sys-foreign-cols=0
slow_query_log_file=/usr/server/mysql/mysql_slow.log
long_query_time = 1
[mysqldump]
quick
max_allowed_packet = 32M
```
## 4、初始化数据库
``` shell
/usr/server/mysql/scripts/mysql_install_db --defaults-file=/etc/my.cnf  --user=mysql
```
## 5、创建开机启动脚本
``` shell
cd /usr/server/mysql/
cp support-files/mysql.server /etc/rc.d/init.d/mysqld
chkconfig --add mysqld
chkconfig --level 35 mysqld on
```
## 6、启动mysql服务器
``` shell
service mysqld start
```
## 7、连接 MySQL
``` shell
/usr/server/mysql/bin/mysql -u root -p
```
## 其他命令
``` shell
启动：service mysqld start
停止：service mysqld stop
重启：service mysqld restart
重载配置：service mysqld reload
```