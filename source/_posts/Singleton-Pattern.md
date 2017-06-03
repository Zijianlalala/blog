---
title: Singleton Pattern
date: 2017-06-01 12:51:02
tags: 设计模式
---

## 基本代码
* UML图
![单例模式](Singleton.png)

* 懒汉式
```java
package singleton;

public class Singleton {
	private static Singleton instance;

	private Singleton() {
	}

	/**
	 * (浪费)时间换(节省)空间 懒汉式
	 */
	public static Singleton getInstance() {
		if (instance == null) {
			instance = new Singleton();
		}
		return instance;
	}

	public static void main(String[] args) {
		try {
			// 使用反射创建对象
			Singleton t1 = Singleton.class.newInstance();
			Singleton t2 = Singleton.class.newInstance();
			Singleton t3 = Singleton.getInstance();
			Singleton t4 = Singleton.getInstance();
			
			System.out.println(t1); // singleton.Singleton@6bc7c054
			System.out.println(t2); // singleton.Singleton@232204a1
			System.out.println(t3); // singleton.Singleton@4aa298b7
			System.out.println(t4); // singleton.Singleton@4aa298b7
		} catch (Exception e) {
			System.out.println(e);
		}
	}
}
```

* 饿汉式
```java
package singleton;

public class Singleton2 {
	/**
	 * (浪费)空间换(节省)时间 饿汉式
	 */
	private static Singleton2 instance = new Singleton2();

	private Singleton2() {
	}

	public static Singleton2 getInstance() {
		return instance;
	}
}
```
* 懒汉式代码可以看出,在**单例类中**通过**反射**创建对象,创建的**并不是同一个对象**,在单例类外不能通过反射创建对象.
***
