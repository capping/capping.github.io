---
title: Java 日常总结
date: 2018-09-17 14:06:46
tags: [Java]
categories: [Java]
---
Java 日常总结，方便以后分类

1. AtomicLong
```
# AtomicLong 继承 Number, 实现 Serializable
public class AtomicLong extends Number implements java.io.Serializable
# 构造函数：
public AtomicLongs
public AtomicLongs(long initialValue)

# 使用
private final AtomicLong counter = new AtomicLong();
counter.incrementAndGet();  // 原子的将当前值增加1
```
