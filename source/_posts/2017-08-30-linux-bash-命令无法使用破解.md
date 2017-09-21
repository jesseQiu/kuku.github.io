---
title: Linux bash 命令无法使用破解
date: 2017-08-30 10:16:29
categories: Linux
---

### 问题描述

在一次部署时，更新了 `/ect/profile` 文件后，发现 `ls`和其它的基本命令都不能使用了,提示

```bash
[root@fapiao ~]# ls
bash: ls: 未找到命令...
相似命令是： 'lz'
``` 

错误信息。但在其它还未关闭的ssh终端中可以使用，推测是 `/etc/profile` 文件的问题,查看 `$PATH` 后发现不对；切换 `root` 权限准备修改 `profile` 文件后，发现 `vi` 命令不能用。

### 解决方法

在 ssh 终端中执行下面语句，可以让此会话终端中环境变量起作用
```bash
export PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin
```
这时已经可以使用 bash 命令了，再通过修改 `/etc/profile` 文件，重新 source 后系统恢复正常。

### 错误原因
在修改 profile 文件时，使用了 `$PATH='xxx/xx'` 后面没有使用 `:` 拼接原来的 `$PATH`,导致 PATH 丢失了重要环境变量.
> 如果是错误的字符也会导致环境变量丢失，例如
```bash
PATH=$PATH:$JAVA_HOME/bin
PATH=$PATH:/usr/local/node-v6.10.3-linux-x64/bin/
PATH=#PATH:/usr/local/ngrok/      
export PATH JAVA_HOME CLASSPATH
```
上面的第 3 行， `PATH=` 后面的是 `#` 而不是 `$` 符号，在 `source /etc/profile` 后导致环境变量丢失

### 参考
- [Linux 基本命令不能用的解决方法](https://zhidao.baidu.com/question/41683527.html)