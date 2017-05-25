---
title: Java Web浅析
date: 2017-04-26 20:40:08
tags: Java
---

## 准备
* servlet容器基本概念:
Tomcat是Servlet的运行环境,即一个Servlet容器。 
Servlet容器的作用是负责处理客户请求,当客户请求来到时,Servlet容器获取请求,然后调用某个Servlet,并把Servlet的执行结果返回给客户.
Servlet容器的工作过程是:当客户请求某个资源时,Servlet容器使用ServletRequest对象把客户的请求信息封装起来,然后调用java Servlet API中定义的Servlet的一些生命周期方法,完成Servlet的执行,接着把Servlet执行的要返回给客户的结果封装到 ServletResponse对象中,最后Servlet容器把客户的请求发送给客户,完成为客户的一次服务过程.每一个Servlet的类都执行 init(),service(),destory()三个函数的自动调用,在启动时调用一次init()函数用以进行参数的初始化,在服务期间每当接收到对该Servlet的请求时都会调用Service()函数执行该Servlet的服务操作,当容器销毁时调用一次destory()函数。 
典型的Servlet应用是监听器,过滤器的实现.
* MVC全名是Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写,一种软件设计典范,用一种业务逻辑,数据,界面显示分离的方法组织代码,将业务逻辑聚集到一个部件里面,在改进和个性化定制界面及用户交互的同时,不需要重新编写业务逻辑.MVC被独特的发展起来用于映射传统的输入,处理和输出功能在一个逻辑的图形化用户界面的结构中
* Servlet(Server Applet),全称Java Servlet.是用Java编写的服务器端程序.其主要功能在于交互式地浏览和修改数据,生成动态Web内容.狭义的Servlet是指Java语言实现的一个接口,广义的Servlet是指任何实现了这个Servlet接口的类,一般情况下,人们将Servlet理解为后者.
Servlet运行于支持Java的应用服务器中.从原理上讲,Servlet可以响应任何类型的请求,但绝大多数情况下Servlet只用来扩展基于HTTP协议的Web服务器.
* XML是一种元标记语言,强调以数据为核心,这两大特点在XML的众多技术特点中最为突出,同时也奠定了XML在信息管理中的优势.
与HTML不同,XML不是一种具体的标记语言,它没有固定的标记符号,是一种元标记语言,是一种用来定义标记的标记语言,它允许用户自己定义一套适于应用的DTD.
在一个普通的文档里,往往混合有文档数据,文档结构,文档样式三个要素.而对于XML文档来说,数据是其核心.将样式与内容分离,是XML的巨大优点.一方面可以使应用程序轻松的从文档中寻找并提取有用的数据信息,而不会迷失在混乱的各类标签中;另一方面,由于内容与样式的独立,也可以为同一内容套用各种样式,使得显示方式更加丰富,快捷.
DTD(Document Type Definition)的作用是定义允许或不允许什么在文档中出现.DTD的结构:一般由元素类型声明,属性表声明,实体声明,记号声明等构成.一个典型的文档类型定义文件会把未来所要创作的XML文档的元素结构,属性类型,实体引用等预先进行规定.用户既可以直接在XML文档中定义DTD,也可以通过URL引用外部的DTD.DTD为XML文档的编写者和处理者提供了共同遵循的原则,使得与文档相关的各种工作有了统一的标准.
* EL(Expression Language) 目的:为了使JSP写起来更加简单.表达式语言的灵感来自于 ECMAScript 和 XPath 表达式语言,它提供了在JSP 中简化表达式的方法,让Jsp的代码更加简化.讲道理EL贼好用,用过的都说好
* JSTL（JSP Standard Tag Library，JSP标准标签库)是一个不断完善的开放源代码的JSP标签库，是由apache的jakarta小组来维护的。JSTL只能运行在支持JSP1.2和Servlet2.3规范的容器上，如tomcat 4.x。在JSP 2.0中也是作为标准支持的。

### 部署时
#### 文件描述 
* web.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"id="WebApp_ID" version="3.1">
    <display-name>designPattern</display-name>
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
    </welcome-file-list>

    <context-param>
        <param-name>driver</param-name>
        <param-value>org.sqlite.JDBC</param-value>
    </context-param>
    
    <context-param>
        <param-name>database</param-name>
        <param-value>jdbc:sqlite:/home/n/workspace/designPattern_web/pattern.db</param-value>
    </context-param>
</web-app>
```

* 数据库连接类,这只是一个普通的Java类.它的任务是作为一个属性值,被ServletContext实例化,并设置在ServletContext中,以便Servlet获取
```java
package pattern;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class DatabaseConnect {
    private String driver;
    private String database;
    private Connection c;

    public DatabaseConnect(String driver, String database) {
        this.driver = driver;
        this.database = database;
    }

    public Connection getConnection() {
        try {
            Class.forName(driver);
            c = DriverManager.getConnection(database);

        } catch (Exception e) {
            System.out.println(e);
        }
        return c;
    }

    public void close() {
        try {
            c.close();

        } catch (SQLException e) {
            System.out.println(e);
        }
    }
}

```

* 监听者类实现ServletContextListener,它得到上下文的初始化参数,**创建数据库链接类的实例**,并把这个实例**设置为上下文属性**(**不推荐**)
```java
package pattern;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

@WebListener
public class InitDatabaseListener implements ServletContextListener {

    public InitDatabaseListener() {
    }

    public void contextDestroyed(ServletContextEvent sce) {
        DatabaseConnect connect = (DatabaseConnect) sce.getServletContext().getAttribute("connect");
        connect.close();
    }

