---
title: Decorator Pattern
date: 2017-05-03 20:45:30
tags: 设计模式
---

## 装饰器模式
* 装饰器模式（Decorator Pattern）允许向一个现有的对象添加新的功能，同时又不改变其结构。这种类型的设计模式属于结构型模式，它是作为现有的类的一个包装。
这种模式创建了一个装饰类，用来包装原有的类，并在保持类方法签名完整性的前提下，提供了额外的功能。
我们通过下面的实例来演示装饰器模式的用法。其中，我们将把一个形状装饰上不同的颜色，同时又不改变形状类。

## 介绍
* 意图：动态地给一个对象添加一些额外的职责。就增加功能来说，装饰器模式相比生成子类更为灵活。
* 主要解决：一般的，我们为了扩展一个类经常使用继承方式实现，由于继承为类引入静态特征，并且随着扩展功能的增多，子类会很膨胀。
* 何时使用：在不想增加很多子类的情况下扩展类。
* 如何解决：将具体功能职责划分，同时继承装饰者模式。
* 关键代码： 1、Component 类充当抽象角色，不应该具体实现。 2、修饰类引用和继承 Component 类，具体扩展类重写父类方法。
* 应用实例： 1、孙悟空有 72 变，当他变成"庙宇"后，他的根本还是一只猴子，但是他又有了庙宇的功能。 2、不论一幅画有没有画框都可以挂在墙上，但是通常都是有画框的，并且实际上是画框被挂在墙上。在挂在墙上之前，画可以被蒙上玻璃，装到框子里；这时画、玻璃和画框形成了一个物体。
* 优点：装饰类和被装饰类可以独立发展，不会相互耦合，装饰模式是继承的一个替代模式，装饰模式可以动态扩展一个实现类的功能。
* 缺点：多层装饰比较复杂。
* 使用场景： 1、扩展一个类的功能。 2、动态增加功能，动态撤销。
* 注意事项：可代替继承。

## 实现
* ```java
// Salary.java
public abstract class Salary {
    public double getReward(){return 0;}
}

// Person.java
public class Person extends Salary {
    private String name;
    private double baseSalary = 1;

    Person(String name, double baseSalary) {
        this.name = name;
        this.baseSalary = baseSalary;
    }

    public String getName() {
        return name;
    }

    public double getReward() {
        return baseSalary;
    }
}

// Reward.java
public class Reward extends Salary {
    public Salary salary = null;

    public Reward(Salary salary) {
        this.salary = salary;
    }

    public double getSalary() {
        return salary.getReward();// 这个改成具体方法
    }
}

// Reward_add100.java
public class Reward_add100 extends Reward{
    public Reward_add100(Salary salary){
        super(salary);
    }

    public double getReward(){
        return 100+super.getSalary();
    }
}

// Reward_mul2.java
package reward;

public class Reward_mul2 extends Reward{
    public Reward_mul2(Salary salary){
        super(salary);
    }

    public double getReward(){
        return 2*super.getSalary();
    }
}

// Test.java
public class Test {

    public static void main(String[] args) {
        Person person = new Person("小菜",100);

        double s1=new Reward_mul2(new Reward_add100(person)).getReward();
        double s2=new Reward_add100(new Reward_mul2(person)).getReward();

        System.out.print("(100+100)*2 = "+s1+"\n");
        System.out.print("100*2+100 = "+s2+"\n");
    }

}
```

## 个人理解
* 装饰者模式是个结构型模式,这些设计模式关注类和对象的组合。继承的概念被用来组合接口和定义组合对象获得新功能的方式。装饰者模式,继承A类,同时,属性包含一个A类的对象,一般的,构造函数传入一个A类的对象,并把这个对象赋值给自身的属性,然后对这个属性进行操作.
* 这个模式的优势是,需要某个功能,可以单独的开发这个功能,且只需要考虑本体的接口,如果不需要某个功能了,只需要在组合的时候去掉.但是,如果用继承实现,则不容易修改中间的某个类,装饰者比继承更灵活



