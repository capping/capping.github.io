---
title: url中的特殊字符
date: 2018-06-26 16:05:14
tags: [url]
categories: [Network]
---
uri: uniform resource identifier，统一资源标识符。 
url: uniform resource locator，统一资源定位符。 

本文中不讨论`url`和`uri`的区别, 暂全文使用url.

最近调用了一个api, 使用`get`请求资源. 遇到一个问题, 有的数据可以返回正确的结果, but not all!
这就让我很疑惑. 解决思路也很简单, 也没有花多少时间, 在此记录下解决思路, 最后做下总结.

解决思路:
拿到错误数据, 拼接好`url`, 拿到后台接受的数据.
```
# 问题url例子
https:www.example.com?a=123&b=abc#
```
其实看到这个url, 问题就明了了, 不过我还是输出下后台拿到的数据
```
Array
(
    [a] => 123
    [b] => abc
)
```
问题就出在url中带有特殊字符！

以下是url中的特殊字符以及对应的编码：

符号 | url中的含义 | 编码
----|---|---
+   | URL 中+号表示空格 | %2B
空格 | URL中的空格可以用+号或者编码  |  %20
/   | 分隔目录和子目录  |  %2F
?   | 分隔实际的URL和参数| %3F
%   | 指定特殊字符 | %25
#   | 表示书签   | %23
&   | URL中指定的参数间的分隔符 | %26
=   | URL中指定参数的值 | %3D

解决办法:
 
## 1. 替换url中的特殊字符
```
class StringUtil
{
    public static function UrlEncode($str)
    {
        $str = str_replace('%', '%25', $str);
        $str = str_replace('+', '%2B', $str);
        $str = str_replace(' ', '%20', $str);
        $str = str_replace('/', '%2F', $str);
        $str = str_replace('?', '%3F', $str);
        $str = str_replace('#', '%23', $str);
        $str = str_replace('&', '%26', $str);
        $str = str_replace('=', '%3D', $str);
        return $str;
    }
}
```
很明显, 需要先替换`%`, 因为编码中都存在%.

**最近(2019-01-18)又遇到一个问题，是由于php默认的url encode编码标准引起的**

## 2. http_build_query或urlencode

随着时间的增长，开始使用http_build_query或urlencode函数解决上面的问题，但是新的问题又出现啦。

先看常用的检验请求合法性的一个方式
```
function createToken($params) {
    $secretKey = 'secretKey';
    ksort($params);
    $query = http_build_query($params);
    $token = md5($query . $secretKey);
    return $token;
}
```
client 每个请求都携带一个 token ，token 是由请求参数和一个 secret key 一起md5之后计算出来的， 然后 server 端使用同样的算法计算token（一般还会校验time，给 token 一个有效期，我这里简化了），然后对比 token ，保证请求的合法性

但是，被接入方是Java开发的程序，当我的请求中带有`*`号时，验证一直无法通过。

![PHP和Java对`*`号进行urlencode](https://capping.github.io/images/java-php-encode.png)

这样，问题就很清晰了，不是算法问题，而是 url encode 的编码标准的问题。

那么怎么规避这个问题呢？

常见的无非就是这两种方案，第一，两边使用同样的编码标准，第二，生成 token 的算法改一下，不要使用 url encode 之后的字符串加密（建议）。
