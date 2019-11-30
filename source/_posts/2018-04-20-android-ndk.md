---
title: Android NDK
date: 2018-04-25 14:34:39
updated: 2018-04-25 14:34:39
categories: Android
---


![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/android-ndk.jpg)

## 一、Android NDK？
Android NDK 是在 SDK 前面又加上了“原生”二字，即 Native Development Kit，因此又被 Google 称为 “NDK”。
众所周知，Android 程序运行在 Dalvik 虚拟机中，NDK 允许用户使用类似 C/C++ 之类的原生代码语言执行部分程序。

### 1. NDK 包括
- 从 C/C++ 生成原生代码库所需要的工具和 build files
- 将一致的原生库嵌入可以在 Android 设备上部署的应用程序包文件（application packages files ，即 .apk 文件）中
- 支持所有未来 Android 平台的一系列原生系统头文件和库

### 2. 为何要用到 NDK?
- 代码的保护，由于 apk 的 java 层代码很容易被反编译，而 C/C++ 库被反编译的难度较大
- 在 NDK 中调用第三方 C/C++ 库，因为大部分的开源库都是用 C/C++ 代码编写的
- 便于移植，用 C/C++ 写的库可以方便在其他的嵌入式平台上再次使用


## 二、JNI
JNI 是 Java Native Interface 的缩写，它提供了若干的 API 实现了 Java 和其他语言的通信（主要是 C&C++ ）。从 Java1.1 开始，JNI 标准成为 java 平台的一部分，它允许 Java 代码和其他语言写的代码进行交互。JNI 一开始是为了本地已编译语言，尤其是 C 和 C++ 而设计的，但是它并不妨碍你使用其他编程语言，只要调用约定受支持就可以了。使用 java 与本地已编译的代码交互，通常会丧失平台可移植性。但是，有些情况下这样做是可以接受的，甚至是必须的。例如，使用一些旧的库，与硬件、操作系统进行交互，或者为了提高程序的性能。**JNI 标准至少要保证本地代码能工作在任何 Java 虚拟机环境**。


## 三、创建 NDK 工程
### 1. 创建一个空的 Activity 工程
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-1.png)
*AndroidStudio 3.0 上创建 NDK 项目的时候,记得勾选 include c++ support,这样会很方便。接着一路 Next 最后点击 Finish*
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-2.png)
工程会自动创建 Ndk 环境配置
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-3.png)
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-4.png)
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-5.png)
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-6.png)

### 2. 创建生成 .so 库和对应 jar 包模块
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-7.png)
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-8.png)
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-9.png)

### 3. 把第一步中的 NDK 代码移到新创建的模块中
这一步的目的主要是让 NDK 部分的代码独立一个模块，后面可以生成对应的 .so 文件和 jar 包提供给其他项目使用
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-10.png)
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-11.png)
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-12.png)
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-13.png)
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-14.png)
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-15.png)
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-16.png)


### 4. 在其他工程中使用 NDK 的 so 文件和 jar 包
![](https://raw.githubusercontent.com/Jesse-Chiu/images/master/ndk-17.png)



## 参考链接
- [Android studio3.0 JNI/NDK开发流程](https://www.jianshu.com/p/a37782b56770)
- [Android NDK开发：JNI 基础篇](https://www.jianshu.com/p/ac00d59993aa)
- [Android ABI issue analysis](https://www.jianshu.com/p/18a8a4e6af3f)
- [Android NDK 入门](https://www.imooc.com/learn/411)
- [Android NDK 进阶](https://www.imooc.com/learn/918)
- **推荐** [AndroidStudio 3.0 NDK 环境搭建](http://pvphero.github.io/2018/02/08/AS3NDKEnvironment/)
- [AndroidStudio项目之CmakeLists解析](https://blog.csdn.net/pvpheroszw/article/details/79296257)
- [AndroidStudio项目CMakeLists解析](https://www.cnblogs.com/chenxibobo/p/7678389.html)

