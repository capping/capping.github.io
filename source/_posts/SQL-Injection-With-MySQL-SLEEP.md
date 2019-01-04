---
title: SQL Injection With MySQL SLEEP()
date: 2018-12-08 20:15:14
tags: [MySQL]
categories: [MySQL]
---

    最近，我们从一个客户端收到一个警告，说在一台服务器上运行的线程过高。登录之后，我们注意到所有`selects`都在等待表级读锁。我们滚动了进程列表，
找到了导致问题的`selects`。杀死了它，一切都恢复了正常。
    起初我们不明白为什么这个查询要花这么长时间，因为它看起来和其他一样。然后我们注意到其中一个WHERE字句很奇怪。在那里，我们发现查询附带了一个
SLEEP(3)。显然，该服务器是SQL注入攻击的受害者。

### What Is SQL Injection?
    我想我们大多数人都知道SQL注入是什么，但是作为一个复习，SQL注入是当有人向WHERE中提供恶意的输入，以运行自己的语句时。
    通常，当您请求用户输入时就会发生这种情况，例如用户名，但是他们没有给你一个真实的名字，而是给你一个MySQL语句，它会在你不知道的情况由你的服务器
运行
让我们看一些例子：
```
mysql> describe post;
+-------+------------------+------+-----+---------+----------------+
| Field | Type             | NULL | Key | Default | Extra          |
+-------+------------------+------+-----+---------+----------------+
| id    | int(10) unsigned | NO   | PRI | NULL    | auto_increment |
| test  | varchar(127)     | YES  |     | NULL    |                |
+-------+------------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)
mysql> select * from post;
+----+--------+
| id | test   |
+----+--------+s
|  1 | text1  |
|  2 | text2  |
|  3 | text3  |
|  4 | text4  |
|  5 | text5  |
|  6 | text6  |
|  7 | text7  |
|  8 | text8  |
|  9 | text9  |
| 10 | text10 |
+----+--------+
10 rows in set (0.00 sec)
```
