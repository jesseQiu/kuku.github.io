---
title: Java 设计模式(Design Patterns)
date: 2018-04-10 09:49:25
categories: Java
---

- 简单的工厂设计模式
- 静态代理设计模式
- 适配器(Adapter)设计模式  

## 一、简单的工厂设计模式
```java
public class Main {
    public static void main(String[] args) {
        // 使用工厂模式来降低两者之间耦合
        Product phone = ProductFactory.getProduct("phone");
        if(phone != null){
            phone.work();
        }
    }
}
// 定义产品接口
interface Product{
    void work();
}
// 实现手机接口
class Phone implements Product{
    public void work(){
        System.out.println("开始生产手机...");
    }
}
// 实现电脑接口
class Computer implements Product{
    public void work(){
        System.out.println("开始生产电脑...");
    }
}
// 定义工厂类
class ProductFactory{
    public static Product getProduct(String name){
        if("phone".equals(name)){
            return new Phone();
        }else if("computer".equals(name)){
            return new Computer();
        }else{
            return null;
        }
    }
}
```

## 二、代理设计模式

### 2.1 静态代理设计模式
- 定义: 为其它对象提供一种代理以控制这个对象的访问(真实对象的代表)
```java
public class Main {
    public static void main(String[] args) {
        // 实例具体要被代理的对象
        UserAction userAction = new UserAction();
        // 使用代理来控制被代理实例操作
        ActionProxy actionProxy = new ActionProxy(userAction);
        actionProxy.doAction();
    }
}
// 定义接口
interface Action{
    void doAction();
}
// 接口实现
class UserAction implements Action{
    public void doAction(){
        // 模拟耗时操作
        for (int i = 0; i < 1000000; i++) { }
        System.out.println("用户开始工作...");
    }
}
// 代理接口实现，同样实现 Action 接口
class ActionProxy implements Action{
    private final Action target;
    public ActionProxy(Action target){
        this.target = target;
    }
    public void doAction(){
        long startTime = System.currentTimeMillis();
        // do something...
        // 这里可以根据条件控制 target 对象的访问调用
        this.target.doAction();
        long endTime = System.currentTimeMillis();
        System.out.println("工耗时: " + (endTime - startTime));
    }
} 
```

### 2.2 动态代理设计模式


## 三、适配器(Adapter)设计模式
- 定义: 将一个类的接口转换成客户希望的另一个接口。适配器模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。
```java
public class Main {
    public static void main(String[] args) {
        PowerA powerA = new PowerAPlug();
        work(powerA);

        PowerB powerB = new PowerBPlug();
        // work(powerB); // 不能满足客户接口需求

        // 通过适配器使得 PowerB 满足客户接口
        Adapter adapter = new Adapter(powerB);
        // 因为 Adapter 是对 PowerA 接口的实现，可以正常调用
        work(adapter);
    }
    // 客户的接口
    private static void work(PowerA powerA) {
        System.out.println("正在连接...");
        powerA.insert();
        System.out.println("工作结束");
    }
}
// 定义接口
interface PowerA{
    void insert();
}
// 实现可以正常满足客户需求的类
class PowerAPlug implements PowerA{
    public void insert(){
        System.out.println("电源 A 开始工作...");
    }
}
// 定义接口
interface PowerB{
    void connect();
}
// 实现一个不能满足客户需求的类
class PowerBPlug implements PowerB{
    public void connect(){
        System.out.println("电源 B 开始工作...");
    }
}
// 实现一个适配器，以达到接口 PowerB 能在客户接口中使用
class Adapter implements PowerA{
    private PowerB powerB;
    public Adapter(PowerB powerB){
        this.powerB = powerB;
    }
    // 实现接口 PowerA 的方法
    public void insert(){
        // 实际调用的是接口 PowerB 中的方法，以达到适配目的
        powerB.connect();
    }
}
```


