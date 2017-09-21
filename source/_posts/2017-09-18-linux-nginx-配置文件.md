---
title: Linux Nginx 配置文件
date: 2017-09-19 10:16:29
categories: Linux
---

## 一、默认的配置文件
### 1. 全局配置文件
```bash
# 这个需要修改为启动 nginx 用户名，建议使用 root
user  nginx; 
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    # 这里会包含默认配置文件，也就是下面的文件
    include /etc/nginx/conf.d/*.conf; 
}

```
### 2. 网站默认配置文件
```bash
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    # proxy the PHP scripts to Apache listening on 127.0.0.1:80
    #
    #location ~ \.php$ {
    #    proxy_pass   http://127.0.0.1;
    #}

    # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
    #
    #location ~ \.php$ {
    #    root           html;
    #    fastcgi_pass   127.0.0.1:9000;
    #    fastcgi_index  index.php;
    #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    #    include        fastcgi_params;
    #}

    # deny access to .htaccess files, if Apache's document root
    # concurs with nginx's one
    #
    #location ~ /\.ht {
    #    deny  all;
    #}
}
```
上面显示的，只配置了一个虚拟服务器，如果需要配置多个服务器，可以添加相应的结构。下面是对虚拟服务器配置几点说明:
- 配置多个 server 时，linten 端口和 server_name 不能完全相同，必须有其中一项不同。
- 如果你购买的域名配置了服务器 ip，相同的 server 配置中，server_name 为域名的优先级高于 localhost。
```bash
server_name=www.jessechiu.com > server_name=localhost
```
- 一个 server 中只能配置一个更路径('/')端口转发
```bash
location / {
    root   /usr/share/nginx/html;
    index  index.html index.htm;
}
或者
location / {
    proxy_pass http://127.0.0.1:6066;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
```
> 如果需要多个端口转发可以通过配置多个 server 信息，注意其中的 server_name 配置不能相同(nodejs 服务器是这样，其它的没有验证)。


## 二、nginx location 匹配规则

### location 匹配命令
- ~      #波浪线表示执行一个正则匹配，区分大小写  
- ~*     #表示执行一个正则匹配，不区分大小写  
- ^~     #^~表示普通字符匹配，如果该选项匹配，只匹配该选项，不匹配别的选项，一般用来匹配目录  
- =      #进行普通字符精确匹配  
- @      #"@" 定义一个命名的 location，使用在内部定向时，例如 error_page, try_files

### location 匹配的优先级(与location在配置文件中的顺序无关)
- = 精确匹配会第一个被处理。如果发现精确匹配，nginx停止搜索其他匹配。  
- 普通字符匹配，正则表达式规则和长的块规则将被优先和查询匹配，也就是说如果该项匹配还需去看有没有正则表达式匹配和更长的匹配。  
- ^~ 则只匹配该规则，nginx停止搜索其他匹配，否则nginx会继续处理其他location指令。  
- 最后匹配理带有"~"和"~*"的指令，如果找到相应的匹配，则nginx停止搜索其他匹配；当没有正则表达式或者没有正则表达式被匹配的情况下，那么匹配程度最高的逐字匹配指令会被使用。

### location 优先级官方文档
>1.  Directives with the = prefix that match the query exactly. If found, searching stops.
      =前缀的指令严格匹配这个查询。如果找到，停止搜索。

>2.  All remaining directives with conventional strings, longest match first. If this match used the >^~ prefix, searching stops.
    所有剩下的常规字符串，最长的匹配。如果这个匹配使用^〜前缀，搜索停止。

>3.  Regular expressions, in order of definition in the configuration file.
     正则表达式，在配置文件中定义的顺序。

>4.  If #3 yielded a match, that result is used. Else the match from #2 is used.
     如果第3条规则产生匹配的话，结果被使用。否则，使用第2条规则的结果。

例如:
```bash
location  = / {
  # 只匹配"/".
  [ configuration A ] 
}
location  / {
  # 匹配任何请求，因为所有请求都是以"/"开始
  # 但是更长字符匹配或者正则表达式匹配会优先匹配
  [ configuration B ] 
}
location ^~ /images/ {
  # 匹配任何以 /images/ 开始的请求，并停止匹配 其它location
  [ configuration C ] 
}
location ~* .(gif|jpg|jpeg)$ {
  # 匹配以 gif, jpg, or jpeg结尾的请求. 
  # 但是所有 /images/ 目录的请求将由 [Configuration C]处理.   
  [ configuration D ] 
}
```
请求URI例子:
```bash
* / -> 符合configuration A
* /documents/document.html -> 符合configuration B
* /images/1.gif -> 符合configuration C
* /documents/1.jpg ->符合 configuration D

@location 例子  
error_page 404 = @fetch;

location @fetch(  
  proxy_pass http://fetch;  
)
```

## 三、配置文件检查
修改完配置后，最好使用命令检查下配置是否正确，这样可以排除配置格式错误导致配置不生效，影响服务器的稳定运行
```bash
nginx -t

// 有错误情况
[root@jessechiu conf.d]# nginx -t
nginx: [emerg] unexpected "}" in /etc/nginx/conf.d/kuku.conf:16
nginx: configuration file /etc/nginx/nginx.conf test failed

// 正常情况
[root@jessechiu conf.d]# nginx -t
nginx: [warn] conflicting server name "localhost" on 0.0.0.0:80, ignored
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
## 四、配置生效
修改配置后需要重启 Nginx 才能生效，如果关闭 Nginx 再打开会影响服务运行，我们可以向 Nginx 发送信号，平滑重启。
```bash
kill -HUP 进程号
```

其中进程号是 Nginx 的进程号，可以使用 `ps aux | grep nginx` 命令查看。
或者使用下面的命令使配置生效
```bash
nginx -s reload
```

## 五、错误排查
### 1. 403 forbidden
如果 config 配置信息都没有错误，出现 403 forbidden 错误，可能是由于由于启动用户和nginx工作用户不一致所致。
只要将 `/etc/nginx/conf.d/nginx.config` 的 user 改为和启动用户一致.
![](http://img.blog.csdn.net/20170718094235060?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvb25seXN1bm55Ym95/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
> 这里我设置的是 root，因为 nginx 安装是必须使用 root 权限。

## 参考
- [官网](https://www.nginx.com/resources/admin-guide/reverse-proxy/)
- [配置文件详解](http://www.cnblogs.com/hunttown/p/5759959.html)
- [nginx location 匹配规则](http://www.nginx.cn/115.html)
- [nginx 配置入门](http://www.nginx.cn/591.html)
- [解决 Nginx 出现 403 forbidden (13: Permission denied)报错的四种方法](http://blog.csdn.net/onlysunnyboy/article/details/75270533)





