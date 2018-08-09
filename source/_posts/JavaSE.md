---
title: JavaSE
date: 2018-08-08 09:57:06
tags: [java]
categories: [java]
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

#### What Can Java Technology Do?

- Development Tools: The development tools provide everything you'll need for compiling, running, monitoring, debugging, and documenting your application. As a new developer, the main tools you'll be using are the javac compiler, the java launcher and the javadoc documentation tool.
- Application Programming Interface(API): The API provides the core functionlity of the java programming language. 
- Deployment Technologies
- User Interface Tookit
- Integration Libraries: Integration libraries such as the Java IDL API, JDBC API, Java Naming and Directory Interface(JNDA) API, and Java Remote Method Invocation over Internet Inter-ORB Protocol Technology (Java RMI-IIOP Technology) enable database access and manipulation of remote objects.

#### How Will Java Technology Change My Life?

## Learning the Java language

Lessons describing the essential concepts and features of the Java Programming Language.

This trail provides everything you'll need to know about getting started with the Java programming language.

### Object-Oriented Programming Concepts

teaches you the core concepts behind object-oriented programming: objects, messages, classes and inheritance. The lesson ends by showing you how these concepts translate into code.  

#### What Is an Object?

Software objects are conceptually similar to real-world object: they too consist of state and related behavior. An object stores it state in fields(variables in some 
programming languages) and exposes its behavior through methods (functions in some programming languages).Methods operate on an object's internal state and serve as 
the primary mechanism for object-to-object communication. Hiding internal state and requiring all interaction to be performed through an object's methods is know as 
`data encapsulation`（数据封装） -- a fundamental principel of object-oriented programming.

**Bundling code into individual software objects provides a number of benefit, include:**
1. Modularity: The source code for an object can be written and maintained independently of the source code for other objects. Once created, an object can be easily passed around inside the system.
2. Information-hiding: By interacting only with an object's methods, the datails of its internal implementation remain hidden from the outside world.
3. Code re-use: If an object already exists (perhaps written by another software developer), you can use that object in your program. This allows specialists to implement/test/debug complex, task-specific objects, which you can then trust to run in your own code.
4. Pluggability and debugging easy: If a particular object turns out to be problematic, you can simple remove it from your application and plug in a different object as its replacement.This is analogous to fixing mechanical problems in ther real world. If a bolt breaks, you replace it, not the entire machine.

#### What Is a Class?

A class is the blueprint from which individual objects are created.

#### What Is Inheritance?

Object-Oriented programming allows classes to inherit commonly used state and behavior from other classes.In the Java programming language, each class is allowed to have one direct superclass, and each superclass has the potential for an unlimited number of subclass.(java 是单继承的)

#### What Is an Interface?

Interface form a contract between the class and the outside world, and this contract is enforced at build time by the compiler. If your class claims to implement an interface， all methods defined by that interface must appear in its source code before the class will successfully compile.

Note: To actually compile the ACMEBicycle class, you'll need to add the `public` keyword to the beginning of the implemented interface methods.

#### What Is a Package?

A package is a namespace that organizes a set of related classes and interface.(因为java开发的软件由成千上万的类文件组成，将关联类和接口放在同一特定意义的包中)

