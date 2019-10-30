---
title: Android Studio 技巧全集
date: 2018-04-05 09:49:25
categories: Android
---

>本文主要是记录 Android Studio 使用技巧，提高我们开发应用效率

<img class="nofancybox" src="http://images.jessechiu.com/android-studio.png">

## 一、打印日志
- 定义日志 TAG 变量
`logt` + 回车
```java
private static final String TAG = "MainActivity";
```

- 快速打印日志
`logd` + 回车
```java
 Log.d(TAG, "onCreate: is done");
```

- 打印函数参数
`logm` + 回车
```java
Log.d(TAG, "onCreate() called with: savedInstanceState = [" + savedInstanceState + "]");
```

## 二、代码编辑
- 代码提示
`ctrl + alt + 空格`

- 代码移动
`ctrl + shift + up/down`

- 复制当前行代码到下一行
`ctrl + d`

- 删除当前行代码
`ctrl + y`

- 类中的方法间移动
`alt + up/down`

- 选中代码,连续按选中多行
`ctrl + w`


## 三、代码查看相关

- 打开一个类
`ctrl + n`

- 打开一个文件
`ctrl + shift + n`

- 查找类中的方法或变量
`ctrl + shift + alt + n`

- 查看变量声明
`ctrl + b`

- 查看父类
`ctrl + u`

- 查看一个方法在那些地方调用
`ctrl + alt + h`

- 查看一个类中的方法(弹框显示)
`ctrl + alt + i`

- 代码返回快捷键
`ctrl + alt + left/right`

- 窗口返回快捷键
`ctrl + left/right`

- 代码折叠/展开
`ctrl + .`

- 重写父类
`ctrl + o`

- 大括号间跳转
`ctrl + [ 或 ]`

- 为选中代码快速添加 `if for try/catch 等语句`
`ctrl + alt + t`


## 四、代码生成(`if foreach toast...`)
- `ctrl + j`


## 五、代码查找格式化

- 代码替换
`ctrl + r`

- 代码查找
`ctrl + f`

- 代码格式化
`ctrl +alt + l`


## 参考
- [与Android Studio的第一次亲密接触](https://www.imooc.com/learn/206)
- [AndroidStudio技巧全集](https://www.imooc.com/learn/650)
- [android studio 中文社区](http://www.android-studio.org/)