    public void contextInitialized(ServletContextEvent sce) {
        ServletContext servletContext = sce.getServletContext();
        String driver = servletContext.getInitParameter("driver");
        String database = servletContext.getInitParameter("database");
        DatabaseConnect connect = new DatabaseConnect(driver, database);
        servletContext.setAttribute("connect", connect.getConnection());
    }

}
```

* 此类实现另一版本数据库连接,通过监听器获取配置文件中的属性,保存在静态变量中,每次使用的时候通过静态变量创建连接,用完断开(**推荐**)
```java
package bean;

import java.sql.Connection;
import java.sql.SQLException;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextAttributeListener;
import javax.servlet.ServletContextEvent;
import javax.servlet.ServletContextListener;
import javax.servlet.annotation.WebListener;

import pattern.DatabaseConnect;

@WebListener
public class ConnectBean implements ServletContextListener, ServletContextAttributeListener{

    private Connection connect;
    private static String driver;
    private static String database;
    public ConnectBean() {
        this.connect = new DatabaseConnect(driver, database).getConnection();
    }

    public void contextInitialized(ServletContextEvent sce) {
        ServletContext servletContext = sce.getServletContext();
        driver = servletContext.getInitParameter("driver");
        database = servletContext.getInitParameter("database");
    }

    public Connection getConnection(){
        return this.connect;
    }
    public void close(){
        try {
            connect.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

* 这个类对应一个数据表
```java
package pattern;

public class Info {
    // 与数据库中的INFO表对应
    private String id;
    private String name;
    private String introduction;
    private String code;
    private String essence;

    public Info(String id, String name, String introduction, String code, String essence) {
        super();
        this.id = id;
        this.name = name;
        this.introduction = introduction;
        this.code = code;
        this.essence = essence;
    }


    public Info(String id, String name) {
        super();
        this.id = id;
        this.name = name;
    }


    public Info() {
        super();
        this.id = null;
        this.name = null;
        this.introduction = null;
        this.code = null;
        this.essence = null;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getIntroduction() {
        return introduction;
    }

    public void setIntroduction(String introduction) {
        this.introduction = introduction;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public String getEssence() {
        return essence;
    }

    public void setEssence(String essence) {
        this.essence = essence;
    }

}

```


* Servlet~~从上下文得到数据库连接类的实例~~通过ConnectBean获得数据库链接,得到的为Object类的对象,需要强制转化成连接类的对象
```java
package pattern;

import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.Vector;

import javax.servlet.RequestDispatcher;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import bean.ConnectBean;

@WebServlet("/content.do")
public class Content extends HttpServlet {
    private static final long serialVersionUID = 1L;

    public Content() {
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response)
        throws ServletException, IOException {
            response.setContentType("text/html");
            // 不推荐的方法,因为数据库一直连接,所以不需要断开
            // Connection connect =
            // (Connection)getServletContext().getAttribute("connect");


            // 推荐的方法,每次使用创建连接
            ConnectBean connectbean = new ConnectBean();
            Connection connect = connectbean.getConnection();

            try {
                PreparedStatement pstmt = connect.prepareStatement("SELECT * FROM INFO");
                ResultSet rs = pstmt.executeQuery();
                Vector<Info> v = new Vector<Info>();

                while (rs.next()) {
                    v.add(new Info(rs.getString(1), rs.getString(2)));
                }
                rs.close();
                pstmt.close();

                // 使用完成后断开连接
                connectbean.close();

                request.setAttribute("content", v);
                RequestDispatcher view = request.getServletContext().getRequestDispatcher("/content.jsp");
                view.forward(request, response);
            } catch (SQLException e) {
                e.printStackTrace();
            }

            response.getWriter().append("Served at: ").append(request.getContextPath());
        }
}

```

* content.jsp
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8"%>
<%@ page import="java.util.*"%>
<%@ page import="java.sql.Connection"%>
<%@ page import="pattern.*"%>
<!--声明使用JSTL-->
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>

<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<link rel="stylesheet" type="text/css" href="style.css">
<link rel="icon" href="favicon.ico">
<title>目录</title>
</head>
<body>
    <div id="content">
        <!--使用JSTL中的forEach-->
        <c:forEach var="temp" items="${content}">
            ${temp['name']}<hr/>
        </c:forEach>
    </div>
</body>
</html>
```

1. Web应用启动时,容器读取这个应用的部署描述文件,为这个应用创建一个新的ServletContext
2. 为每个上下文初始化参数创建一个String名/值对,将名/值参数的引用交给ServletContext
3. 创建一个监听者,调用监听者的contextInitiallzed()方法,创建对象并将其设为属性

### 运行时
1. 客户发送get或者post请求
2. 容器通过@WebServlet("/content.do"),将请求发送到真正的servlet
3. 通过request.setAttribute("属性名",属性值)设置属性
4. 创建视图RequestDispatcher view = request.getServletContext().getRequestDispatcher("/content.jsp")
5. view.forward(request, response);
6. 转到jsp,通过EL获取属性,并显示

## 注意事项
1. JSP & Servlet通过request.getParameter("name") 获取表单提交的参数
2. Servlet通过getServletContext().getAttribute("name") 获取上下文初始化属性
3. Servlet通过request.getSession().setAttribute("name", value) 设置会话属性
4. Servlet通过request.getSession().getAttribute("name") 获取会话属性
5. Listener的上下文初始化函数传入ServletContextEvent类型的对象(sce)
6. Listener通过sce.getServletContext() 创建ServletContext的对象(servletContext);
7. Listener通过servletContext.getInitParameter("name")获取配置文件中的参数
8. 以上获取的**属性**均为Object类的对象,需要进行**类型转化**
9. 使用JSTL时,复制tomcat目录下/webapps/examples/WEB-INF/lib中的两个文件,到WebContent/WEB-INF/lib目录中,刷新工程.直接在JSP中添加taglib.**无需对web.xml进行任何修改**


