---
title: PhantomJs
date: 2016-09-28 16:29:12
updated: 2016-09-28 16:29:12
tags: PhantomJs
categories: Testing
feature:
---

> PhantomJS 是一个基于 webkit 的 JavaScript API。它使用 QtWebKit 作为它核心浏览器的功能，使用 webkit 来编译解释执行 JavaScript 代码,不提供图形界面。任何你可以在基于 webkit 浏览器做的事情，它都能做到。它不仅是个隐形的浏览器，提供了诸如 CSS 选择器、支持 Web 标准、DOM 操作、JSON、HTML5、Canvas、SVG 等，同时也提供了处理文件 I/O 的操作，从而使你可以向操作系统读写文件等。PhantomJS 的用处可谓非常广泛，诸如前端无界面自动化测试（需要结合Jasmin）、网络监测、网页截屏等。



### 一、PhantomJS 下载与安装

1. 通过网页下载 
	[官方下载地址](http://phantomjs.org/download.html) 目前官方支持三种操作系统，包括 windows/Mac OS/Linux 这三大主流的环境。你可以根据你的运行环境选择要下载的包，下面以 Windows 为例。下载完成后解压文件，建议为方便使用，单独放在一个文件夹里，如我放在 D:/workspace/phantomjs里。到这里，你已经成功下载安装好 PhantomJS 了。那么，打开D:/workspace/phantomjs/bin 文件夹，双击运行 phantomjs.exe，出现如下界面，那么你就可以运行 JS 代码了。如果需要全局运行，可以将 phantomjs.exe 添加到环境变量里。具体如下：**打开我的电脑->右键属性->高级系统设置->高级标签->环境变量**，在系统变量里找到 Path,将你的 phantomjs 添加到环境变量里。比方说我的路径添加的为 `“;D:/workspace/phantomjs/bin”`，切记不要少了前面那个**分号**。

2. 通过 Npm 下载
	```bash
		$ npm install phantomjs -g
	```
	使用下面的命令，查看是否安装成功。

	```bash
		$ phantomjs --version
	```
3. 具体的用法可以参考 [阮一峰的教程](http://javascript.ruanyifeng.com/tool/phantomjs.html)，我这边就不做信息的搬运工了。

### 参考
- [PhantomJS快速入门](http://www.th7.cn/web/js/201503/89432.shtml)
- [PhantomJS官方地址](http://phantomjs.org/)
- [PhantomJS官方API](http://phantomjs.org/api/)
- [PhantomJS官方示例](http://phantomjs.org/examples/)
- [PhantomJS GitHub](https://github.com/ariya/phantomjs/)