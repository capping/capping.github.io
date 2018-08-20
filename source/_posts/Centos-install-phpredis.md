---
title: Centos_install_phpredis
date: 2018-08-20 13:49:14
tags: [php, redis]
categories: [redis]
---
暂记录，有时间完善
```
wget https://github.com/phpredis/phpredis/archive/4.1.1.tar.gz
tar -zxvf 4.1.1.tar.gz
cd phpredis-4.1.1/
/usr/local/php/bin/phpize # 生成configure配置文件
./configure --with-php-config=/usr/local/php/bin/php-config
make
make install
vi /usr/local/php/lib/php.ini
> extension="redis.so"
重启php-fpm
nginx -s reload # 重启nginx
```