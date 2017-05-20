---
title: JSON server
date: 2017-03-23 18:49:25
tags: Library
categories: JavaScript
---

当我们在开发有前后端通信项目时，常常需要后台数据来调试我们的前端界面。如果后端还未完成，是否有办法快速搭建一个服务器帮助我们 `mock` 返回后台数据呢？那就赶紧往下看吧！

## 一、JSON server
JSON server 就是用来快速搭建一个后台数据服务器，它还提供了完整的 REST API，下面是 github 上的介绍
> Get a full fake REST API with zero coding in less than 30 seconds (seriously)

## 二、快速开始
### 1. 下载安装
`npm install -g json-server`

### 2. 运行自带的 `demo` 数据库服务器

`json-server db.json`
也可以运行自定义的数据库
`json-server your-db.json`

这时可以通过浏览器打开 `http://localhost:3000` 查看
```js
{
  "posts": [
    {
      "id": 1,
      "title": "json-server",
      "author": "typicode"
    }
  ],
  "comments": [
    {
      "id": 1,
      "body": "some comment",
      "postId": 1
    }
  ],
  "profile": {
    "name": "typicode"
  }
}
```
上面是已经存在的数据

### 3. REST API 测试
下面的是代码我们使用 `axios` 库来测试对应的 HTTP 请求，如果不知道 `axios` 是啥，可以参看下官网 [axios](https://github.com/mzabriskie/axios);
#### 3.1 GET
```js
axios.get('http://localhost:3000/comments/1')
.then((result)=>{
	console.log('get->success: ' + JSON.stringify(result.data));
	resultEle.value = JSON.stringify(result.data);
})
.catch((err)=>{
	console.error('get->error: ' + JSON.stringify(err));
});

// {"id":1,"body":"some comment","postId":1}
```
上面请求的路径是 `comments/1` 对应到上面的数据库中 `comments` 属性中 `id=1` 的值。

#### 3.2 POST
```js
axios.post('http://localhost:3000/comments',{
	body: "json server is very great",
	postId: 2
})
.then((result)=>{
	console.log('post->success: ' + JSON.stringify(result.data));
	resultEle.value = JSON.stringify(result.data);
})
.catch((err)=>{
	console.error('post->error: ' + JSON.stringify(err));
});
```
// 注意上面的 `url` 和 `get` 的区别, `post` 中只有到 `comments` 这一级

#### 3.3 put(修改)
```js
axios.put('http://localhost:3000/comments',{
	body: "json server is very great",
	postId: 2
})
.then((result)=>{
	console.log('put->success: ' + JSON.stringify(result.data));
	resultEle.value = JSON.stringify(result.data);
})
.catch((err)=>{
	console.error('put->error: ' + JSON.stringify(err));
});
```
同 `post` 基本一样

#### 3.4 delete
```js
axios.delete('http://localhost:3000/comments/2')
.then((result)=>{
	console.log('delete->success: ' + JSON.stringify(result.statusText));
	resultEle.value = JSON.stringify(result.statusText);
})
.catch((err)=>{
	console.error('delete->error: ' + JSON.stringify(err));
});
```
路径同 `get` 的一样，具体到要删除的某个值

在上面的每次操作完刷新下 `http://localhost::3000/db` 页面，可以看到每次 HTTP 请求操作结果。

#### 3.5 完整测试代码
```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>axios demo</title>
</head>
<body>
<div>
	<h3>结果显示</h3>
	<textarea id="result" rows="10" cols="50" readonly></textarea> <br>
<!-- 	<h3>输入数据</h3>
	<textarea id="input" rows="5" cols="50"></textarea> -->
</div>
<div>
	<button id="get">get</button>
	<button id="post">post</button>
	<button id="put">put</button>
	<button id="delete">delete</button>
</div>
<script type="text/javascript">
	window.onload = function(){
		var resultEle = document.getElementById('result');
		var inputEle = document.getElementById('input');
		var getEle = document.getElementById('get');
		var postEle = document.getElementById('post');
		var putEle = document.getElementById('put');
		var deleteEle = document.getElementById('delete');

		// get
		getEle.addEventListener('click',()=>{
			axios.get('http://localhost:3000/comments/1')
			.then((result)=>{
				console.log('get->success: ' + JSON.stringify(result.data));
				resultEle.value = JSON.stringify(result.data);
			})
			.catch((err)=>{
				console.error('get->error: ' + JSON.stringify(err));
			});
		})

		// post
		postEle.addEventListener('click',()=>{
			axios.post('http://localhost:3000/comments',{
				body: "json server is very great",
      	postId: 2
			})
			.then((result)=>{
				console.log('post->success: ' + JSON.stringify(result.data));
				resultEle.value = JSON.stringify(result.data);
			})
			.catch((err)=>{
				console.error('post->error: ' + JSON.stringify(err));
			});
		})

		// put
		putEle.addEventListener('click',()=>{
			axios.put('http://localhost:3000/comments/2',{
				body: "axios is very useful",
      	postId: 2
			})
			.then((result)=>{
				console.log('put->success: ' + JSON.stringify(result.data));
				resultEle.value = JSON.stringify(result.data);
			})
			.catch((err)=>{
				console.error('put->error: ' + JSON.stringify(err));
			});
		})

		// delete
		deleteEle.addEventListener('click',()=>{
			axios.delete('http://localhost:3000/comments/2')
			.then((result)=>{
				console.log('delete->success: ' + JSON.stringify(result.statusText));
				resultEle.value = JSON.stringify(result.statusText);
			})
			.catch((err)=>{
				console.error('delete->error: ' + JSON.stringify(err));
			});
		})
	}
</script>

<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
</body>
</html>
```

## 参考
- [json-server](https://github.com/typicode/json-server)
