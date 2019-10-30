---
title: Android Starting
date: 2018-03-28 09:49:25
categories: Android
---
>Android 应用采用 Java 编程语言编写。Android SDK 工具将您的代码 — 连同任何数据和资源文件 — 编译到一个 APK：Android 软件包，即带有 .apk 后缀的存档文件中。一个 APK 文件包含 Android 应用的所有内容，它是基于 Android 系统的设备用来安装应用的文件。

安装到设备后，每个 Android 应用都运行在自己的安全沙箱内：

- Android 操作系统是一种多用户 Linux 系统，其中的每个应用都是一个不同的用户；

- 默认情况下，系统会为每个应用分配一个唯一的 Linux 用户 ID（该 ID 仅由系统使用，应用并不知晓）。系统为应用中的所有文件设置权限，使得只有分配给该应用的用户 ID 才能访问这些文件；

- 每个进程都具有自己的虚拟机 (VM)，因此应用代码是在与其他应用隔离的环境中运行；

- 默认情况下，每个应用都在其自己的 Linux 进程内运行。Android 会在需要执行任何应用组件时启动该进程，然后在不再需要该进程或系统必须为其他应用恢复内存时关闭该进程。

Android 系统可以通过这种方式实现最小权限原则。也就是说，默认情况下，每个应用都只能访问执行其工作所需的组件，而不能访问其他组件。 这样便营造出一个非常安全的环境，在这个环境中，应用无法访问系统中其未获得权限的部分。

不过，应用仍然可以通过一些途径与其他应用共享数据以及访问系统服务：

- 可以安排两个应用共享同一 Linux 用户 ID，在这种情况下，它们能够相互访问彼此的文件。 为了节省系统资源，可以安排具有相同用户 ID 的应用在同一 Linux 进程中运行，并共享同一 VM（应用还必须使用相同的证书签署）。

- 应用可以请求访问设备数据（如用户的联系人、短信、可装载存储装置 [SD 卡]、相机、蓝牙等）的权限。 用户必须明确授予这些权限。 如需了解详细信息，请参阅 使用系统权限。


## 一、开发环境
 ### 1. Eclipse
- JDK(Java Development Kit)(工具集成了 JRE(java 运行环境))
- Eclipse(IDE 工具)
- Android SDK(Software Development Kit)
- ADT(Android Development Tools)
- ![android-eclipse-目录结构](http://images.jessechiu.com/android-eclipse-%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.png)

### 2. Android Studio(集成环境)

## res 和 assets　区别
- res: 实际生成 apk 时包含的资源
- assets: 只有用到部分会打包到 apk 中


## 二、Android 四大组件
- Activity(表示具有用户界面的单一屏幕)
- Service(服务是一种在后台运行的组件，用于执行长时间运行的操作或为远程进程执行作业)
- Content Provider(内容提供程序管理一组共享的应用数据) 
- Broadcast Receiver(广播接收器是一种用于响应系统范围广播通知的组件)

## 三、数据库
### 1. sharePreferences(主要用来存储配置参数)
### 2. SQLite
### 3. Content Provider
### 4. File

## 四、系统服务
- MountService: 监听是否有 SD 卡安装及移除
- ClipboardService: 提供剪切板功能
- PackageManagerService:提供软件包安装及移除查看
- 电量、网络连接状态...
- getSystemService()// Activity 方法

## 五、杂食
- Android 应用并没有单一入口点（例如，没有 main() 函数）,因为应用的启动可以通过其中某个组件。
- 只用系统可以调用各个应用的某个组件(A 要调用 B 某个组件，需要先通过 intent 向系统发送消息，再由系统调起 B 应用)
- 四种组件类型中的三种 — Activity、服务和广播接收器 — 通过名为 Intent 的异步消息进行启动。Intent 会在运行时将各个组件相互绑定（您可以将 Intent 视为从其他组件请求操作的信使），无论组件属于您的应用还是其他应用。
- intent: 信使，在不同页面间传递数据

- 文件系统
	* openFileOutput()//写文件
	* openFileInput()//读取文件
	* getFileDir()// 默认存储目录
	* getFileCacheDir()
	* getDir()// 创建目录
	* /data/data/packagename
	* /mnt/sdcard/Android/data

- ContentProvider
- ContentResolve
- Broadcast
- BroadcastReceiver (生命周期只有十秒，不能处理耗时操作)
- GestureDetector()// 手势识别
- GestureOverlayView() // 手势识别插件


## 六、学习资料
- [官方教程](https://developer.android.com/guide/index.html?utm_source=android-studio)
- [AndroidStudio 技巧全集](https://www.imooc.com/learn/650)
- [Android攻城狮的第一门课（入门篇）](https://www.imooc.com/learn/96)
- [Android攻城狮的第二门课（第1季）](https://www.imooc.com/learn/107)
- [Android攻城狮的第二门课（第2季）](https://www.imooc.com/learn/142)
- [Android攻城狮的第二门课（第3季）](https://www.imooc.com/learn/179)
- [一种更清晰的 Android架构 ](https://blog.csdn.net/hwz2311245/article/details/46896387)
