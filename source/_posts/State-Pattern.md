---
title: State Pattern
date: 2017-05-19 14:05:29
tags: 设计模式
---

## 状态模式
在状态模式（State Pattern）中，类的行为是基于它的状态改变的。这种类型的设计模式属于行为型模式。
在状态模式中，我们创建表示各种状态的对象和一个行为随着状态对象改变而改变的 context 对象。

## 介绍
* 意图：允许对象在内部状态发生改变时改变它的行为，对象看起来好像修改了它的类。
* 主要解决：对象的行为依赖于它的状态（属性），并且可以根据它的状态改变而改变它的相关行为。
* 何时使用：代码中包含大量与对象状态有关的条件语句。
* 如何解决：将各种具体的状态类抽象出来。
* 关键代码：通常命令模式的接口中只有一个方法。而状态模式的接口中有一个或者多个方法。而且，状态模式的实现类的方法，一般返回值，或者是改变实例变量的值。也就是说，状态模式一般和对象的状态有关。实现类的方法有不同的功能，覆盖接口中的方法。状态模式和命令模式一样，也可以用于消除 if...else 等条件选择语句。
* 应用实例： 1、打篮球的时候运动员可以有正常状态、不正常状态和超常状态。 2、曾侯乙编钟中，'钟是抽象接口','钟A'等是具体状态，'曾侯乙编钟'是具体环境（Context）。
* 优点： 1、封装了转换规则。 2、枚举可能的状态，在枚举状态之前需要确定状态种类。 3、将所有与某个状态有关的行为放到一个类中，并且可以方便地增加新的状态，只需要改变对象状态即可改变对象的行为。 4、允许状态转换逻辑与状态对象合成一体，而不是某一个巨大的条件语句块。 5、可以让多个环境对象共享一个状态对象，从而减少系统中对象的个数。
* 缺点： 1、状态模式的使用必然会增加系统类和对象的个数。 2、状态模式的结构与实现都较为复杂，如果使用不当将导致程序结构和代码的混乱。 3、状态模式对"开闭原则"的支持并不太好，对于可以切换状态的状态模式，增加新的状态类需要修改那些负责状态转换的源代码，否则无法切换到新增状态，而且修改某个状态类的行为也需修改对应类的源代码。
* 使用场景： 1、行为随状态改变而改变的场景。 2、条件、分支语句的代替者。
* 注意事项：在行为受状态约束的时候使用状态模式，而且状态不超过 5 个。

## 实现
1. 员工请假程序,员工为客户端,只需要提交请假申请,得到请假是否成功的结果,无需关注内部流程.
```java
package state;

public class Test {

    public static void main(String[] args) {
        Context c = new Context();
        c.setLeaveDays(4);
        c.Handle();
    }
}
```

2. 上下文(员工只与该类打交道)来管理员工的请假信息(leaveDays),和领导对请假的意见(access),其内部有一个请假状态的实例(current),这个状态默认提交给请假者的顶头上司(StateA),且该状态可以根据需要跳转(跳转过程由State具体类控制)到其他状态(StateB,StateC),Handle调用当前状态(此时为StateA)的处理函数,传入当前Context(为了让State获取员工请假信息,并能在状态中将当前状态设置为另一种状态)
```java
package state;

public class Context {
    private State current;
    private int leaveDays;
    private boolean access;

    public Context(){
        current = new StateA();
    }

    public void setState(State current) {
        this.current = current;
    }



    public int getLeaveDays() {
        return leaveDays;
    }

    public void setLeaveDays(int leaveDays) {
        this.leaveDays = leaveDays;
    }

    public boolean isAccess() {
        return access;
    }

    public void setAccess(boolean access) {
        this.access = access;
    }

    public void Handle(){
        current.Handle(this);
    }

}
```

3. State抽象类,与其具体类,每个State都有一个处理函数,传入上下文的对象,根据对象内部的信息进行处理
```java
// 抽象类
package state;

public abstract class State {
    public abstract void Handle(Context context);
}

// 状态A,项目经理可以直接拒绝请假,或者同意请假转到状态B判断天数(可以直接在该类判断天数,并选择直接给出结果还是转到部门经理,但是为了熟悉状态模式,天数判断单独分成一个状态).
package state;

public class StateA extends State {

    @Override
    public void Handle(Context context) {
        // 实际情况根据条件判断是同意请假
        // 此处通过随机数模拟是否同意请假
        context.setAccess(Math.random()>0.3);
        if(!context.isAccess()){
            System.out.println("项目经理不同意请假");
        }else{
            // 此处改变状态
            context.setState(new StateB());

            // 改变状态后,调用新状态下的处理函数
            context.Handle();
        }
    }

}

// 状态B,根据天数判断是否需要转到部门经理
package state;

public class StateB extends State{

    @Override
    public void Handle(Context context) {
        if(context.getLeaveDays()<=3){
            System.out.println("项目经理同意请假且小于3天,请假成功");
        }else{
            System.out.println("项目经理同意请假但大于3天,转交部门经理");
            context.setState(new StateC());
            context.Handle();
        }
    }
}

// 状态C,部门经理给出结果
package state;

public class StateC extends State {

    @Override
    public void Handle(Context context) {
        // 实际情况根据条件判断是同意请假
        // 此处通过随机数模拟是否同意请假
        context.setAccess(Math.random()>0.5);
        if(!context.isAccess()){
            System.out.println("部门经理不同意请假");
        }else{
            System.out.println("部门经理同意请假");
        }
    }
}

```

