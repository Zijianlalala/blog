---
title: Visitor Pattern
date: 2017-06-01 12:53:41
tags: 设计模式
---
## 访问者模式
在访问者模式（Visitor Pattern）中，我们使用了一个访问者类，它改变了元素类的执行算法。通过这种方式，元素的执行算法可以随着访问者改变而改变。这种类型的设计模式属于行为型模式。根据模式，元素对象已接受访问者对象，这样访问者对象就可以处理元素对象上的操作。
## 介绍
* 意图：主要将数据结构与数据操作分离。
* 主要解决：稳定的数据结构和易变的操作耦合问题。
* 何时使用：需要对一个对象结构中的对象进行很多不同的并且不相关的操作，而需要避免让这些操作"污染"这些对象的类，使用访问者模式将这些封装到类中。
* 如何解决：在被访问的类里面加一个对外提供接待访问者的接口。
* 关键代码：在数据基础类里面有一个方法接受访问者，将自身引用传入访问者。
* 应用实例：您在朋友家做客，您是访问者，朋友接受您的访问，您通过朋友的描述，然后对朋友的描述做出一个判断，这就是访问者模式。
* 优点： 1、符合单一职责原则。 2、优秀的扩展性。 3、灵活性。
* 缺点： 1、具体元素对访问者公布细节，违反了迪米特原则。 2、具体元素变更比较困难。 3、违反了依赖倒置原则，依赖了具体类，没有依赖抽象。
* 使用场景： 1、对象结构中对象对应的类很少改变，但经常需要在此对象结构上定义新的操作。 2、需要对一个对象结构中的对象进行很多不同的并且不相关的操作，而需要避免让这些操作"污染"这些对象的类，也不希望在增加新操作时修改这些类。
* 注意事项：访问者可以对功能进行统一，可以做报表、UI、拦截器与过滤器。
## 基本代码
* UML图
![访问者模式](Visitor.png)

* 主类测试
```java
package visitor;

public class Main {

    public static void main(String[] args) {
        ObjectStructure o = new ObjectStructure();
        o.attach(new ConcreteElementA());
        o.attach(new ConcreteElementB());

        ConcreteVisitor1 v1 = new ConcreteVisitor1();
        ConcreteVisitor2 v2 = new ConcreteVisitor2();

        o.accpet(v1);
        // ConcreteElementA被ConcreteVisitor1访问
        // ConcreteElementB被ConcreteVisitor1访问
        o.accpet(v2);
        // ConcreteElementA被ConcreteVisitor2访问
        // ConcreteElementB被ConcreteVisitor2访问
    }
}
```

* Element
```java
package visitor;

public abstract class Element {
    public abstract void accept(Visitor visitor);
}
```
* ConcreteElement
```java
package visitor;

public class ConcreteElementA extends Element {

    @Override
    public void accept(Visitor visitor) {
        visitor.visitConcreteElementA(this);
    }

    public void opterationA(){}
}


package visitor;

public class ConcreteElementB extends Element {

    @Override
    public void accept(Visitor visitor) {
        visitor.visitConcreteElementB(this);
    }

    public void opterationB(){}
}
```

* Visitor
```java
package visitor;

public abstract class Visitor {
    public abstract void visitConcreteElementA(ConcreteElementA concreteElementA);
    public abstract void visitConcreteElementB(ConcreteElementB concreteElementB);
}
```

