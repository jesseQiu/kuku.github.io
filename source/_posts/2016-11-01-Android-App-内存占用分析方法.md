---
title: Android App 内存占用分析方法
date: 2016-11-01 10:45:58
updated: 2016-11-01 10:45:58
categories: Android
---

> 本文主要是归纳了如何查看 Android App 的内存占用情况的方法，这对于分析内存泄漏提供很大的帮助。

### 一、使用 adb 命令查看内存信息

```bash
adb shell dumpsys meminfo <package_name>

255|root@APOS_A8:/ # dumpsys meminfo xxx
Applications Memory Usage (kB):
Uptime: 59964405 Realtime: 349971868

** MEMINFO in pid 25881 [xxx] **
                   Pss  Private  Private  Swapped     Heap     Heap     Heap
                 Total    Dirty    Clean    Dirty     Size    Alloc     Free
                ------   ------   ------   ------   ------   ------   ------
  Native Heap    28871    28796        0     1856    32960    20574    12385
  Dalvik Heap    27533    27504        0        0    33334    27614     5720
 Dalvik Other      508      508        0       20
        Stack      544      544        0       56
       Ashmem      212      212        0        0
      Gfx dev    10514     9668        0        0
    Other dev        9        0        8        0
     .so mmap    11401      272    10456     1540
    .apk mmap      108        0        4        0
    .ttf mmap      135        0       84        0
    .dex mmap     4240        0     3636        0
    .oat mmap     3378        0     1908        0
    .art mmap     3770     1892     1460        0
   Other mmap      749        4      716        0
   EGL mtrack    17120    17120        0        0
      Unknown    37169    37168        0     3416
        TOTAL   146261   123688    18272     6888    66294    48188    18105

 Objects
               Views:      302         ViewRootImpl:        5
         AppContexts:       11           Activities:        7
              Assets:        3        AssetManagers:        3
       Local Binders:       47        Proxy Binders:       44
       Parcel memory:       16         Parcel count:       63
    Death Recipients:        2      OpenSSL Sockets:        0

 SQL
         MEMORY_USED:      278
  PAGECACHE_OVERFLOW:       86          MALLOC_SIZE:       62

 DATABASES
      pgsz     dbsz   Lookaside(b)          cache  Dbname
         4       40             74       97/53/16  /data/data/xxx/databases/unit.db
         4       28             24         2/45/5  /data/data/xxx/databases/app.db
```
其中，package_name 也可以换成程序的 pid。

pid 可以通过 `adb shell ps | grep package_name` 来查找

```bash
adb shell ps | grep package_name

USER     PID   PPID  VSIZE  RSS     WCHAN    PC        NAME
u0_a162   1945  285   1272164 162472 ffffffff b6e9108c D xxx
u0_a162   2080  285   972704 36212 ffffffff b6e9184c S xxx
```
上面列出了包含 xxx 的应用包名称的所有进程。

#### 这里得到的信息非常多，重点关注如下几个字段：

1. Native/Dalvik 的 Heap 信息
具体在上面的第一行和第二行，它分别给出的是 JNI 层和 Java 层的内存分配情况，如果发现这个值一直增长，则代表程序可能出现了内存泄漏。

2. Total 的 PSS 信息
这个值就是你的应用真正占据的内存大小，通过这个信息，你可以轻松判别手机中哪些程序占内存比较大了。

### 二、整机内存查看

```bash
> adb shell cat /proc/meminfo

MemTotal:         923276 kB
MemTotal:           37996 kB
Buffers:           69252 kB
MemTotal:           288024 kB
SwapCached:         3064 kB
Active:           306192 kB
Inactive:         344540 kB
Active(anon):     139440 kB
Inactive(anon):   155708 kB
```
主要看 MemTotal、MemTotal、MemTotal 这三个：
- MemTotal is the total amount of memory available to the kernel and user space (often less than the actual physical RAM of the device, since some of that RAM is needed for the radio, DMA buffers, etc).

- MemFree is the amount of RAM that is not being used at all. The number you see here is very high; typically on an Android system this would be only a few MB, since we try to use available memory to keep processes running

- Cached is the RAM being used for filesystem caches and other such things. Typical systems will need to have 20MB or so for this to avoid getting into bad paging states; the Android out of memory killer is tuned for a particular system to make sure that background processes are killed before the cached RAM is consumed too much by them to result in such paging.

### 三、查看内存占用
```bash
> top
或者
> busybox top
```

### 四、[通过 Eclipse ADT 开发工具的 DDMS 来查看(Heap)](http://www.cnblogs.com/yejiurui/p/3472765.html)

### 五、[使用新版 Android Studio 检测内存泄露和性能](http://www.2cto.com/kf/201512/455421.html)


### 参考
- [检测App的内存占用和泄漏](http://blog.csdn.net/luohai859/article/details/49906661)
- [Android pm 命令详解](http://www.cnblogs.com/JianXu/p/5380882.html)
