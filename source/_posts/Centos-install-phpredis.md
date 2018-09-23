---
title: Centos安装 phpredis 扩展
date: 2018-08-20 13:49:14
tags: [PHP, redis, centos, Linux]
categories: [PHP]
---

## 安装配置
```
# 1. 下载phpredis
wget https://github.com/phpredis/phpredis/archive/4.1.1.tar.gz

# 2. 解压 
# z-tar包被gzip压缩,用gunzip解压 x-从tar包中提取文件 v-显示详细信息 f-指定被处理的文件
tar -zxvf 4.1.1.tar.gz

# 3. 进入解压后目录
cd phpredis-4.1.1/

# 4. 生成configure配置文件
/usr/local/php/bin/phpize

# 5. 配置
./configure --with-php-config=/usr/local/php/bin/php-config

# 6. 编译
make

# 7. 运行测试
make test

# 8. 安装
make install

# 9. 配置ini文件
vi /usr/local/php/lib/php.ini
> extension="redis.so"

# 10. 重启php-fpm
kill PID
/usr/local/php/sbin/php-fpm

# 11. 重启nginx
nginx -s reload # 重启nginx
```

## 遇到的问题

重启php-fpm时使用

> /usr/local/php/sbin/php-fpm -c /usr/local/php/etc/php-fpm.conf

导致 `phpinfo()` 中参数 `Loaded Configuration File` 是 `/usr/local/php/etc/php-fpm.conf` ,所以打印 `phpinfo` 时一直无法显示`redis`扩展。

正确的做法是上面显示的，不指定配置文件 ~~-c /usr/local/php/etc/php-fpm.conf~~ 的做法
