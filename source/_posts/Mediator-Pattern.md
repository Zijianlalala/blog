---
title: Mediator Pattern
date: 2017-06-02 11:04:32
tags: 设计模式
---
## 中介者模式
中介者模式（Mediator Pattern）是用来降低多个对象和类之间的通信复杂性。这种模式提供了一个中介类，该类通常处理不同类之间的通信，并支持松耦合，使代码易于维护。中介者模式属于行为型模式。

## 介绍 
* 意图：用一个中介对象来封装一系列的对象交互，中介者使各对象不需要显式地相互引用，从而使其耦合松散，而且可以独立地改变它们之间的交互。
* 主要解决：对象与对象之间存在大量的关联关系，这样势必会导致系统的结构变得很复杂，同时若一个对象发生改变，我们也需要跟踪与之相关联的对象，同时做出相应的处理。
* 何时使用：多个类相互耦合，形成了网状结构。
* 如何解决：将上述网状结构分离为星型结构。
* 关键代码：对象 Colleague 之间的通信封装到一个类中单独处理。
* 应用实例： 1、中国加入 WTO 之前是各个国家相互贸易，结构复杂，现在是各个国家通过 WTO 来互相贸易。 2、机场调度系统。 3、MVC 框架，其中C（控制器）就是 M（模型）和 V（视图）的中介者。
* 优点： 1、降低了类的复杂度，将一对多转化成了一对一。 2、各个类之间的解耦。 3、符合迪米特原则。
* 缺点：中介者会庞大，变得复杂难以维护。
* 使用场景： 1、系统中对象之间存在比较复杂的引用关系，导致它们之间的依赖关系结构混乱而且难以复用该对象。 2、想通过一个中间类来封装多个类中的行为，而又不想生成太多的子类。
* 注意事项：不应当在职责混乱的时候使用。


## 基本代码
* UML图
![中介者模式](Mediator.png)

* 主类测试
```java
package mediator;

public class Main {

	public static void main(String[] args) {
		// 创建中介者
		ConcreteMediator m = new ConcreteMediator();

		// 创建具体的同事,并关联中介者
		ConcreteColleague1 c1 = new ConcreteColleague1(m);
		ConcreteColleague2 c2 = new ConcreteColleague2(m);

		// 中介者维护两个同事
		m.setColleague1(c1);
		m.setColleague2(c2);

		// 通过中介者发送消息
		c1.send("哈哈");
		c2.send("吼吼");
	}

}
```

* Mediator
```java
package mediator;

public abstract class Mediator {
	// 中介者类,接收发送消息的人和消息内容
	public abstract void send(String message,Colleague colleague);
}
```

* Colleague
```java
package mediator;

public abstract class Colleague {
	// 每个具体的同事需要认识一个中介者
	// 同事之间不需要认识
	protected Mediator mediator;

	public Colleague(Mediator mediator) {
		this.mediator = mediator;
	}
}
```

* CocreteColleague
```java
package mediator;

public class ConcreteColleague1 extends Colleague {

	public ConcreteColleague1(Mediator mediator) {
		super(mediator);
	}

	// 消息发送给中介者
	public void send(String message) {
		mediator.send(message, this);
	}

	// 接收通知
	public void notify(String message) {
		System.out.println("同事1的得到消息:" + message);
	}
}


package mediator;

public class ConcreteColleague2 extends Colleague {
	public ConcreteColleague2(Mediator mediator) {
		super(mediator);
	}

	public void send(String message) {
		mediator.send(message, this);
	}

	public void notify(String message) {
		System.out.println("同事2的得到消息:" + message);
	}
}
```

* ConcreteMediator
```java
package mediator;

public class ConcreteMediator extends Mediator {
	// 中介者类降低了同事之间的耦合度
	// 但是增加了中介者类负担
	// 这个类只是为了体现中介者负责调度两个类之间的关系
	
	// 具体中介者,有两个具体同事的引用
	private ConcreteColleague1 colleague1;
	private ConcreteColleague2 colleague2;

	public void setColleague1(ConcreteColleague1 colleague1) {
		this.colleague1 = colleague1;
	}

	public void setColleague2(ConcreteColleague2 colleague2) {
		this.colleague2 = colleague2;
	}

	@Override
	public void send(String message, Colleague colleague) {
		// 同事1发出的内容直接通知给同事2
		if (colleague == colleague1) {
			colleague2.notify(message);
		} else {
			colleague1.notify(message);
		}
	}

}
```
