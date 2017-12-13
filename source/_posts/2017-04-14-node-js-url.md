---
title: url 模块
date: 2017-04-14 14:34:39
updated: 2017-04-14 14:34:39
categories: Node.js
---

`url`模块用于生成和解析URL。该模块使用前，必须加载。
```js
┌─────────────────────────────────────────────────────────────────────────────────────────────┐
│                                            href                                             │
├──────────┬──┬─────────────────────┬─────────────────────┬───────────────────────────┬───────┤
│ protocol │  │        auth         │        host         │           path            │ hash  │
│          │  │                     ├──────────────┬──────┼──────────┬────────────────┤       │
│          │  │                     │   hostname   │ port │ pathname │     search     │       │
│          │  │                     │              │      │          ├─┬──────────────┤       │
│          │  │                     │              │      │          │ │    query     │       │
"  https:   //    user   :   pass   @ sub.host.com : 8080   /p/a/t/h  ?  query=string   #hash "
│          │  │          │          │   hostname   │ port │          │                │       │
│          │  │          │          ├──────────────┴──────┤          │                │       │
│ protocol │  │ username │ password │        host         │          │                │       │
├──────────┴──┼──────────┴──────────┼─────────────────────┤          │                │       │
│   origin    │                     │       origin        │ pathname │     search     │ hash  │
├─────────────┴─────────────────────┴─────────────────────┴──────────┴────────────────┴───────┤
│                                            href                                             │
└─────────────────────────────────────────────────────────────────────────────────────────────┘
```

```javascript
var url = require('url');
```

## url.parse()
`url.parse()` 将一个 URL 字符串转换为URL对象
```js
url.parse('http://user:pass@host.com:8080/p/a/t/h?query=string#hash');
{ protocol: 'http:',
  auth: 'user:pass',
  host: 'host.com:8080',
  port: '8080',
  hostname: 'host.com',
  hash: '#hash',
  search: '?query=string',
  query: 'query=string',
  pathname: '/p/a/t/h',
  path: '/p/a/t/h?query=string',
  href: 'http://user:pass@host.com:8080/p/a/t/h?query=string#hash' }
```

## url.format()
`url.format()` 方法允许将一个 URL 对象转换为 URL 字符串
```js
url.format({
     protocol: 'http:',
     host: 'www.example.com',
     pathname: '/p/a/t/h',
     search: 'query=string'
 });
'http://www.example.com/p/a/t/h?query=string'
```

## url.resolve(from, to)

`url.resolve`方法用于生成URL。它的第一个参数是基准URL，其余参数依次根据基准URL，生成对应的位置。

```javascript
url.resolve('/one/two/three', 'four')
// '/one/two/four'

url.resolve('http://example.com/', '/one')
// 'http://example.com/one'

url.resolve('http://example.com/one/', 'two')
// 'http://example.com/one/two'

url.resolve('http://example.com/one', '/two')
// 'http://example.com/two'
```
