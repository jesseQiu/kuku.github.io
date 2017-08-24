---
title: Sever-Sent Event
date: 2017-06-29 14:34:39
updated: 2017-06-29 14:34:39
categories: Node.js
---

## 简介

服务器向浏览器推送信息，除了 WebSocket，还有一种方法：Server-Sent Events（以下简称 SSE）。
严格地说，HTTP 协议无法做到服务器主动推送信息。但是，有一种变通方法，就是服务器向客户端声明，接下来要发送的是流信息（streaming）。
也就是说，发送的不是一次性的数据包，而是一个数据流，会连续不断地发送过来。这时，客户端不会关闭连接，会一直等着服务器发过来的新的数据流，视频播放就是这样的例子。本质上，这种通信就是以流信息的方式，完成一次用时很长的下载。
SSE 就是利用这种机制，使用流信息向浏览器推送信息。它基于 HTTP 协议，目前除了 IE/Edge，其他浏览器都支持

## 测试代码
### 客户端：
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
</head>
<body>
<div id="example"></div>
<script>
  var source = new EventSource('http://127.0.0.1:8844/stream');
  var div = document.getElementById('example');
  
  source.onopen = function (event) {
    div.innerHTML += '<p>Connection open ...</p>';
  };
  
  source.onerror = function (event) {
    div.innerHTML += '<p>Connection close.</p>';
  };
  
  source.addEventListener('connecttime', function (event) {
    div.innerHTML += ('<p>Start time: ' + event.data + '</p>');
  }, false);
  
  source.onmessage = function (event) {
    div.innerHTML += ('<p>Ping: ' + event.data + '</p>');
  };
  
</script>
</body>
</html>
```
### 服务器端：
```js
var http = require("http");
http.createServer(function (req, res) {
  var fileName = "." + req.url;

  if (fileName === "./stream") {
    res.writeHead(200, {
      "Content-Type":"text/event-stream",
      "Cache-Control":"no-cache",
      "Connection":"keep-alive",
      "Access-Control-Allow-Origin": '*',
    });
    res.write("retry: 10000\n");
    res.write("event: connecttime\n");
    res.write("data: " + (new Date()) + "\n\n");
    res.write("data: " + (new Date()) + "\n\n");

    interval = setInterval(function () {
      res.write("data: " + (new Date()) + "\n\n");
    }, 1000);

    req.connection.addListener("close", function () {
      clearInterval(interval);
    }, false);
  }
}).listen(8844, "127.0.0.1");
```

## 参考
- [Server-Sent Events 教程](http://www.ruanyifeng.com/blog/2017/05/server-sent_events.html)