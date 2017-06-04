---
title: Chain of Responsibility Pattern
date: 2017-06-01 12:52:38
tags: 设计模式
---
## 责任链模式

## 基本代码
* UML图
![责任链模式](Chain-of-Responsibility.png)

* 
```java
package chainOfResponsibility;

public class Main {
    public static void main(String[] args) {
        Handler h1 = new ConcreteHandler1();
        Handler h2 = new ConcreteHandler2();
        Handler h3 = new ConcreteHandler3();
        // 责任链的跳转在客户端设置
        h1.setSuccessor(h2);
        h2.setSuccessor(h3);

        int[] requests={2,5,14,22,18,3,27,20};
        for(int request : requests){
            h1.HandleRequest(request);
        }
    }

}
```

* 维护一个自身的对象,可以自己处理请求或者交给维护的对象处理
```java
package chainOfResponsibility;

public abstract class Handler {
    protected Handler successor;
    public void setSuccessor(Handler successor){
        this.successor = successor;
    }

    public abstract void HandleRequest(int request);
}
```

* 具体实现类
```java
package chainOfResponsibility;

public class ConcreteHandler1 extends Handler {

    @Override
    public void HandleRequest(int request) {
        if (request >= 0 && request < 10) {
            String temp = this.getClass().getSimpleName() + " 处理 " + request;
            System.out.println(temp);
        } else if (successor != null) {
            successor.HandleRequest(request);
        }
    }
}


package chainOfResponsibility;

public class ConcreteHandler2 extends Handler {

    @Override
    public void HandleRequest(int request) {
        if (request >= 10 && request < 20) {
            String temp = this.getClass().getSimpleName() + " 处理 " + request;
            System.out.println(temp);
        } else if (successor != null) {
            successor.HandleRequest(request);
        }
    }
}


package chainOfResponsibility;

public class ConcreteHandler3 extends Handler {

    @Override
    public void HandleRequest(int request) {
        if (request >= 20 && request < 30) {
            String temp = this.getClass().getSimpleName() + " 处理 " + request;
            System.out.println(temp);
        } else if (successor != null) {
            successor.HandleRequest(request);
        }
    }
}
```
## 其他
* 责任链模式的跳转位置在客户端决定,状态模式的跳转在状态类内实现
