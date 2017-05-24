# redis

[TOC]

## redis 安装

** window版**

[下载地址](https://github.com/MSOpenTech/redis/releases)

### 配置文件

redis.windows.conf：

```
#是否作为守护进程运行
daemonize no
#Redis 默认监听端口
port 6379
#客户端闲置多少秒后，断开连接
timeout 300
#日志显示级别
loglevel verbose
#指定日志输出的文件名，也可指定到标准输出端口
logfile redis.log
#设置数据库的数量，默认最大是16,默认连接的数据库是0，可以通过select N 来连接不同的数据库
databases 32
#Dump持久化策略
#当有一条Keys 数据被改变是，900 秒刷新到disk 一次
#save 900 1
#当有10 条Keys 数据被改变时，300 秒刷新到disk 一次
save 300 100
#当有1w 条keys 数据被改变时，60 秒刷新到disk 一次
save 6000 10000
#当dump     .rdb 数据库的时候是否压缩数据对象
rdbcompression yes
#dump 持久化数据保存的文件名
dbfilename dump.rdb
###########    Replication #####################
#Redis的主从配置,配置slaveof则实例作为从服务器
#slaveof 192.168.0.105 6379
#主服务器连接密码
# masterauth <master-password>
############## 安全性 ###########
#设置连接密码
#requirepass <password>
############### LIMITS ##############
#最大客户端连接数
# maxclients 128
#最大内存使用率
# maxmemory <bytes>
########## APPEND ONLY MODE #########
#是否开启日志功能
appendonly no
# AOF持久化策略
#appendfsync always
#appendfsync everysec
#appendfsync no
################ VIRTUAL MEMORY ###########
#是否开启VM 功能
#vm-enabled no
# vm-enabled yes
#vm-swap-file logs/redis.swap
#vm-max-memory 0
#vm-page-size 32
#vm-pages 134217728
#vm-max-threads 4
```

### 主从复制

在从服务器配置文件中配置slaveof ,填写服务器IP及端口即可,如果主服务器设置了连接密码,在masterauth后指定密码就行了。

### 持久化

+ redis提供了两种持久化文案,Dump持久化和AOF日志文件持久化。
+ Dump持久化是把内存中的数据完整写入到数据文件,由配置策略触发写入,如果在数据更改后又未达到触发条件而发生故障会造成部分数据丢失。
+ AOF持久化是日志存储的,是增量的形式,记录每一个数据操作动作,数据恢复时就根据这些日志来生成。

## 命令行操作

使用CMD命令提示符,打开redis-cli连接redis服务器 ,也可以使用telnet客户端

`redis-cli -h 服务器 –p 端口 –a 密码`
`redis-cli.exe -h 127.0.0.1 -p 6379`

以下是一些服务器管理常用命令:

```
info   #查看服务器信息
select <dbsize> #选择数据库索引  select 1
flushall #清空全部数据
flushdb  #清空当前索引的数据库
slaveof <服务器> <端口>  #设置为从服务器
slaveof no one #设置为主服务器
shutdown  #关闭服务
```

### 批处理

service-install.bat

```
redis-server.exe --service-install redis.windows.conf --loglevel verbose 
```

uninstall-service.bat

```
redis-server --service-uninstall
```

startup.bat

```
redis-server.exe redis.windows.conf 
```