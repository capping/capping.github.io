---
title: JavaSE
date: 2018-08-08 09:57:06
tags:
---

## Getting Started

An introduction to Java technology and lessions on installing Java development software and using it to create a simple program.

### The Java Technology Phenomenon

Java technology is both a programming language and a platform. (java即使编程语言也是平台)

#### The java Programming Language

The Java programming language is a high-level language that can be characterized by all of the following buzzword.
 - Simple  
 - Architecture neutral (平台无关的)
 - Object oriented
 - Portable (可移植的)
 - Distributed (分布式的)
 - High performance
 - Multithreaded
 - Roubust (健壮的)
 - Dynamic
 - Secure

> MyProgram.java -- javac --> MyProgram.class -- java --> MyProgram

Through the Java VM, the same application is capable of running on multiple platforms.

#### The java platform

A platform is the hardware or software environment in which a program runs.  
The Java platform has two components:
- The Java Virtual Machine.
- The Java Application Programming Interface(API).
JVM 是java平台的基础，并被移植到各种给予硬件的平台上。
The API is a large collection of ready-made(现成) software componment that provide many useful capabilities(功能). It is grouped into libraries of related classes and interfaces; these libraries are known as packages.

![](https://github.com/capping/blog/blob/master/source/images/getStarted-jvm.gif?raw=true)
The API and Java Virtual Machine insulate the program from the underlying hardware.
