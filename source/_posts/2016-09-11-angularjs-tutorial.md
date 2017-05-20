---
title: Angularjs Tutorial
date: 2016-09-11 21:49:06
updated: 2016-09-11 21:49:06
tags:
categories: AngularJS
---

### 本文主要是记录 `AngularJs 1.x` 入门教程
1. 从 [angular-phonecat git 仓库](https://github.com/angular/angular-phonecat) 下载源码。
	- 使用 `git clone --depth=16 https://github.com/angular/angular-phonecat.git` 命令下载会把最新的 16 次合并的版本信息也一起 clone 下来。
	- 注意: 如果是从 git 仓库下载源码，则要注意，如果是下载 `master` 的代码泽没有 `git` 的版本信息。

2. 运行 `Phonecat` 技巧
	- 先打开一个 `cmd-1` ，`checkout` 最后一步，例如 `git checkout -f step-14`，运行应用 `npm start`。
	- 再打开一个 `cmd-2`，用于在学习过程中切换步骤，如现在学习到第 5 步，在这个命令行窗口输入 `git checkout -f step-5`，这时就更新到了第 5 步骤的代码。
	- 这样做相对于只打开一个 `cmd` 窗口，每次切换步骤都要先终止运行 -> 切换到下一步 -> 运行应用,这时应用就需要重新 `npm install` 过，效率低。

### 参考:
- 官方 [angular-phonecat](https://docs.angularjs.org/tutorial) 入门教程
- 官方 [git 仓库地址](https://github.com/angular/angular-phonecat) 
- [中文教程](http://www.apjs.net/#dir2)
- [中文论坛](http://www.angularjs.cn/)
- [angularjs 1.x 基础教程代码](https://github.com/Jesse-Chiu/angularjs-1.x-course)
- [AnguarJs 2.0](https://angular.io/)
- 使用 AngualrJs 框架开发的网站
	- [AngularJs 中文社区](http://angularjs.cn/)
	- [Worktile 让工作更简单](https://worktile.com/)
	- [锤子官网](http://www.smartisan.com/#/shop)
	- [知乎周刊](https://zhuanlan.zhihu.com/Weekly)
	- [ionic](http://ionicframework.com/getting-started/)
