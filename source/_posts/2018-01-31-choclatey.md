---
title: Choclatey
date: 2018-01-31 14:34:39
updated: 2018-01-31 14:34:39
categories: Tools
---

>Chocolatey 是什么？很简单，Chocolatey 就是 Windows 系统的 yum 或 apt-get

## 一、Chocolatey 介绍

Chocolatey 是一款专为 Windows 系统开发的、基于 NuGet 的包管理器工具，类似于 Node.js 的 npm，MacOS 的 brew，Ubuntu 的 apt-get，它简称为 choco。Chocolatey 的设计目标是成为一个去中心化的框架，便于开发者按需快速安装应用程序和工具。

## 二、Chocolatey安装

### 1. `cmd.exe` 方式
以管理员权限打开 cmd.exe 命令行提示，执行如下内容：
```bash
@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
```

### 2. `PowerShell` 方式
使用 PowerShell，同样必须以管理员权限打开 PowerShell，执行如下命令：
```bash
iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))
```

## 三、Chocolatey 用法

### 1、命令
- search - 搜索包 choco search something
- list - 列出包 choco list -lo
- install - 安装 choco install baretail
- pin - 固定包的版本，防止包被升级 choco pin windirstat
- upgrade - 安装包的升级 choco upgrade baretail
- uninstall - 安装包的卸载 choco uninstall baretail
- 安装 Ruby Gem - choco install compass -source ruby
- 安装 Python Egg - choco install sphynx -source python
- 安装 IIS 服务器特性 - choco install IIS -source windowsfeatures
- 安装 Webpi 特性 - choco install IIS7.5Express -source webpi

### 2、常用的一些命令

- 列出本地已安装的包
```bash
choco list --local-only
```

- 列出 Windows 系统已安装的软件
```bash
choco list -li
// 或使用
choco list -lai
```

- 升级所有已安装的包
```bash
choco upgrade all -y
```

## 参考:
- [Chocolatey](https://chocolatey.org/)
- [Windows上的巧克力味Chocolatey详解](http://blog.csdn.net/chszs/article/details/51782334)
