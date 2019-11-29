---
title: Java Starting
date: 2017-11-06 18:49:25
categories: Java
---

## 一、Java 分类
- JavaSE（J2SE）（Java2 Platform Standard Edition，java 平台标准版） // 基础核心版本(面向对象、API、JVM...)
- JavaEE(J2EE)(Java 2 Platform,Enterprise Edition，java 平台企业版) // 企业版(JSP、EJB、服务...)
- JavaME(J2ME)(Java 2 Platform Micro Edition，java 平台微型版) // 手机(移动设备、游戏、通信、...)


## 二、Java 环境变量配置
- [Java 官网 JDK 下载地址](https://www.oracle.com/technetwork/cn/java/javase/downloads/index.html)
- *现在下载需要注册账号*

### 1. Java Windows 环境配置
- 新建一个环境变量: JAVA_HOME
例如:
```bash
C:\Program Files (x86)\Java\jdk.xxx  // JDK 相应的版本安装目录
```

- 新建一个环节变量：CLASSPATH
```bash
.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar // 类库目录(*.jar 文件)
```
**如果下载的 JDK 版本 > 10.x 的可以不需要配置这一步**

- 在 PATH 环境变量中添加
```bash
%JAVA_HOME%\bin;
``` 

- 测试是否配置成功
```bash
java -version
java version "11.0.5" 2019-10-15 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.5+10-LTS)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.5+10-LTS, mixed mode)

javac -version
javac 11.0.5
```
如果正常显示上面信息表示设置成

- 编写代码测试
新建一个 HelloJava.java 文件
```java
public class HelloJava {
    public static void main(String[] args) {
        System.out.println("Hello World");
    }
}
```
编译测试
```bash
$javac HelloJava.java
$ java HelloJava
Hello World
```
如果正常显示表示可以正常编译 Java 文件



## 三、Java 组成
- JDK(Java Development Kit) 是 Java 语言的软件开发工具包(SDK)。在 JDK 的安装目录下有一个 jre 目录，里面有两个文件夹 bin 和 lib，在这里可以认为 bin 里的就是 jvm，lib 中则是 jvm 工作所需要的类库，而 jvm 和 lib 合起来就称为 jre。

- JRE（Java Runtime Environment，Java 运行环境），包含 JVM 标准实现及 Java 核心类库。JRE 是 Java 运行环境，并不是一个开发环境，所以没有包含任何开发工具（如编译器和调试器）
最后 JVM 也一目了然了

- JVM 是 Java Virtual Machine（Java虚拟机）的缩写，JVM 是一种用于计算设备的规范，它是一个虚构出来的计算机，是通过在实际的计算机上仿真模拟各种计算机功能来实现的。

- 包含关系: JDK > JRE > JVM

- java 语言执行流程
![](http://images.jessechiu.com/java-20180420.png)

## 四、Eclipse
- http://www.eclipse.org
- http://www.my-eclipse.cn // 拓展工具包
- 创建工程 -> 创建包 -> 创建文件 -> 运行
- 自动创建 `setter`和`getter`:在创建类的 `superclass 点击选择，输入要继承的类名`
- 自动创建 `toString()` 方法: `菜单 -> Source -> Generat toString()`
- 自动创建 `equals() 和 hashCode()` 方法: `菜单 -> Source -> Generat toString()`


## 五、基本语法
- [参考](/2018/04/09/java-simple-grammar/)


## 六、UML()
### 建模工具
- Visio(Micorsoft)
- Rational Rose(IBM)
- Power Designer

## 七、JUnit
> 是一个 Java 语言的单元测试框架。它由 Kent Beck 和 Erich Gamma 建立，逐渐成为源于 Kent Beck 的 sUnit 的 xUnit 家族中最为成功的一个。 JUnit 有它自己的 JUnit 扩展生态圈。多数 Java 的开发环境都已经集成了 JUnit 作为单元测试的工具。
JUnit 是由 Erich Gamma 和 Kent Beck 编写的一个回归测试框架（regression testing framework）。Junit 测试是程序员测试，即所谓 **白盒测试**，因为程序员知道被测试的软件如何（How）完成功能和完成什么样（What）的功能。Junit 是一套框架，继承 TestCase 类，就可以用 Junit 进行自动测试了。

## 八、Tomcat
> Tomcat 服务器是一个免费的开放源代码的 Web 应用服务器，属于轻量级应用服务器，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试 JSP 程序的首选。对于一个初学者来说，可以这样认为，当在一台机器上配置好 Apache 服务器，可利用它响应 HTML（标准通用标记语言下的一个应用）页面的访问请求。实际上 Tomcat 是 Apache 服务器的扩展，但运行时它是独立运行的，所以当你运行 tomcat 时，它实际上作为一个与 Apache 独立的进程单独运行的。

## 九、Jetty
Jetty 是一个开源的 servlet 容器，它为基于 Java 的 web 容器，例如 JSP 和 servlet 提供运行环境。Jetty 是使用 Java 语言编写的，它的 API 以一组 JAR 包的形式发布。开发人员可以将 Jetty 容器实例化成一个对象，可以迅速为一些独立运行（stand-alone）的 Java 应用提供网络和 web 连接。

## 十、Maven
> Maven 项目对象模型(POM)，可以通过一小段描述信息来管理项目的构建，报告和文档的软件项目管理工具。
Maven 除了以程序构建能力为特色之外，还提供高级项目管理工具。由于 Maven 的缺省构建规则有较高的可重用性，所以常常用两三行 Maven 构建脚本就可以构建简单的项目。由于 Maven 的面向项目的方法，许多 Apache Jakarta 项目发文时使用 Maven，而且公司项目采用 Maven 的比例在持续增长。

### 10.1 相应的命令
```bash
mvn -v // 查看 maven 版本
mvn compile // 编译
mvn test // 测试
mvn package // 打包
mvn clean // 删除 target 包
mvn install // 安装 jar 包到本地仓库 
```
### 10.2 常见错误处理
- `Could not create the Java Virtual Machine`
![maven-02](http://images.jessechiu.com/maven-02.png)
主要是由于 java 默认的最大内存不够 maven 使用，需要在启动 maven 时重新指定参数。
可以在环境变量 PATH 中手动加入 `MAVEN_OPTS:-Xms128m-Xmx512m` 参数值,如下图
![maven-01](http://images.jessechiu.com/maven-01.png)

### 10.3 学习资源: [项目管理利器——maven](http://www.imooc.com/learn/443)


## 十一、Spring
> Spring 是一个开放源代码的设计层面框架，他解决的是业务逻辑层和其他各层的松耦合问题，因此它将面向接口的编程思想贯穿整个系统应用。Spring 是于 2003 年兴起的一个轻量级的 Java 开发框架，由 Rod Johnson 创建。简单来说，Spring 是一个分层的 JavaSE/EEfull-stack(一站式) 轻量级开源框架。
> Spring MVC 属于 SpringFrameWork 的后续产品，已经融合在 Spring Web Flow 里面。Spring 

### 11.1 IOC
- IOC: 控制反转
- DI: 依赖注入

### 11.2 Bean 
EJB 是 Enterprise Java Bean 的缩写，一个 Bean 扮演着应用程序 **素材** 的角色。它包含有一个 functional interface，一个 life-cycle interface，以及一个实现它所支援的商业方法的类别。
JavaBean 是描述 Java 的软件组件模型，有点类似于 Microsoft 的 COM 组件概念。在 Java 模型中，通过 JavaBean 可以无限扩充 Java 程序的功能，通过 JavaBean 的组合可以快速的生成新的应用程序。对于程序员来说，最好的一点就是 JavaBean 可以实现代码的重复利用，另外对于程序的易维护性等等也有很重大的意义。

### 11.3 Spring MVC
Spring MVC 属于 SpringFrameWork 的后续产品，已经融合在 Spring Web Flow 里面。Spring 框架提供了构建 Web 应用程序的全功能 MVC 模块。使用 Spring 可插入的 MVC 架构，从而在使用 Spring 进行 WEB 开发时，可以选择使用 Spring 的 SpringMVC 框架或集成其他 MVC 开发框架，如 Struts1，Struts2 等。

#### 11.3.1 Spring MVC 拦截器
- 解决乱码问题
- 解决权限问题


## 十二、JDBC
>JDBC（Java DataBase Connectivity,jav 数据库连接）是一种用于执行 SQL 语句的 Java API，可以为多种 **关系数据库** 提供统一访问，它由一组用 Java 语言编写的类和接口组成。JDBC 提供了一种基准，据此可以构建更高级的工具和接口，使数据库开发人员能够编写数据库应用程序，同时，JDBC 也是个商标名。


## 十三、ORM
>对象关系映射（英语：(Object Relational Mapping，简称ORM，或O/RM，或O/R mapping），是一种程序技术，**用于实现面向对象编程语言里不同类型系统的数据之间的转换** 。从效果上说，它其实是创建了一个可在编程语言里使用的--“虚拟对象数据库”。
面向对象是从软件工程基本原则（如耦合、聚合、封装）的基础上发展起来的，而关系数据库则是从数学理论发展而来的，两套理论存在显著的区别。为了解决这个不匹配的现象，对象关系映射技术应运而生。
对象关系映射（Object-Relational Mapping）提供了概念性的、易于理解的模型化数据的方法。ORM 方法论基于三个核心原则： 
- 简单：以最基本的形式建模数据。 
- 传达性：数据库结构被任何人都能理解的语言文档化。 
- 精确性：基于数据模型创建正确标准化的结构。 
典型地，建模者通过收集来自那些熟悉应用程序但不熟练的数据建模者的人的信息开发信息模型。建模者必须能够用非技术企业专家可以理解的术语在概念层次上与数据结构进行通讯。建模者也必须能以简单的单元分析信息，对样本数据进行处理。ORM 专门被设计为改进这种联系。
简单的说：ORM 相当于中继数据。具体到产品上，例如 ADO.NET Entity Framework。DLINQ 中实体类的属性[Table] 就算是一种中继数据。

### 13.1 为什么使用 ORM
- SQL 语法版本不同: PL/SQL(Orical) 和 T/SQL(Microsoft)
- 同样的功能不同的数据库有不同的表现
- 不利于维护和扩展

### 13.2. 主流框架
- Mybatis
- Hibernate
- TopLink

## 十四、TopLink
>是位居第一的 Java 对象关系可持续性体系结构，原署 WebGain 公司的产品，后被 Oracle 收购，并重新包装为 Oracle AS TopLink。TOPLink 为在关系数据库表中存储 Java 对象和企业 Java 组件 (EJB) 提供高度灵活和高效的机制。TopLink 为开发人员提供极佳的性能和选择，可以与任何数据库、任何应用服务器、任何开发工具集和过程以及任何 J2EE 体系结构协同工作。


## 十五、Mybatis
>MyBatis 本是 apache 的一个开源项目 iBatis, 2010 年这个项目由 apache software foundation 迁移到了google code，并且改名为 MyBatis 。2013年11月迁移到 Github。
iBATIS 一词来源于 “internet” 和 “abatis” 的组合，是一个基于 Java 的持久层框架。iBATIS 提供的持久层框架包括 SQL Maps 和 Data Access Objects（sDAO）


## 十六、Hibernate
>Hibernate 是一个开放源代码的对象关系映射框架，它对 JDBC 进行了非常轻量级的对象封装，它将 POJO 与数据库表建立映射关系，是一个全自动的 orm 框架，hibernate 可以自动生成 SQL 语句，自动执行，使得 Java 程序员可以随心所欲的使用对象编程思维来操纵数据库。 Hibernate 可以应用在任何使用 JDBC 的场合，既可以在 Java 的客户端程序使用，也可以在 Servlet/JSP 的 Web 应用中使用，最具革命意义的是，Hibernate 可以在应用 EJB 的 J2EE 架构中取代 CMP，完成数据持久化的重任


## 十七、JSTL
>JSTL（JavaServer Pages Standard Tag Library，JSP 标准标签库)是一个不断完善的开放源代码的 JSP 标签库，是由 apache 的 jakarta 小组来维护的。JSTL 只能运行在支持 JSP1.2 和 Servlet2.3 规范的容器上，如 tomcat 4.x。在 JSP 2.0 中也是作为标准支持的。
JSTL 1.0 发布于 2002 年 6 月，由四个定制标记库（core、format、xml 和 sql）和一对通用标记库验证器（ScriptFreeTLV 和 PermittedTaglibsTLV）组成。core 标记库提供了定制操作，通过限制了作用域的变量管理数据，以及执行页面内容的迭代和条件操作。它还提供了用来生成和操作 URL 的标记。顾名思义，format 标记库定义了用来格式化数据（尤其是数字和日期）的操作。它还支持使用本地化资源束进行 JSP 页面的国际化。xml 库包含一些标记，这些标记用来操作通过 XML 表示的数据，而 sql 库定义了用来查询关系数据库的操作。
如果要使用JSTL，则必须将 jstl.jar 和 standard.jar 文件放到 classpath 中，如果你还需要使用 XML processing 及 Database access (SQL)标签，还要将相关 JAR 文件放到 classpath 中，这些 JAR 文件全部存在于下载回来的 zip 文件中。

## 十八、EL
>EL（Expression Language） 是为了使 JSP 写起来更加简单。表达式语言的灵感来自于 ECMAScript 和 XPath 表达式语言，它提供了在 JSP 中简化表达式的方法，让 Jsp 的代码更加简化。



## 参考:
- [Java工程师 技术栈系列视频教程](http://www.imooc.com/course/programdetail/pid/31)
- [Java入门第一季](http://www.imooc.com/learn/85)
- [Java入门第二季](http://www.imooc.com/learn/124)
- [Java入门第三季](http://www.imooc.com/learn/110)
- [Spring 入门篇](http://www.imooc.com/learn/196)
- [Spring 事物管理](http://www.imooc.com/learn/478)
- [Spring MVC 起步](http://www.imooc.com/learn/47)
- [Spring MVC 拦截器](http://www.imooc.com/learn/498)






