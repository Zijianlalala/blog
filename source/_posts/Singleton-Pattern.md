---
title: Singleton Pattern
date: 2017-06-01 12:51:02
tags: 设计模式
---
## 单例模式
单例模式（Singleton Pattern）是 Java 中最简单的设计模式之一。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。
这种模式涉及到一个单一的类，该类负责创建自己的对象，同时确保只有单个对象被创建。这个类提供了一种访问其唯一的对象的方式，可以直接访问，不需要实例化该类的对象。
### 注意：
0. 单例类只能有一个实例。
0. 单例类必须自己创建自己的唯一实例。
0. 单例类必须给所有其他对象提供这一实例。

## 介绍
* 意图：保证一个类仅有一个实例，并提供一个访问它的全局访问点。
* 主要解决：一个全局使用的类频繁地创建与销毁。
* 何时使用：当您想控制实例数目，节省系统资源的时候。
* 如何解决：判断系统是否已经有这个单例，如果有则返回，如果没有则创建。
* 关键代码：构造函数是私有的。
* 应用实例： 1、一个党只能有一个主席。 2、Windows 是多进程多线程的，在操作一个文件的时候，就不可避免地出现多个进程或线程同时操作一个文件的现象，所以所有文件的处理必须通过唯一的实例来进行。 3、一些设备管理器常常设计为单例模式，比如一个电脑有两台打印机，在输出的时候就要处理不能两台打印机打印同一个文件。
* 优点： 1、在内存里只有一个实例，减少了内存的开销，尤其是频繁的创建和销毁实例（比如管理学院首页页面缓存）。 2、避免对资源的多重占用（比如写文件操作）。
* 缺点：没有接口，不能继承，与单一职责原则冲突，一个类应该只关心内部逻辑，而不关心外面怎么样来实例化。
* 使用场景： 1、要求生产唯一序列号。 2、WEB 中的计数器，不用每次刷新都在数据库里加一次，用单例先缓存起来。 3、创建的一个对象需要消耗的资源过多，比如 I/O 与数据库的连接等。
* 注意事项：getInstance() 方法中需要使用同步锁 synchronized (Singleton.class) 防止多线程同时进入造成 instance 被多次实例化。

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
