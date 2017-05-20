---
title: 删除 node_modules
date: 2016-10-13 22:18:52
updated: 2016-10-13 22:18:52
tags: Npm
categories: Node.js
feature:
---

> 由于 `Npm` 在下载的包不是扁平化的结构，这样照成文件深度嵌套。但是在 `windows` 下，目录路径不能超过 MAX_PATH 长度，具体请查阅 [《MAX_PATH 还是 MAX_PATH + 1》](http://demon.tw/programming/max_path-or-max_path-1.html)。
MAX_PATH 只有 248 字符！10-20 个模块深度就可以超过了，大一点的模块，分分钟超越 MAX_PATH，没有最长，只有更长，这样当我们需要删除 `node_modules`时就会失败。这时有个简单方法，就是通过下载 `ramraf` 插件来删除。

### 步骤：
1. npm install -g rimraf

2. rimraf node_modules

### 参考：
- [rimraf](https://www.npmjs.com/package/rimraf)
