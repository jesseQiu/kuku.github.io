---
title: JSP Starting
date: 2017-11-03 18:49:25
tags: JSP
categories: Java
---

## 一、JSP
>JSP 全名为 Java Server Pages，中文名叫 java 服务器页面，其根本是一个简化的 Servlet 设计，它是由 Sun Microsystems 公司倡导、许多公司参与一起建立的一种动态网页技术标准。JSP 技术有点类似 ASP 技术，它是在传统的网页 HTML（标准通用标记语言的子集）文件(*.htm,*.html) 中插入 Java 程序段 (Scriptlet)和 JSP 标记(tag)，从而形成 JSP 文件，后缀名为(*.jsp)。 用 JSP 开发的 Web 应用是跨平台的，既能在 Linux 下运行，也能在其他操作系统上运行。
它实现了 Html 语法中的 java 扩展（以 <%, %>形式）。JSP 与 Servlet 一样，是在服务器端执行的。通常返回给客户端的就是一个 HTML 文本，因此客户端只要有浏览器就能浏览。
JSP 技术使用 Java 编程语言编写类 XML 的 tags 和 scriptlets，来封装产生动态网页的处理逻辑。网页还能通过 tags 和 scriptlets 访问存在于服务端的资源的应用逻辑。JSP 将网页逻辑与网页设计的显示分离，支持可重用的基于组件的设计，使基于 Web 的应用程序的开发变得迅速和容易。 JSP(Java Server Pages)是一种动态页面技术，它的主要目的是将表示逻辑从 Servlet 中分离出来。
Java Servlet 是 JSP 的技术基础，而且大型的 Web 应用程序的开发需要 Java Servlet 和 JSP 配合才能完成。JSP 具备了 Java 技术的简单易用，完全的面向对象，具有平台无关性且安全可靠，主要面向因特网的所有特点。


### 1. 相关知识点
- 动态网页的动态是指能与用户进行交互，例如登入
- 请求重定向: `response.sendRedirect()` 本质是两次请求，前一次请求对象不会保存，地址栏 url 会变。
- 请求转发: `request.getRequestDispatcher().forward(req.resp)` 服务器行为，一次请求，转发后请求对象会保存，地址栏的 url 地址不会改变。

### 2. JavaBean
>JavaBean 就是符号某种设计规范的 java 类型，属性私有，无参构造函数，使用 setter 和 getter 操作属性值。

- 使用 `javabean` 方法

方法一:
```java
<%@page import="com.student.User" %>
<% 
	User user = new User();
	user.setName("jesse");
	user.setPassword("888888");
%>
<%= user.getName()%>
<%= user.getPassword()%>
```

方法二:
```java
<jsp:useBean id="user" class="com.student.User" scope="page"/>
<jsp:setProperty name="user" property="name" value="jesse"/>
<jsp:setProperty name="user" property="password" value="888888"/>
<jsp:getProperty name="user" property="name" />
<jsp:getProperty name="user" property="password" />
// 或者
<%= user.getName()%>
<%= user.getPassword()%>
```

- scope 作用范围:`page、application、session`

## 二、Tomcat 服务器
Tomcat 是 Apache 软件基金会（Apache Software Foundation）的 Jakarta 项目中的一个核心项目，由 Apache、Sun 和其他一些公司及个人共同开发而成。由于有了 Sun 的参与和支持，最新的 Servlet 和 JSP 规范总是能在 Tomcat 中得到体现，Tomcat 5 支持最新的 Servlet 2.4 和 JSP 2.0 规范。因为 Tomcat 技术先进、性能稳定，而且免费，因而深受 Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的 Web 应用服务器。
Tomcat 服务器是一个免费的开放源代码的 Web 应用服务器，属于 **轻量级** 应用服务器，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试 JSP 程序的首选。对于一个初学者来说，可以这样认为，当在一台机器上配置好Apache 服务器，可利用它响应 HTML（标准通用标记语言下的一个应用）页面的访问请求。实际上 Tomcat 是 Apache 服务器的扩展，但运行时它是独立运行的，所以当你运行 tomcat 时，它实际上作为一个与 Apache 独立的进程单独运行的。
诀窍是，当配置正确时，Apache 为 HTML 页面服务，而 Tomcat 实际上运行 JSP 页面和 Servlet。另外，Tomcat 和 IIS 等 Web 服务器一样，具有处理 HTML 页面的功能，另外它还是一个 Servlet 和 JSP 容器，独立的 Servlet容器是 Tomcat 的默认模式。不过，Tomcat 处理静态 HTML 的能力不如 Apache 服务器。目前 Tomcat 最新版本为 9.0。

### 2.1. 环境配置
- Tomcat [下载](https://tomcat.apache.org/download-70.cgi)
- 设置环境变量: 
```bash
CATALINA_HOME:E:\program\apache-tomcat-7.0.82
PATH:%CATALINA_HOME%\bin;
```
- 启动和关闭
```bash
startup
```
浏览器输入: `http://localhost:8080` 如果看到左上叫有一只猫，就表示成功

### 2.2 Tomcat 目录结构
![](http://images.jessechiu.com/tomcat.png)

### 2.3 WEB-INF 目录
WEB-INF 是 java 的 web 应用安全目录，所谓安全就是客户端无法访问，只有服务端可以访问的目录
- web.xml: 项目部署文件
```xml
// 配置默认页面
<welcome-file-list>
    <welcome-file>/login.jsp</welcome-file>
</welcome-file-list>
// ps 如果修改了配置文件，建议重启 Tomcat 服务器
```
- classes 文件夹: 用以放置 *.class 文件
- lib 文件夹: 用于存放需要的 jar 包
- [IntelliJ IDEA 创建 JavaWeb 工程](https://blog.csdn.net/zmx729618/article/details/71439154)



## 三、Servlet Applet
>Servlet（Server Applet）是 Java Servlet 的简称，称为 **小服务程序或服务连接器**，用 Java 编写的服务器端程序，主要功能在于交互式地浏览和修改数据，生成动态 Web 内容。
狭义的 Servlet 是指 Java 语言实现的一个接口，广义的 Servlet 是指任何实现了这个 Servlet 接口的类，一般情况下，人们将 Servlet 理解为后者。Servlet 运行于支持 Java 的应用服务器中。从原理上讲，Servlet 可以响应任何类型的请求，但绝大多数情况下 Servlet 只用来扩展基于 HTTP 协议的 Web 服务器。
最早支持 Servlet 标准的是 JavaSoft 的 Java Web Server，此后，一些其它的基于 Java 的 Web 服务器开始支持标准的 Servlet。

### 3.1 绝对路径和相对路径
绝对路径
```jsp
// 错误的 绝对路径
<a href="/servlet/HelloWorld"></a> // 不能正确访问
// 正确的 绝对路径写法
<a href="<%= path %>/servlet/HelloWorld"></a> // 不能正确访问
```
相对路径
```jsp
<a href="servlet/HelloWorld"></a>
```
**ps:但是在配置文件中 `<url-pattern>/servlet/HelloWorld</url-pattern>`**必须以 `/` 开头


## 参考
- [JAVA遇见HTML——JSP篇](http://www.imooc.com/learn/166)
- [JAVA遇见HTML——Servlet篇](http://www.imooc.com/learn/269)