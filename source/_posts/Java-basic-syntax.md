---
title: Java基础语法
date: 2018-09-23 10:58:42
tags: [Java]
categories: [Java]
---
1. final关键字
类，方法和成员变量能被定义为`final`.  
- 如果一个类被声明为final, 则不能被继承.  
- `final`标记的方法不能被子类复写  
```
1. 把方法锁住, 防止任何继承类修改它的意义和实现
2. 高效. 编译器在遇到调用final方法时会转入内嵌机制, 大大提高执行效率.
```
- `final`标记的变量即成为常量,只能被赋值一次
```
# 单例模式
// 懒汉式
class Singleton
{
	private static Singleton singleton = new Singleton();

	private Singleton() {
	}

	public static Singleton getInstance() {
		return singleton;
	}

	public static void main(String[] args) {
		System.out.println(Singleton.getInstance());
	}
}

// 饿汉式
class Singleton
{
	private static Singleton singleton;

	private Singleton() {
	}

	public static Singleton getInstance() {
		if (null == singleton) {
			synchronized(Object.class) {
				if (null == singleton) {
					singleton = new Singleton();
				}
			}
		}
		return singleton;
	}
}
```

2. 静态代码块: 用static声明, jvm加载类时执行, 仅执行一次.
构造代码块: 类中直接使用{}定义, 每一次构建对象时执行.
执行顺序: 静态块, main(), 构造块, 构造方法.

