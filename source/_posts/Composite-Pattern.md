---
title: Composite Pattern
date: 2017-06-01 12:51:46
tags: 设计模式
---

## 组合模式
组合模式（Composite Pattern），又叫部分整体模式，是用于把一组相似的对象当作一个单一的对象。组合模式依据树形结构来组合对象，用来表示部分以及整体层次。这种类型的设计模式属于结构型模式，它创建了对象组的树形结构。
这种模式创建了一个包含自己对象组的类。该类提供了修改相同对象组的方式。

## 介绍
**意图**：将对象组合成树形结构以表示"部分-整体"的层次结构。组合模式使得用户对单个对象和组合对象的使用具有一致性。
**主要解决**：它在我们树型结构的问题中，模糊了简单元素和复杂元素的概念，客户程序可以向处理简单元素一样来处理复杂元素，从而使得客户程序与复杂元素的内部结构解耦。
**何时使用**： 1、您想表示对象的部分-整体层次结构（树形结构）。 2、您希望用户忽略组合对象与单个对象的不同，用户将统一地使用组合结构中的所有对象。
**如何解决**：树枝和叶子实现统一接口，树枝内部组合该接口。
**关键代码**：树枝内部组合该接口，并且含有内部属性 List，里面放 Component。
**应用实例**： 1、算术表达式包括操作数、操作符和另一个操作数，其中，另一个操作符也可以是操作树、操作符和另一个操作数。 2、在 JAVA AWT 和 SWING 中，对于 Button 和 Checkbox 是树叶，Container 是树枝。
**优点**： 1、高层模块调用简单。 2、节点自由增加。
**缺点**：在使用组合模式时，其叶子和树枝的声明都是实现类，而不是接口，违反了依赖倒置原则。
**使用场景**：部分、整体场景，如树形菜单，文件、文件夹的管理。
**注意事项**：定义时为具体类。

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
