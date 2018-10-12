---
title: PHP基础语法
date: 2018-09-23 10:20:49
tags: [PHP]
categories: [PHP]
---
1. PHP final关键字
类和方法能被定义为`final`.  
- 如果父类中的方法被声明为`final`, 则子类无法覆盖该方法.   
- 如果一个类被声明为final, 则不能被继承.
```
# 单例模式
<?php
final class Singleton
{
	private static $instance;

	public static function getInstance(): Singleton
	{
		if (null === static::$instance) {
			static::$instance = new static();
		}

		return static::$instance;
	}

	/**
	 * 不允许从外部调用以防止创建多个实例
	 * 要使用单例，必须通过 Singleton::getInstance() 方法获取实例
	 */
	private function __construct() 
	{
	}

	/**
	 * 防止实例被克隆(这会创建实例的副本)
	 * @return [type] [description]
	 */
	private function __clone()
	{
	}

	/**
	 * 防止反序列化(这将创建他的副本)
	 */
	private function __wakeup()
	{
	}
}
```