---
title: Centos安装配置redis
date: 2018-08-20 10:27:14
tags: [redis, centos]
categories: [redis]
toc: true
---
## 安装
```
cd /usr/local/src/
# 下载
wget http://download.redis.io/releases/redis-4.0.11.tar.gz

# 解压
tar xzf redis-4.0.11.tar.gz
mv redis-4.0.11 /usr/local/redis
cd /usr/local/redis
make
make test
```
有的机器会出现类似以下错误：
```
make[1]: Entering directory `/root/redis/src'
You need tcl 8.5 or newer in order to run the Redis test
……
```
这是因为没有安装tcl导致，yum安装即可：
```
yum install tcl
make test
make install
```
## 配置
```
bind: 在bind 127.0.0.1前加H#"将其注释掉, 使外网可以连接
默认为保护模式, 把 protected-mode yes 改为 protected-mode no
默认为不守护进程模式, 把daemonize no 改为daemonize yes 使Redis进程在后台运行
将 requirepass foobared前的"#"去掉, 密码改为你想要设置的密码
daemonize : 是否以后台daemon方式运行
pidfile : pid文件位置
port : 监听的端口号
timeout : 请求超时时间
loglevel : log信息级别
logfile : log文件位置
databases : 开启数据库的数量
save * * : 保存快照的频率, 第一个*表示多长时间, 第三个*表示执行多少次写操作。在一定时间内执行一定数量的写操作时, 自动保存快照。可设置多个条件。
rdbcompression : 是否使用压缩
dbfilename : 数据快照文件名（只是文件名）
dir : 数据快照的保存目录（仅目录）
appendonly : 是否开启appendonlylog, 开启的话每次写操作会记一条log, 这会提高数据抗风险能力, 但影响效率。
appendfsync : appendonlylog如何同步到磁盘。三个选项, 分别是每次写都强制调用fsync、每秒启用一次fsync、不调用fsync等待系统自己同步
```
redis 配置文件示例
> https://github.com/linli8/cnblogs/blob/master/redis%E5%89%AF%E6%9C%AC.conf

启动 : redis-server ./redis.conf
停止 : redis-cli -h 127.0.0.1 -p 6379 shutdown

开启自启动: echo "/usr/local/bin/redis-server /usr/local/redis/redis.conf" >>/etc/rc.local
