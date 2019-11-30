---
title: Java 观察者模式(Observer) 设计模式
date: 2018-04-18 09:49:25
categories: Java
---

## 一、简介
观察者模式（又被称为 发布-订阅（Publish/Subscribe）模式，属于 **行为** 型模式的一种，它定义了 **一种一对多** 的依赖关系，让多个观察者对象同时监听某一个主题对象。这个主题对象在状态变化时，会通知所有的观察者对象，使他们能够自动更新自己。

## 二、结构图
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/observer.png)

## 三、具体角色
- Subject：抽象主题（抽象被观察者），抽象主题角色把所有观察者对象保存在一个集合里，每个主题都可以有任意数量的观察者，抽象主题提供一个接口，可以增加和删除观察者对象。

- ConcreteSubject：具体主题（具体被观察者），该角色将有关状态存入具体观察者对象，在具体主题的内部状态发生改变时，给所有注册过的观察者发送通知。

- Observer：抽象观察者，是观察者者的抽象类，它定义了一个更新接口，使得在得到主题更改通知时更新自己。

- ConcrereObserver：具体观察者，实现抽象观察者定义的更新接口，以便在得到主题更改通知时更新自身的状态。

## 四、简单实例
观察者模式这种发布-订阅的形式我们可以拿微信公众号来举例，假设微信用户就是观察者，微信公众号是被观察者，有多个的微信用户关注了程序猿这个公众号，当这个公众号更新时就会通知这些订阅的微信用户。

- 具体实现代码
```java
// 抽象观察者(Observer)
interface MyObserver{
    void update(String msg);
}
// 实现具体的观察者(Concrete Observer)
class WeiinUser implements MyObserver{
    private String name;
    public WeiinUser(String name) {
        this.name = name;
    }
    @Override
    public void update(String msg) {
        System.out.println(name + " msg = " + msg);
    }
}
// 抽象被观察者(Subject)
interface MySubject{
    // 添加订阅者
    void attach(MyObserver myObserver);
    // 删除订阅者
    void detach(MyObserver myObserver);
    // 通知订阅者消息更新
    void notify(String msg);
}
// 实现具体的被观察者(Concrete Subject)
class SubscriptionSubject implements MySubject{
    // 存储订阅公众号的微信用户
    private List<MyObserver> weixinUserList = new ArrayList<MyObserver>();
    @Override
    public void attach(MyObserver myObserver) {
        weixinUserList.add(myObserver);
    }
    @Override
    public void detach(MyObserver myObserver) {
        weixinUserList.remove(myObserver);
    }
    @RequiresApi(api = Build.VERSION_CODES.N)
    @Override
    public void notify(String msg) {
        weixinUserList.forEach(obs -> obs.update(msg));
    }
}
```

- 测试代码
```java
public static void main(String[] args) {
        // 创建观察者
        MyObserver user1 = new WeiinUser("kuku");
        MyObserver user2 = new WeiinUser("lili");
        MyObserver user3 = new WeiinUser("jesse");
        // 创建被观察者
        SubscriptionSubject subscriptionSubject = new SubscriptionSubject();
        // 观察者向被观察者注册
        subscriptionSubject.attach(user1);
        subscriptionSubject.attach(user2);
        subscriptionSubject.attach(user3);
        // 被观察者 通知消息给 观察者
        subscriptionSubject.notify("你有心的消息，嘻嘻!!!");

    }
```

## 参考
- [观察者模式](https://blog.csdn.net/itachi85/article/details/50773358)