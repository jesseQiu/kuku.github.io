---
title: RxJava Staring
date: 2018-04-17 14:34:39
updated: 2018-04-17 14:34:39
categories: Java
---

![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/rxjava.jpg)

## 一、ReactiveX

## 1. 历史
ReactiveX 是 Reactive Extensions 的缩写，一般简写为 Rx，最初是 LINQ 的一个扩展，由微软的架构师 Erik Meijer 领导的团队开发，在 2012 年 11 月开源，Rx 是一个编程模型，目标是提供一致的编程接口，帮助开发者更方便的处理异步数据流，Rx 库支持 .NET、JavaScript 和 C++。Rx的大部分语言库由 ReactiveX 这个组织负责维护，社区网站是 reactivex.io。

> LINQ（Language Integrated Query）语言集成查询是一组用于c#和Visual Basic语言的扩展。它允许编写C#或者Visual Basic代码以查询数据库相同的方式操作内存数据。

### 2. 什么是 ReactiveX
- 微软给的定义是，Rx 是一个 **函数库**，让开发者可以利用 **可观察序列** 和 **LINQ** 风格查询操作符来编写异步和基于事件的程序，使用 Rx，开发者可以用 Observables 表示异步数据流，用 LINQ 操作符查询异步数据流， 用 Schedulers 参数化异步数据流的并发处理，Rx 可以这样定义：Rx = Observables + LINQ + Schedulers。

- ReactiveX.io 给的定义是，Rx 是一个使用可观察数据流进行异步编程的 **编程接口**，ReactiveX 结合了观察者模式、迭代器模式和函数式编程的精华。

### 3. ReactiveX 的应用
很多公司都在使用 ReactiveX，例如 Microsoft、Netflix、Github、Trello、SoundCloud。

### 4. ReactiveX 宣言
ReactiveX 不仅仅是一个编程接口，它是一种 **编程思想的突破**，它影响了许多其它的程序库和框架以及编程语言。


## 二、 RxJava
RxJava 是 ReactiveX 在 JVM 上的一个实现，ReactiveX 使用 Observable 序列组合异步和基于事件的程序。RxJava 尽力做到非常轻巧，它仅关注 Observable 的抽象和与之相关的高层函数，实现为一个单独的 JAR 文件。

### 2.1 响应式编程
是一种基于 **异步数据流** 概念的编程模式

### 2.2 特点
- jar 包 < 1M 
- 轻量级的框架
- 支持 java 8 lambda 表单式
- 支持 java 6+ 和 Android 2.3+
- 支持异步和同步

### 2.3 扩展的观察者模式
- 添加了 `onCompleted()` 通知事件
- 添加了 `onError()` 事件
- 组合而不是嵌套，避免了回调地狱
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/rajava-observable.png)

### 2.4 `subscribeOn()` vs `observeOn()`
- `subscribeOn()` 改变的是 **订阅** 的线程，即 `subscribe()` 执行的线程(创建可观察对象时回调事件，只会触发一次)
- `observeOn()` 改变的是 **发送** 的线程，即 `onNext()` 的过程
- 注意：即我们在逻辑中通过操作符对数据进行修改都是在发送的过程中。除了最初 Observable 创建的过程

### 2.5  RxAndroid
- RxJava 针对 Android 平台的一个扩展，用于 Android 开发
- 提供响应式扩展组件开发，易于开发 Android 应用程序
- Schedulers/AndroidSchedulers(调度器): 解决主线程和多线程问题


## 三、学习线路

1. Java 基础知识

2. 掌握 java 中的 [观察者模式](/2018/04/18/java-observer-design/)和[迭代模式](/2018/04/13/java-iterator-design/)

3. 慕课网的 [RxJava 与 RxAndroid 基础入门](https://www.imooc.com/learn/877) 免费视频，先有个整体的认识

4. 看 [rxjava 官方文档](https://github.com/ReactiveX/RxJava/wiki/) 或 者 [给 Android 开发者的 RxJava 详解](http://gank.io/post/560e15be2dca930e00da1083)


## 四、代码示例(rx 2.x 版本)
```java
public static void main(String[] args) {
    // 创建 被观察者
    Observable observable = Observable.create(new ObservableOnSubscribe<String>() {
        @Override
        // 当有观察者注册时会调用该方法
        public void subscribe(ObservableEmitter<String> emitter) throws Exception {
            System.out.println("HelloRxJava.subscribe");
            emitter.onNext("one");
            emitter.onNext("two");
        }
    });
    // 创建 观察者 one
    Observer observerOne = new Observer() {
        @Override
        public void onSubscribe(Disposable d) {
            System.out.println("observerOne.onSubscribe");
        }
        @Override
        public void onNext(Object o) {
            System.out.println("observerOne.onNext: " + o);
        }
        @Override
        public void onError(Throwable e) {
            System.out.println("observerOne.onError");
        }
        @Override
        public void onComplete() {
            System.out.println("observerOne.onComplete");
        }
    };
    // 创建 观察者 two
    Observer observerTwo = new Observer() {
        @Override
        public void onSubscribe(Disposable d) {
            System.out.println("observerTwo.onSubscribe");
        }
        @Override
        public void onNext(Object o) {
            System.out.println("observerTwo.onNext: " + o);
        }
        @Override
        public void onError(Throwable e) {
            System.out.println("observerTwo.onError");
        }
        @Override
        public void onComplete() {
            System.out.println("observerTwo.onComplete");
        }
    };

    // 使用 just
    Observable observable = Observable.just("one","two","123'");

    // 使用 fromArray
    String[] msgs = {"one","two","123"};
    Observable observable = Observable.fromArray(msgs);

    // 观察者 注册 到 被观察者中
    observable.subscribe(observerOne);
    observable.subscribe(observerTwo);

    // 使用回调方式，直接调用
    String[] str1 = {"1","2","3"};
    Observable.fromArray(str1).subscribe(new Consumer<String>() {
        @Override
        public void accept(String s) throws Exception {
            System.out.println("s = [" + s + "]");
        }
    });
}

// 运行结果
observerOne.onSubscribe
HelloRxJava.subscribe
observerOne.onNext: one
observerOne.onNext: two

observerTwo.onSubscribe
HelloRxJava.subscribe
observerTwo.onNext: one
observerTwo.onNext: two
```


## 参考链接
- [rxjava 英文文档](https://github.com/ReactiveX/RxJava/wiki/)
- [rxjava 中文文档](https://mcxiaoke.gitbooks.io/rxdocs/content/)
- [rxjava 经典资料](https://github.com/lzyzsd/Awesome-RxJava/)
- [RxJava 与 RxAndroid 基础入门](https://www.imooc.com/learn/877)// 慕课网视频

