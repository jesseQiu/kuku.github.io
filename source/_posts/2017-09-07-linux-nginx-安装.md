---
title: Linux Nginx 安装
date: 2017-09-07 10:16:29
categories: Linux
---

## 一、关于 Nginx

Nginx (“engine x”) 是一个高性能的 HTTP 和 反向代理 服务器，也是一个 IMAP/POP3/SMTP 代理服务器。 Nginx 是由 Igor Sysoev 为俄罗斯访问量第二的 Rambler.ru 站点开发的，第一个公开版本0.1.0发布于2004年10月4日。其将源代码以类 BSD 许可证的形式发布，因它的稳定性、丰富的功能集、示例配置文件和低系统资源的消耗而闻名。

## 二、安装步骤

> 安装过程需要 root 用户权限。

### 1.添加 Nginx 到 YUM 源

添加 CentOS 7 Nginx yum 资源库,打开终端,使用以下命令:

```bash
rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```
> 可以直接访问 [nginx.packages](http://nginx.org/packages/) 查看其它系统资源路径。

### 2.安装 Nginx

在你的 CentOS 7 服务器中使用 yum 命令从 Nginx 源服务器中获取来安装 Nginx：
```bash
yum install -y nginx
```
稍等片刻 Nginx 将完成安装在你的 CentOS 7 服务器中。

### 3.启动 Nginx

刚安装的 Nginx 不会自行启动。运行 Nginx:
```bash
systemctl start nginx.service
```
如果一切进展顺利的话，现在你可以通过你的域名或 IP 来访问你的Web页面来预览一下 Nginx 的默认页面；

![nginx_default](http://images.statics.9696e.com/wp-content/uploads/2014/11/nginx_default.png)

如果看到这个页面,那么说明你的 CentOS 7 中 web 服务器已经正确安装。

### 4. CentOS 7 开机启动 Nginx
```bash
systemctl enable nginx.service
```

## 三、Nginx 配置信息

### 1. 网站文件存放默认目录
```bash
/usr/share/nginx/html
```

### 2. 网站默认站点配置
```bash
/etc/nginx/conf.d/default.conf
```

### 3. 自定义 Nginx 站点配置文件存放目录
```bash
/etc/nginx/conf.d/
```

### 4. Nginx全局配置
```bash
/etc/nginx/nginx.conf
```
在这里你可以改变设置用户运行 Nginx 守护程序进程一样,和工作进程的数量得到了 Nginx 正在运行,等等。

## 四、Linux 查看公网IP

您可以运行以下命令来显示你的服务器的公共IP地址:
```bash
ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//'
```

## 参考
- [Nginx 实战基础篇一 源码包编译安装部署 web 服务器](http://dreamfire.blog.51cto.com/418026/1140965)
- [CentOS 7 YUM 安装 Nginx](http://blog.csdn.net/ysydao/article/details/51388385)
- [Linux(Centos)之安装Nginx及注意事项](http://www.cnblogs.com/hanyinglong/p/5102141.html)
- [sudo 命令](http://man.linuxde.net/sudo)
- [systemctl 命令](http://man.linuxde.net/systemctl)