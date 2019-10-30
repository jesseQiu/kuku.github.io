---
title: RxJava 函数封装
date: 2018-07-23 14:34:39
updated: 2018-07-23 14:34:39
categories: Java
---

## 一、封装同步函数
```java
public Observable
     wrappedMethod() {
      return Observable.defer(() -> {
        return Observable.just(library.synchronousMethod());
      });
    }
```
如果你的库拥有可用的同步方法，那么将其用 RxJava 封装起来的最好的方式是使用 `Observable.defer()`这会简单地延迟这个调用直到 observable 被订阅，然后在 subscription 的分配线程中执行


## 二、封装抛出异常的同步方法
```java
public Observable queryInventory(final List skus) {
  return Observable.defer(() -> {
    try {
      return Observable.just(helper.queryInventory(skus));
    } catch (IabException e) {
      return Observable.error(e);
    }
  });
}

```
将同步调用的代码用 try-catch 包起来，并根据结果返回 `Observable.just()` 或是 `Observable.error()`,这里不能直接使用 `Observable.just()` 方法，因为它抛出的 `IabException` 并不是 `RuntimeException` 的子类，因此必须被捕获。


## 三、封装带有监听器的方法
```java
public Observable setup() {
  return Observable.create( emitter -> {
    if (!helper.setupSuccessful()) {
      helper.startSetup(result -> {
        if ( emitter.isUnsubscribed()) return;
 
        if (result.isSuccess()) {
           emitter.onNext(null);
           emitter.onCompleted();
        } else {
           emitter.onError(new IabException(result.getMessage()));
        }
      });
    } else {
       emitter.onNext(null);
       emitter.onComplete();
    }
  });
}
```
封装那些使用了监听器的方法时，`Observable.just()` 并不管用，因为它一般没有返回值。我们必须使用 `Observable.create()`,这样我们就可以将监听器的结果回调给  emitter

## 参考链接
- [使用 RxJava 封装现有的库](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2016/0413/4141.html)

