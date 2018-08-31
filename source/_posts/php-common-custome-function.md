---
title: 常用的PHP工具类
date: 2018-08-31 16:46:14
tags: [php]
categories: [php]
---

# StringUtil

1. 生成随机字符串
```
function randomString($length = 6) :string
{
    $pattern = '1234567890abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
    $str = '';
    for ($i = 0; $i < $length; $i++) {
        $str .= $pattern{mt_rand(0, 61)};
    }

    return $str;
}
```
