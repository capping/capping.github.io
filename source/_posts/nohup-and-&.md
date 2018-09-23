---
title: nohup and &
date: 2018-08-30 11:11:08
tags: [Linux]
categories: [Linux]
---

今天来解决下一直存在的一个疑问 `nohup` 和 `&` 到底是什么？

在日常中使用往往是这样子的
```
nohup php demo.php &
```

但是为什么要两个一起用呢？ 只用一个行不行？ 只用其中的一个是什么效果呢？

我们先看最后一个问题
1. 只用 `&`  
&的意思是在后台运行, 就是当你运行代码 `php demo.php &` 时, 即使你使用 `Ctrl C` ,代码照样会运行(因为对SIGINT信号免疫)。但是要注意, 当你关掉shell后, `php demo.php`进程就消失了。可见 `&` 的后台并不硬(因为对SIGHUP信号不免疫)

2. 只用 `nohup`
nohup的意思是忽略SIGHUP信号, 所以当运行 `nohup demo.php` 的时候, 关闭shell `php demo.php` 进程还是存在(对SIGHUP信号免疫)。 但是直接在shell中使用 `Ctrl C`, `php demo.php`进程就会消失(因为对SIGINT信号不免疫)。

所以, &和nohup没有半毛钱的关系, 要让进程真正不受shell中Ctrl C和shell关闭的影响, 那该怎么办呢？ 那就用 `nohup php demo.php &` 吧, 两全其美。

解决了最后一个问题, 其他问题就迎刃而解啦

如果你懂守护进程, 那么 `nohup php demo.php &` 颇有点让 `php demo.php` 成为守护进程的感觉。
