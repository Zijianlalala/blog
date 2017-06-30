---
title: Observer Pattern
date: 2017-05-04 22:08:34
tags: 设计模式
category: 设计模式
---
## 观察者模式
当对象间存在一对多关系时，则使用观察者模式（Observer Pattern）。比如，当一个对象被修改时，则会自动通知它的依赖对象。观察者模式属于行为型模式。

## 介绍
* 意图：定义对象间的一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。
* 主要解决：一个对象状态改变给其他对象通知的问题，而且要考虑到易用和低耦合，保证高度的协作。
* 何时使用：一个对象（目标对象）的状态发生改变，所有的依赖对象（观察者对象）都将得到通知，进行广播通知。
* 如何解决：使用面向对象技术，可以将这种依赖关系弱化。
* 关键代码：在抽象类里有一个 ArrayList 存放观察者们。
* 应用实例： 1、拍卖的时候，拍卖师观察最高标价，然后通知给其他竞价者竞价。 2、西游记里面悟空请求菩萨降服红孩儿，菩萨洒了一地水招来一个老乌龟，这个乌龟就是观察者，他观察菩萨洒水这个动作。
* 优点： 1、观察者和被观察者是抽象耦合的。 2、建立一套触发机制。
* 缺点： 1、如果一个被观察者对象有很多的直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间。 2、如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃。 3、观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。
* 使用场景： 1、有多个子类共有的方法，且逻辑相同。 2、重要的、复杂的方法，可以考虑作为模板方法。
* 注意事项： 1、JAVA 中已经有了对观察者模式的支持类。 2、避免循环引用。 3、如果顺序执行，某一观察者错误会导致系统卡壳，一般采用异步方式。

## 基本代码
* UML图
![观察者模式](Observer.png)
***

## 实现
* 直接放代码，我偷懒了
```java

// 抽象主题角色,该角色调用Shout方法的时候,所有的观察者调用其update
public abstract class Animal extends Observable {
    public abstract void Shout();
}

// 真实主题角色
public class Cat extends Animal {
    private String name;


    public Cat(String name) {
        super();
        this.name = name;
    }


    public String getName() {
        return name;
    }

    @Override
        public void Shout(){
            System.out.println("喵喵喵");
            setChanged();
            notifyObservers(this);
        }
}

// 同上
public class Dog extends Animal {
    private String name;

    public Dog(String name) {
        super();
        this.name = name;
    }

    public String getName() {
        return name;
    }

    @Override
        public void Shout() {
            System.out.println("汪汪旺");
            setChanged();
            notifyObservers(this);
        }
}

// 观察者角色，与上面类似，可以有抽象观察者和具体观察者角色
// 这里直接实现一个观察者，让这个观察者观察两个状态
// 猫叫和狗叫都触发观察者的update,但是效果不同
public class Jerry extends Animal implements Observer {
    private String name;

    public Jerry(String name) {
        super();
        this.name = name;
    }

    public String getName(){
        return name;
    }

    // 观察者必须实现这个方法,该方法在主题发生改变时调用
    @Override
    public void update(Observable o, Object arg) {
        if (o instanceof Cat) {
            Cat c = (Cat) o;
            System.out.println(c.getName() + "来了,快跑啊");
        }
        if (o instanceof Dog) {
            Dog c = (Dog) o;
            System.out.println(c.getName() + "来了,不用跑");
        }
    }

    @Override
    public void Shout() {
        System.out.println("此处应有老鼠的叫声");
    }
}

// 测试
public class Test {
    public static void main(String[] args) {
        Cat c = new Cat("Tom");
        Dog d = new Dog("Jack");
        Jerry j = new Jerry("Jerry");
        c.addObserver(j);
        d.addObserver(j);
        c.Shout();
        d.Shout();	
    }
}
```

## 其他
* 观察者角色主要是为了实现，一个类改变，其他类收到通知，进行相应的改变
* java实现的Observer接口和Observable类，就是根据观察者模式实现的，Observer接口是我见过最水的一个接口
* C#中的委托和事件，相对java耦合行更低
* Head First中家庭影院的例子，个人认为用模板方法模式更合适
