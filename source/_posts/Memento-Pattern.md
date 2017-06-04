---
title: Memento Pattern
date: 2017-06-01 12:53:29
tags: 设计模式
---
## 备忘录模式
备忘录模式（Memento Pattern）保存一个对象的某个状态，以便在适当的时候恢复对象。备忘录模式属于行为型模式。

## 介绍
**意图**：在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。
**主要解决**：所谓备忘录模式就是在不破坏封装的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态，这样可以在以后将对象恢复到原先保存的状态。
**何时使用**：很多时候我们总是需要记录一个对象的内部状态，这样做的目的就是为了允许用户取消不确定或者错误的操作，能够恢复到他原先的状态，使得他有"后悔药"可吃。
**如何解决**：通过一个备忘录类专门存储对象状态。
**关键代码**：客户不与备忘录类耦合，与备忘录管理类耦合。
**应用实例**： 1、后悔药。 2、打游戏时的存档。 3、Windows 里的 ctri + z。 4、IE 中的后退。 4、数据库的事务管理。
**优点**： 1、给用户提供了一种可以恢复状态的机制，可以使用户能够比较方便地回到某个历史的状态。 2、实现了信息的封装，使得用户不需要关心状态的保存细节。
**缺点**：消耗资源。如果类的成员变量过多，势必会占用比较大的资源，而且每一次保存都会消耗一定的内存。
**使用场景**： 1、需要保存/恢复数据的相关状态场景。 2、提供一个可回滚的操作。
**注意事项**： 1、为了符合迪米特原则，还要增加一个管理备忘录的类。 2、为了节约内存，可使用原型模式+备忘录模式。


## 基本代码
* UML图
![备忘录模式](Memento.png)
***

* 主类测试
```java
package memento;

public class Main {
    /**
     * 备忘录对客户端透明,客户端只需要创建发起人和管理者的对象
     * 如果备忘录修改所记录的内容
     * 客户端不需要作出改变
     * 
     * 发起人和备忘录之间是紧耦合的
     * 如果备忘录修改所记录的内容
     * 则需要修改备忘录和发起人
     * 不符合开放封闭原则
     * */

    public static void main(String[] args) {
        // 创建发起人对象,设置其状态并显示
        Originator o = new Originator();
        o.setState("On");
        o.show();

        // 创建管理者对象,保存当前发起人状态
        Caretaker c = new Caretaker();
        c.setMenento(o.createMemento());

        // 改变发起人状态,并显示
        o.setState("Off");
        o.show();

        // 通过管理者,恢复发起人状态
        o.setMemento(c.getMenento());
        o.show();
    }
}
```

* 发起人
```java
package memento;

public class Originator {
    /**
     * 发起人(Originator)
     * state为属性,此处为了说明原理,只写一个属性,一般为多个
     * get/set方法修改该属性
     * crateMemento返回当前state
     * setMemento(Memento memento)通过被保存的信息给state赋值
     * */
    private String state;

    public String getState() {
        return state;
    }

    public void setState(String state) {
        this.state = state;
    }

    // 创建备忘录,在备忘录中保存需要保存的信息(并非所有属性)
    public Memento createMemento() {
        return new Memento(state);
    }

    // 依次获取备忘录中的信息并赋值
    public void setMemento(Memento memento) {
        this.state = memento.getState();
    }

    public void show() {
        System.out.println("State=" + this.state);
    }
}
```

* 备忘录
```java
package memento;

public class Memento {
    /**
     * 备忘录(Memento)
     * 只保存Originator给出的需要保存的信息
     * */
    private String state;

    // 通过一个构造函数获得状态的信息
    public Memento(String state) {
        this.state = state;
    }

    // 通过不同的get方法得到保存的所有信息
    // 此处用一个仅用get说明模式的用法
    public String getState() {
        return state;
    }

}
```

* 管理者
```java
package memento;

public class Caretaker {
    /**
     * 管理者类Caretaker
     * 维护一个备忘录属性
     * 仅有get和set方法
     * */
    private Memento menento;

    public Memento getMenento() {
        return menento;
    }

    public void setMenento(Memento menento) {
        this.menento = menento;
    }
}
```
