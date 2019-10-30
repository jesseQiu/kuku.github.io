---
title: Android 日志分类
date: 2018-07-02 14:34:39
updated: 2018-07-02 14:34:39
categories: Android
---

## Android 日志分类
### 1. ANR(Application Not Responding)
主要是说应用程序出现无响应的情况。在这个情况出现的时候同时在手机界面会弹出响应的对话框，提示应用程序无响应　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　 

#### 1.1 ANR 的几种类型:

当运行指定的APP，如果 Android 系统检测到符合下边的几种条件那就会弹出应用程序无响应的界面。　　　

- 按键超时　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　
Android 默认的响应时间是 **5s**，如果一个触屏事件超过 **5s**，那么就会发生此现象。　　　　　　　                 
- 广播超时
广播的默认响应时间是 **10s**，如果一个广播在 **10s** 之内还没有执行完，那么就会出现此现象。                          
- 服务超时
服务的默认响应事件是 **20s**,如果请求的服务在 **20s** 内失败，那么就会发生此现象。


### 1.2 ANR 事件与异常的区别
ANR 事件是由于一些操作的原因或者是反应事件较慢会出现程序无响应的情况，而异常是程序由于代码或者是一些其他的原因出现程序停止运行的情况，这两种情况的性质是完全不一样的。　　　　　　　   

### 1.3 与 ANR 相关的一些东西
- Java 进程分为主线程和主消息队列
- 主线程主要是用来处理一些有关 UI 事件，比如画图，监听或是接受 UI 事件。　　　　　　　　　　　　   
- 主线程还要从主消息队列取出一些消息并且能够尽可能快的处理这些消息
- 在主线程还没有处理完当前的消息的时候，他是不能再去取下一条消息的
- 如果主线程没有及时处理掉这个消息，那么 ANR 事件就会发生。　　　　　　　　　　　　　　　　　 

### 1.4 如何分析 ANR
- 首先需要找出 ANR 发生的事件，进程号，还有 ANR 的类型。　　　　　　　　　　　　　　　　　　　　
- 检查 CPU 的内存使用情况。　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　
- 需要检查文件 trace.txt，还有映射进程，时间戳等信息。　　　　　　　　　　　　　　　　　　　　　　
- 通过 main log 和 event log 寻找更多的有用信息，用来分析问题　　　　　　　　　　　　　　　　   

### 1.5 分析 ANR 需要注意的地方：
- 尽可能使用 eng 版本去抓取 log，因为 eng 版本抓取的 log 是最全面的，有些 log 在 eng 版本下才会打开　
- 我们还需要抓取一些很重要的 log 文件，提供给 MTK 分析使用，让他们帮着一起分析问题所在

### 1.6 导出日子方法：
anr 问题的 log 一般都在 `/data/anr/` 目录下，使用如下命令即可导出 log

```bash
adb pull /data/anr/traces.txt   d:/ 
// 将手机上的 traces.txt 导出到电脑的 d 目录下
```

但是也会有该命令失效的时候。你能 adb shell ls /data/anr/  看到该文件，但是导出时会提示该文件不存在，原因没有去跟，但是导出的方式可以如下：
```bash
adb shell 
cat  /data/anr/xxx   >/mnt/sdcard/yy/zz.txt   
exit
adb pull /mnt/sdcard/yy/zz.txt  d: // 即可将文件导出到了d盘。
```

### 2. FC(Force Close)
Force Close,意为强行关闭，当前应用程序发生了冲突，例如:
- NullPointExection（空指针）
- IndexOutOfBoundsException（下标越界），
- Android API 使用的顺序错误也可能导致（比如 setContentView() 之前进行了 findViewById()操作）等等一系列未捕获异常

解决办法:
```java
class MyRunnable extends Thread implements Thread.UncaughtExceptionHandler {
 
  int a[];
  @Override
  public void run() {
      // TODO Auto-generated method stub
      Thread.setDefaultUncaughtExceptionHandler(this);
      int i = a[0];//异常
  }
  @Override
  public void uncaughtException(Thread arg0, Throwable arg1) {
      // TODO Auto-generated method stub
      Log.i("tag", "childThread:截获到forceclose，异常原因为：" + "\n" +
              arg1.toString()+"  Thread->"+arg0.getId()+" 本线程id->"+Thread.currentThread().getId()+" "+
              Thread.currentThread().getName());
      android.os.Process.killProcess(android.os.Process.myPid());
  }

}
```


## 参考
- [Application Not Responding(ANR)的事件分析](https://blog.csdn.net/u010586698/article/details/51213092)
- [关于Android Force Close 出现的原因 以及解决方法](https://blog.csdn.net/weixin_37730482/article/details/70830844)
