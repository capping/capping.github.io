---
title: sourceTree安装时免登录
date: 2018-05-16 17:01:55
tags: [git]
categories: [tools]
---

sourceTree是一个工具, 我一直用它做git管理(git的图形化界面GUI)
最大的一个问题是 : 安装 SourceTree 时, 需要使用atlassian授权, 即使翻墙这个过程也会出现反应慢, 收不到邮件或短信的问题, 现提供跳过 atlassian账号 授权方法。

![](https://github.com/capping/blog/blob/master/source/images/1066204-20171122140702633-1169598542.png?raw=true)

安装之后, 转到用户本地文件夹下的 SourceTree 目录, 没有则新建：

> %LocalAppData%\Atlassian\SourceTree\

请把以上路径直接粘贴到我的电脑路径的位置跳转, 才能跳转到正确位置

新建 accounts.json 文件

> %LocalAppData%\Atlassian\SourceTree\accounts.json

输入以下内容保存即可
```
[
  {
    "$id": "1",
    "$type": "SourceTree.Api.Host.Identity.Model.IdentityAccount, SourceTree.Api.Host.Identity",
    "Authenticate": true,
    "HostInstance": {
      "$id": "2",
      "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountInstance, SourceTree.Host.AtlassianAccount",
      "Host": {
        "$id": "3",
        "$type": "SourceTree.Host.Atlassianaccount.AtlassianAccountHost, SourceTree.Host.AtlassianAccount",
        "Id": "atlassian account"
      },
      "BaseUrl": "https://id.atlassian.com/"
    },
    "Credentials": {
      "$id": "4",
      "$type": "SourceTree.Model.BasicAuthCredentials, SourceTree.Api.Account",
      "Username": "",
      "Email": null
    },
    "IsDefault": false
  }
]
```
![](https://github.com/capping/blog/blob/master/source/images/1526462870.png?raw=true)

原博客链接：https://blog.csdn.net/liby_sunny/article/details/78813001  
若影响到个人利益, 请联系博主删除该文章。
