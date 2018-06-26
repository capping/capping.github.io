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
 
替换url中的特殊字符
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
