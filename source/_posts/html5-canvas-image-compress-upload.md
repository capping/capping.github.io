---
title: 前端压缩图片, php实现上传
date: 2018-06-02 10:17:24
tags: [canvas, js, PHP]
categories: [PHP]
---
今天在做一个图片上传的功能。
流程如下: 点击添加图片按钮-->拍照(或者选择相册图片)-->然后会调用php的上传接口实现上传。
遇到的问题: 图片的尺寸过大, 上传时间过长。
解决思路: 前端压缩图片后上传。

无奈前端小哥, 工作量过大, 想让后端实现压缩后上传。经过调研, 妈呀！这不科学(后端的压缩都是图片上传成功后做的, 对提高上传速度没有卵用)。

继续探索, 找前端实现压缩的方法, 大都是使用base64做, 这...

还好我找到了张鑫旭大神的文章(http://www.zhangxinxu.com/wordpress/2017/07/html5-canvas-image-compress-upload/)

前端的实现文章里讲的很详细, 此处不再赘述。

随之而来的问题, 由于此处使用的`toBlob`方法方法实现的, 那做为一个php后端要怎么接收`blob`(binary large object)对象呢？

普通的文件上传php使用`$_FILES`可以接收, 然而我`var_dump()`打印出来的却是一个空数组。 那试试`$_REQUEST`?

然后我借助搜索引擎搜索`前端压缩图片后端php怎么接受?`, 只能说, 我很绝望！ 继续啃大神的文章, 如下一句话, 点醒梦中人:
`可以把canvas转换成Blob文件，通常用在文件上传中，因为是二进制的，对后端更加友好`。

然后我就找到了查询的关键字`php如何接收二进制流？`
豁然开朗:
```
# 方式一
$content = $GLOBALS['HTTP_RAW_POST_DATA'];  // 需要php.ini设置
# 方式二
$content = file_get_contents('php://input');    // 不需要php.ini设置，内存压力小
```
作为一个技术不那么严谨的公司(连服务器的mb_string扩展都没有正确开启), 果断使用方法二。

那拿到前端传过来的blob对象,怎么把blob解析成图片呢？
接下里就顺畅多了, 直接搜索`php如何接收二进制流转换成图片？`
```
$file_name = date("YmdHis").rand("1000","9999").".png";
//图片路径
$file_dir = UPLOADIMGNEW.$file_name;
$img = file_get_contents("php://input");
if (empty($img)) {
    thow new Exception('二进制流为空', 400); //二进制流为空
}
if ($fp = fopen($file_dir, 'w')) {
    if (fwrite($fp, $img)) {
        fclose($fp); //成功
        return json_encode[
            'url' => "http://" . $_SERVER["SERVER_NAME"] . ":" . $_SERVER["SERVER_PORT"].dirname($_SERVER["SCRIPT_NAME"])."/upload/img/".$file_name
        ];
    } else {
        thow new Exception('写入文件不成功', 500); //写入文件不成功
    }
} else {
    thow new Exception('不能打开或创建文件', 500); //不能打开或创建文件
}
```

ok！ 打完收工...