* ConcreteVisitor
```java
package visitor;

public class ConcreteVisitor1 extends Visitor {

    @Override
    public void visitConcreteElementA(ConcreteElementA concreteElementA) {
        System.out.println(concreteElementA.getClass().getSimpleName() + "被" + this.getClass().getSimpleName() + "访问");
    }

    @Override
    public void visitConcreteElementB(ConcreteElementB concreteElementB) {
        System.out.println(concreteElementB.getClass().getSimpleName() + "被" + this.getClass().getSimpleName() + "访问");
    }
}


package visitor;

public class ConcreteVisitor2 extends Visitor {

    @Override
    public void visitConcreteElementA(ConcreteElementA concreteElementA) {
        System.out.println(concreteElementA.getClass().getSimpleName() + "被" + this.getClass().getSimpleName() + "访问");
    }

    @Override
    public void visitConcreteElementB(ConcreteElementB concreteElementB) {
        System.out.println(concreteElementB.getClass().getSimpleName() + "被" + this.getClass().getSimpleName() + "访问");
    }

}
```

* ObjectStructure
```java
package visitor;

import java.util.Vector;

public class ObjectStructure {
    private Vector<Element> elements = new Vector<Element>();

    public void attach(Element element){
        elements.add(element);
    }

    public void detach(Element element){
        elements.remove(element);
    }

    public void accpet(Visitor visitor){
        for(Element element : elements){
            element.accept(visitor);
        }
    }
}
```

## 其他
感触颇深.访问者直接看结构略有复杂,那就一点点的拆看看.
0. **不考虑扩展性**,省略所有的抽象类,并直接用其子类中的一个代替(这里就体现了李氏代换原则).
0. **不考虑客户端的复杂性**,直接在客户端调用ConcreteElement的accpet方法,可以省略掉ObjectStructure.
0. **不考虑操作与结构的紧耦合**,将ConcreteElement中accpet方法直接在类中实现.
0. 至此,就只剩下一个ConcreteElement,该类描述了数据结构,并实现所有的操作,这是像我等新手常写的代码.

现在把上面的过程反过来
0. 有一个具体类,描述了数据结构和所有操作,为了**降低操作和结构的耦合性,易于对操作进行修改**,将操作从类中抽取出来,形成Operator类(一会解释为什么不是Visitor类),并将含数据结构的类命名为Eletment.
0. 将Element实现操作的方法命名为accept(Operator operator),在operator中传入this,调用**可选的**具体方法进行操作.
0. 为了让Operator知道是谁发来的操作请求,其方法OperateEletement(Eletement eletement)接受一个Element的对象,并对该对象中的数据进行操作.
0. 至此,已经实现了**操作与数据分离**.
0. 但是,操作不会只有一个,数据也不会只有一一种,抽象这两个类.Operator和Element都成为抽象类,并有对应的ConcreteOperator和ConcreteElement类
0. 此时的Operator和Element还**都**可以随意扩展,但是扩展会增加客户端的压力
0. 现在再来说**访问者**模式,顾名思义,要解决的问题是将所有的Element都访问一遍,并**对每个对象进行不同的操作**,既然每个具体Element都有其独有的操作,所以在Opertaor类中必须封装好对所有Element的操作,也就使得具体元素变更比较困难,于是就有了**稳定的数据结构**这个应用条件,并且每个具体的Operator都有了特定的操作.
0. 可以将遍历所有Element的过程进行封装,从而形成ObjectStructure,至此,Operator提供所有Element的操作方法,不同的具体实现虽然方法不同,但进行处理的元素是特定的.Operator可以被改名为Visitor,并完成其访问工作
0. 个人认为,是ObjectStructure的出现,限制了Operator的灵活性,但这正是**访问者**要做的事.

假如不是访问者模式,仅仅是想将数据与操作分离.
* 在客户端进行操作和数据的结合
```java
public static void main(String[] args) {	
    ConcreteElementA e1 = new ConcreteElementA();
    ConcreteElementB e2 = new ConcreteElementB();

    ConcreteVisitor1 v1 = new ConcreteVisitor1();
    ConcreteVisitor2 v2 = new ConcreteVisitor2();

    e1.accept(v1);
    e1.accept(v2);
    e2.accept(v1);
    e2.accept(v2);
}
```
此时的ConcreVisitor不局限于抽象类Visitor的限制,而Visitor也只需要提供一个对某一类数据进行操作的接口



