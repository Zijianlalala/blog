---
title: Builder Pattern
date: 2017-06-01 12:51:19
tags: 设计模式
---
## 建造者模式
建造者模式（Builder Pattern）使用多个简单的对象一步一步构建成一个复杂的对象。这种类型的设计模式属于创建型模式，它提供了一种创建对象的最佳方式。

一个 Builder 类会一步一步构造最终的对象。该 Builder 类是独立于其他对象的。

## 介绍
* 意图：将一个复杂的构建与其表示相分离，使得同样的构建过程可以创建不同的表示。
* 主要解决：主要解决在软件系统中，有时候面临着"一个复杂对象"的创建工作，其通常由各个部分的子对象用一定的算法构成；由于需求的变化，这个复杂对象的各个部分经常面临着剧烈的变化，但是将它们组合在一起的算法却相对稳定。
* 何时使用：一些基本部件不会变，而其组合经常变化的时候。
* 如何解决：将变与不变分离开。
* 关键代码：建造者：创建和提供实例，导演：管理建造出来的实例的依赖关系。
* 应用实例： 1、去肯德基，汉堡、可乐、薯条、炸鸡翅等是不变的，而其组合是经常变化的，生成出所谓的"套餐"。 2、JAVA 中的 StringBuilder。
* 优点： 1、建造者独立，易扩展。 2、便于控制细节风险。
* 缺点： 1、产品必须有共同点，范围有限制。 2、如内部变化复杂，会有很多的建造类。
* 使用场景： 1、需要生成的对象具有复杂的内部结构。 2、需要生成的对象内部属性本身相互依赖。
* 注意事项：与工厂模式的区别是：建造者模式更加关注与零件装配的顺序。
## 基本代码
* UML图
![建造者模式](Builder.png)

* Main
```java
package builder;

public class Main {

	public static void main(String[] args) {
		Director director = new Director();
		
		Builder b1 = new ConcreteBuilder1();
		Builder b2 = new ConcreteBuilder2();
		
		director.construct(b1);
		director.construct(b2);
		
		Product p1 = b1.getResult();
		Product p2 = b2.getResult();
		
		p1.show();
		p2.show();
		
	}

}
```

* Product
```java
package builder;

import java.util.Vector;

public class Product {
	private Vector<String> parts = new Vector<String>();
	
	public void add(String part){
		parts.add(part);
	}
	
	public void show(){
		System.out.println("产品 创建----");
		for(String part:parts){
			System.out.println(part);
		}
	}
}
```

* Builder
```java
package builder;

public abstract class Builder {
	public abstract void builderPartA();
	public abstract void builderPartB();
	public abstract Product getResult();
}
```

* ConcreteBuilder
```java
package builder;

public class ConcreteBuilder1 extends Builder {

	private Product product = new Product();

	@Override
	public void builderPartA() {
		product.add("部件A");
	}

	@Override
	public void builderPartB() {
		product.add("部件B");
	}

	@Override
	public Product getResult() {
		return product;
	}

}

package builder;

public class ConcreteBuilder2 extends Builder {
	private Product product = new Product();

	@Override
	public void builderPartA() {
		product.add("部件X");
	}

	@Override
	public void builderPartB() {
		product.add("部件Y");
	}

	@Override
	public Product getResult() {
		return product;
	}
}

```

* Director
```java
package builder;

public class Director {
	public void construct(Builder builder){
		builder.builderPartA();
		builder.builderPartB();
	}
}
```
