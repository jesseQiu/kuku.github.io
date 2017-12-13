---
title: Linux 部署 Nodejs
date: 2017-05-21 10:16:29
categories: Linux
---

本文主要是对 nodejs 在 linux 系统部署备忘

## 部署步骤

### 1. 查看 linux 系统版本信息
这里使用 `uname` 命令,用于打印当前系统相关信息（内核版本号、硬件架构、主机名称和操作系统类型等）
```bash
uname -a
Linux Jesse 3.10.0-514.2.2.el7.x86_64 #1 SMP Tue Dec 6 23:06:41 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux
```

### 2. 下载 nodejs 软件
从上一步中查看到的系统信息，再去[nodejs](https://nodejs.org/en/download/)下载对应的版本
![nodjs版本](http://od6sd4xau.bkt.clouddn.com/linux-nodejs.png)
根据上面的系统版本信息，我选择的是 binarias 版本(可以直接运行的二进制版本)，软件包名为 `node-v6.10.3-linux-x64.tar.xz`

### 3. 通过 WinSCP 软件上传到 linux 服务

### 4. 解压软件包
这边我把软件放在 `/usr/local/` 目录下
注意，这里的 nodejs 软件包是以 `xz` 为后缀，而不是 `*.gz,*.bz2` 为后缀名。所有需要用 xz 工具来解压。
```bash
// 先将 node-v6.10.3-linux-x64.tar.xz 解压成 node-v6.10.3-linux-x64.tar
xz -d node-v6.10.3-linux-x64.tar.xz  
// 再将 tar 解包
tar xvf node-v6.10.3-linux-x64.tar
mv node-v6.10.3-linux-x64 /usr/local/node-v6.10.3-linux-x64
```
ps 因为 xz 工具不能像 gzip 和 bzip2 那要可以通过 `j/zxvf` 一条命令解压解包，所以要分两步.
如果 linux 没有安装 xz 工具可以通过 yum 来安装
```bash
yum install xz
```
如果还是不行，可以[参考](http://blog.sina.com.cn/s/blog_ba08e8e00101b1rs.html)

### 5. 测试解压的 nodejs 软件是否可用
```bash
cd /usr/local/node-v6.10.3-linux-x64/bin/
./node -v
// v6.10.3 如果正常显示 nodejs 版本号，说明软件安装正常
```

### 6. 添加 node 到环境变量中
```bash
exoprt PATH=$PATH:/usr/local/node-v6.10.3-linux-x64/bin/
env | grep PATH
// 如果可以看到刚添加的环境变量说明添加成功
node -v
// v6.10.3 
```
如果需要永久有效，需要在 `/ect/profile` 文件中添加
```bash
vi /etc/profile
// 在文件的尾部添加：PATH=$PATH:/usr/local/node-v6.10.3-linux-x64/bin/
source /etc/profile // 可以让刚设置马上生效
```

### 7. 升级 nodejs
```bash
// 安装 node 更新模块
npm i -g n

// 安装最新稳定版本
n stable

//[root@fpxt01yht code]# n stable
//install : node-v9.2.0
//mkdir : /usr/local/n/versions/node/9.2.0  // 最新的 nodejs 存放位置
//fetch : https://nodejs.org/dist/v9.2.0/node-v9.2.0-linux-x64.tar.gz
######################################################################## 100.0%
//installed : v9.2.0
```
如果使用 `node -v` 命令显示的还是之前版本，可能是又要之前安装的时候，配置了环境变量指向，只要把相应的环境变量注释掉即可，因为 n 模块以及安装了全局最新版本。
**注意:如果之前版本的 node 有安装全局的插件，需要重新安装**

### 8. 安装 PM2 插件
```bash
$ npm i pm2 -g
```

### 9. 安装 git 环境
#### 1. git 安装
在 Linux 上安装预编译好的 Git 二进制安装包，可以直接用系统提供的包管理工具。在 Fedora 上用 yum 安装：
```bash
$ yum install git-core
$ git --version
// git version 1.8.3.1
```
#### 2. 生成 rsa 密钥
首先查看 `~/.ssh` 目录，看是否已经存在 `id_ras` 和 `id_rsa.pub` 两个文件，如果已经有了就不用生成，直接使用。如果没有使用下面的命令生成
```bash
ssh-keygen -t rsa -C "your.email@example.com" -b 4096
```

#### 3. 拷贝生成的公钥内容到 `github` 或者 `gitlab` 上面
```bash
cat ~/.ssh/id_rsa.pub
```
选中内容 -> 回车 -> 在 git 仓库创建 ssh keys -> 粘贴内容 -> 保存
[参考 ssh keys generate it](https://gitlab.com/help/ssh/README)

#### 4. 克隆仓库工程
```bash
git clone git@gitlab.com:JesseChiu/jesse-xxx.git
cd jesse-xxx
// 运行命令
```

#### 5. 同步最新代码
```bash
git pull
```

### 10. 运行
```bash
npm run xxx
```
**注意:如果使用运行 80 端口必须要 root 权限用户。否则会报 `Port 80 requires elevated privileges`**

## 参考
- [nodejs 下载地址](https://nodejs.org/en/download/)
- [linux 部署 nodejs](http://www.cnblogs.com/dubaokun/p/3558848.html)


