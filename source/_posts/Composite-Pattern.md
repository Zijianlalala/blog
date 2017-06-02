---
title: Composite Pattern
date: 2017-06-01 12:51:46
tags: 设计模式
---

## 基本代码
* UML图
![组合模式](Composite.png)

* 主类测试
```java
package composite;

public class Main {

	public static void main(String[] args) {
		// 创建一个节点root,把它视为根节点
		Composite root = new Composite("root");
		// 为根节点添加两个叶子节点
		root.add(new Leaf("Leaf A"));
		root.add(new Leaf("Leaf B"));
		
		// 创建一个节点comp
		Composite comp = new Composite("Composite X");
		comp.add(new Leaf("Leaf XA"));
		comp.add(new Leaf("Leaf XB"));
		// 将该节点设置为根节点的树枝
		root.add(comp);
		
		// 创建一个节点comp2
		Composite comp2 = new Composite("Composite XY");
		comp2.add(new Leaf("Leaf XYA"));
		comp2.add(new Leaf("Leaf XYB"));
		// 将该节点设置为comp的树枝
		comp.add(comp2);
		
		// 再添加一个节点
		root.add(new Leaf("Leaf C"));
		
		// 创建一个叶节点leaf
		Leaf leaf = new Leaf("Leaf D");
		// 设置为根的叶节点
		root.add(leaf);
		// 移除
		root.remove(leaf);
		
		// 显示,设置根节点depth为1
		root.display(1);
	}

}
```

* 抽象构件角色
```java
package composite;

public abstract class Component {
	/**
	 * 抽象构件角色
	 * */
	protected String name;

	public Component(String name) {
		this.name = name;
	}
	
	public abstract void add(Component c);
	public abstract void remove(Component c);
	public abstract void display(int depth);
}

```

* 树枝构件角色
```java
package composite;

import java.util.Vector;

public class Composite extends Component {
	/**
	 * 树枝构件角色
	 * */
	private Vector<Component> children = new Vector<Component>();

	public Composite(String name) {
		super(name);
	}

	@Override
	public void add(Component c) {
		children.add(c);
	}

	@Override
	public void remove(Component c) {
		children.remove(c);
	}

	@Override
	public void display(int depth) {
		// temp保存depth个'-'
		String temp="";
		for(int i=0;i<depth;i++){
			temp=temp+"-";
		}
		
		System.out.println(temp+super.name);
		// 递归显示节点
		for(Component c:children){
			c.display(depth+1);
		}
	}

}
```

* 树叶构建角色
```java
package composite;

public class Leaf extends Component {
	/**
	 * 树叶构件角色
	 * */
	public Leaf(String name) {
		super(name);
	}

	@Override
	public void add(Component c) {
		System.out.println("Cannot add to a leaf");
		
	}

	@Override
	public void remove(Component c) {
		System.out.println("Cannot remove from a leaf");
		
	}

	@Override
	public void display(int depth) {
		String temp="";
		for(int i=0;i<depth;i++){
			temp=temp+"-";
		}
		System.out.println(temp+super.name);
		
	}
	
}
```
