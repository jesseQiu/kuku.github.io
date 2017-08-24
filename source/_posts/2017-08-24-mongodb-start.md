---
title: MongoDB Start
date: 2017-08-24 10:24:54
updated: 2017-08-24 10:24:54
categories: MongoDB
---

## MongoDB Windows 环境安装

1. 首先到[官网](http://www.mongodb.org/downloads )下载合适的安装包, 根据操作系统类型选择相应的版本,只要下载下来解压即可，不需要安装。

2. 创建数据库并启动服务
- 在解压后的 bin 文件夹下打开 `cmd` 运行下面的命令
```bash
mongod --dbpath D:\MongoDB\data
```
上面的 `--dbpath` 参数是指定数据库存放目录
> 如果运行这步出现 "msvcp120.dll 错误"，则需要下载相应的库放到 系统目录下。这个主要是 vs2013 编译的软件运行时需要相应的运行库，在没有装 vs2013 的机子上会报这个错误。具体解决方法[参考](http://www.cr173.com/soft/103918.html)

3. 连接服务器
这里已 Node.js 代码为例
```js

```

4. 命令行查看数据库
```bash
cd x:\xx\mongodb\bin\
mongo
use blog // blog 是数据库 名称
db.users.find() //users 是集合数据库中的 集合 名称。
```

5. 使用图形工具查看
使用 [Robomongo](https://robomongo.org/) 工具查看，是一个基于 Shell 的跨平台开源 MongoDB 的视图管理工具.

## MongoDB Linux 环境安装

## 参考
- [MongoDB Windows 环境安装及配置](http://www.cnblogs.com/lzrabbit/p/3682510.html)