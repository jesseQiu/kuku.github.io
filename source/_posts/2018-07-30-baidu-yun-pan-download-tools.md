---
title: 百度网盘内容下载工具
date: 2018-07-30 14:34:39
updated: 2018-07-30 14:34:39
categories: Tools
---

360网盘关闭后，百度似乎要成为国内网盘的唯一选择，然而百度云下载速度太慢，显然是被限速了，吃相难看。下面有3个方法解决百度网盘限速的问题，演示的下载文件是大于1G的一个 [War3.zip](https://www.runningcheese.com/go/?url=https://pan.baidu.com/s/1miMaRiw) 单文件（用拖拽的方法打开，否则显示页面不存在），使用的宽带是电信20M，百度限速后的下载速度只有256KB/s，而理论上的下载速度是可以达到2M/s的。奶酪也将持续关注百度网盘限速的问题。  

## 1. 百度网盘下载助手脚本 — 2018-04-15更新【最长久】 

最早是网友“有一份田”制作的脚本百度下载助手，可以显示直链，绕过大文件云盘下载，还可以调用[迅雷](https://www.runningcheese.com/go/?url=http://dl.xunlei.com/)或者[IDM](https://www.runningcheese.com/go/?url=http://www.zdfans.com/575.html)加速下载。

![](https://ws1.sinaimg.cn/large/7a6a15d5gy1fqei3eshpyj20k00ci3yq.jpg)  
![](https://ws1.sinaimg.cn/large/7a6a15d5gy1fqei3mv5tej20k00ad74e.jpg)

**使用方法：**  
1、浏览器安装拓展 [Violentmonkey](https://www.runningcheese.com/go/?url=https://violentmonkey.github.io/get-it/) 或者 [Tampermonkey](https://www.runningcheese.com/go/?url=http://tampermonkey.net/)  
2、安装下载脚本：[https://greasyfork.org/zh-CN/scripts/39776](https://www.runningcheese.com/go/?url=https://greasyfork.org/zh-CN/scripts/39776)  
3、用迅雷或IDM下载时，点击“直接下载”，然后将链接复制到迅雷或IDM中去下载，支持在网盘管理页面和文件分享页面生效。

**注意：**  
1、IDM绿色版需要点击运行文件 "!绿化卸载.bat" 才会自动关联下载。  
2、如果你不太会怎么操作，你可以尝试一下已经集成好脚本的Firefox浏览器：https://www.runningcheese.com/v10  
（下载后需要在右上角的那个“火箭”图标里选择“系统工具”，然后再选择“设置IDM”，这样就会自动调用IDM下载了。）  
3、如果下载速度被限制在100KB左右，说明你的帐号被百度网盘拉黑了，需要退出登录才能恢复正常下载。  
4、如果下载速度在200-700KB左右，可以在“设置-->连接”中，将最达连接数设置为16或者32.  
5、在文件分享页面，直接选择文件夹会无法下载，可以同时勾选下载任意一个文件，让文件夹与文件同时下载，这时即可下载。

**另附：**  
迅雷极速版下载：[http://www.pc6.com/softview/softview_108931.html](https://www.runningcheese.com/go/?url=http://www.pc6.com/softview/softview_108931.html)  
IDM 绿色版下载：[https://pan.baidu.com/share/init?shareid](https://www.runningcheese.com/go/?url=https://pan.baidu.com/share/init?shareid=3457730696&uk=1767948507#3edk) 密码 3edk 

## 2. 度盘下载器 — 2018-05-14更新 【速度最快】

PanDownload二代，最新度盘不限速！！实测速度最高可达5MB/S，采用了Aria 2技术，下载自动切换线路，支持设置超高线程爆速下载。由原作者双霖等人开发，2.0版界面全新设计。

[![](https://ww3.sinaimg.cn/large/7a6a15d5gy1fjgvc3ziunj20nq08x41v.jpg)](https://ww3.sinaimg.cn/large/7a6a15d5gy1fjgvc3ziunj20nq08x41v.jpg)

下载地址： （v2.31最新）  
链接: [https://pan.lanzou.com/i0irygf](https://www.runningcheese.com/go/?url=https://pan.lanzou.com/i0irygf)  

## 3. 速盘 — 2018-07-10更新 【功能最强大】

由吾爱破解论坛会员“菩提叶”制作的度盘满速下载工具。这款百度网盘高速下载工具，免费小巧简单易用，采用了Aria2多线程下载，支持免登陆网盘账号解析分享链接下载，支持百度网盘网络资源搜索并下载，登陆我的网盘账户可以下载网盘文件，还能离线下载。下载地址： （v1.4.1最新）

[![](https://ws1.sinaimg.cn/large/7a6a15d5gy1fsfmoq7izgj20sg0icgmp.jpg)](https://ws1.sinaimg.cn/large/7a6a15d5gy1fsfmoq7izgj20sg0icgmp.jpg)

功能介绍：  
1、百度网盘不限速下载 （正常情况下都可以满宽带）  
2、分享链接下载 （不需要登录）  
3、分享资源搜索并下载（不需要等录）  
4、离线下载 （离线下载需要登录）  
5、其它网盘常规操作，如创建文件夹，创建分享链接，删除等（访问自己的网盘需要登录） 6、登录时调用的是百度的登录页，只记录登录cookie，不会保存用户密码等信息。  

## 4. PanDownload — 2018-03-10更新 【最早】 

52Pojie网友@[Kiryuu](https://www.runningcheese.com/go/?url=http://www.52pojie.cn/thread-579335-1-1.html) 制作作的一个C#语言下载工具，工作原理和方法5类似，可满速下载。登陆自己的账号选择文件下载即可，这对于不会使用脚本的人来说非常的方便，作者提供了一个检查更新的通道，可供持续检查更新，这样就不怕软件失效了。

下载地址：（ v1.5.4最新）  
[https://pan.lanzou.com/b173323/](https://www.runningcheese.com/go/?url=https://pan.lanzou.com/b1733233/)  
备用下载：[](https://www.runningcheese.com/baiduyun)

[![](https://ww3.sinaimg.cn/large/7a6a15d5gy1fdal9isi9mj20cm0e6q2z.jpg)](https://ww3.sinaimg.cn/large/7a6a15d5gy1fdal9isi9mj20cm0e6q2z.jpg)  

## 5. Proxyee down — 2018-07-20更新 【支持MAC】 

[Proxyee down](https://www.runningcheese.com/go/?url=https://github.com/proxyee-down-org/proxyee-down) 使用本地http代理服务器方式嗅探下载请求，支持所有操作系统和大部分主流浏览器,支持分段下载和断点下载。

![](https://ws1.sinaimg.cn/large/7a6a15d5gy1ftl7pzoy21j20l206j0sy.jpg)

下载地址：（ v2.5.4最新）  
[https://github.com/proxyee-down-org/proxyee-down/releases](https://www.runningcheese.com/go/?url=https://github.com/proxyee-down-org/proxyee-down/releases)

## 结尾

1，下载链接如若失效，或者高速下载上述软件，[公众号](https://ws1.sinaimg.cn/large/7a6a15d5gy1fr3v1xxt8ej205k05kt8p.jpg)（ID: runningcheese01）回复关键字“百度云”即可获取。  

2，最后，利用这些方法可以在百度网盘离线下载一些冷门的磁力资源并获得最高的下载速度，方法我想应该不用我说了吧？

## 参考链接
- [原文](https://www.runningcheese.com/baiduyun)

