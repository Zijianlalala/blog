---
title: Memento Pattern
date: 2017-06-01 12:53:29
tags: 设计模式
---

## 基本代码
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
