---
title: Simple Factory Pattern
date: 2017-05-01 16:23:14
tags: 设计模式
category: 设计模式
---
## 简单工厂模式
* 简单工厂模式不再被单独看成一种设计模式,简单了解后,着重看 **反射**

### 实现步骤
1. 创建一个接口(抽象产品角色)
2. 创建多个实现接口的实体类(具体产品角色)
3. 创建一个工厂，生成基于给定信息的实体类的对象(工厂角色)

### 角色介绍
* 工厂角色（Creator）：这是简单工厂模式的核心，由它负责创建所有的类的内部逻辑。当然工厂类必须能够被外界调用，创建所需要的产品对象。
* 抽象（Product）产品角色：简单工厂模式所创建的所有对象的父类，注意，这里的父类可以是接口也可以是抽象类，它负责描述所有实例所共有的公共接口。
* 具体产品（Concrete Product）角色：简单工厂所创建的具体实例对象，这些具体的产品往往都拥有共同的父类。

## 控制反转
控制反转（Inversion of Control，英文缩写为IoC）是一个重要的面向对象编程的法则来削减计算机程序的耦合问题。 控制反转一般分为两种类型，依赖注入（Dependency Injection，简称DI）和依赖查找（Dependency Lookup）。依赖注入应用比较广泛。

## 依赖注入
可以把IoC模式看做是工厂模式的升华，可以把IoC看作是一个大工厂，只不过这个大工厂里要生成的对象都是在XML文件中给出定义的，然后利用Java 的“反射”编程，根据XML中给出的类名生成相应的对象。

关于什么是依赖注入，在Stack Overflow上面有一个问题，如何向一个5岁的小孩解释依赖注入，其中得分最高的一个答案是：

> When you go and get things out of the refrigerator for yourself, you can cause problems. You might leave the door open, you might get something Mommy or Daddy doesn’t want you to have. You might even be looking for something we don’t even have or which has expired. What you should be doing is stating a need, “I need something to drink with lunch,” and then we will make sure you have something when you sit down to eat.

## 反射
Java反射机制主要提供了以下功能： 在运行时判断任意一个对象所属的类；在**运行时构造任意一个类的对象**；在运行时判断任意一个类所具有的成员变量和方法；在运行时调用任意一个对象的方法；生成动态代理。
`Class.forName("String")` 通过字符串传入类名的具体位置可以创建该类的对象,解决工厂模式中,增加产品类就需要修改工厂类的问题.可以通过配置文件解决具体类名过长的问题

* [参考代码](http://www.cnblogs.com/lzq198754/p/5780331.html)
```java
interface fruit {
    public abstract void eat();
}
class Apple implements fruit {
    public void eat() {
        System.out.println("Apple");
    }
}
class Orange implements fruit {
    public void eat() {
        System.out.println("Orange");
    }
}
class Factory {
    public static fruit getInstance(String ClassName) {
        fruit f = null;
        try {
            f = (fruit) Class.forName(ClassName).newInstance();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return f;
    }
}
```
## 综上
* 依赖注入是控制反转中的一种
* 反射是实现依赖注入的手段

