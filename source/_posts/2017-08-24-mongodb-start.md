---
title: MongoDB Start
date: 2017-08-24 10:24:54
updated: 2017-08-24 10:24:54
tags: MongoDB
categories: Database
---

## 一、MongoDB Windows 环境安装

### 1. 首先到[官网](http://www.mongodb.org/downloads )下载合适的安装包, 根据操作系统类型选择相应的版本,只要下载下来解压即可，不需要安装。

### 2. 使用命令行指定参数运行
- 在解压后的 bin 文件夹下打开 `cmd` 运行下面的命令
```bash
mongod --dbpath D:\MongoDB\data
```
上面的 `--dbpath` 参数是指定数据库存放目录
> 如果运行这步出现 "msvcp120.dll 错误"，则需要下载相应的库放到 系统目录下。这个主要是 vs2013 编译的软件运行时需要相应的运行库，在没有装 vs2013 的机子上会报这个错误。具体解决方法[参考](http://www.cr173.com/soft/103918.html)

### 3. 使用配置文件方式运行
mongodb.conf 文件
```bash
#数据库路径
dbpath=E:\program\mongodb\mongodb-data

#日志输出文件路径
#logappend=true

#错误日志采用追加模式，配置这个选项后mongodb的日志会追加到现有的日志文件，而不是从新创建一个新文件
logpath=E:\program\mongodb\mongodb-logs\mongodb.log

#启用日志文件，默认启用
#journal=true

#这个选项可以过滤掉一些无用的日志信息，若需要调试使用请设置为false
quiet=true

#端口号 默认为27017 
port=27017

#开启用户名密码,如果开启需要增加 --auth 参数打开
auth=true
```
### 4. 启动数据库
```bash
// 未开启用户密码验证方式
mongod --config E:\program\mongodb\mongodb.conf
// 开启用户密码验证方式
mongod --auth --config E:\program\mongodb\mongodb.conf
```
上面的 `--config` 是指定了运行时的配置文件。


### 5. 命令行方式连接服务
```bash
$ mongo
MongoDB shell version: 3.0.7-pre-
connecting to: test
>
// 使用用户名和密码登入 --authenticationDatabase "admin" 是验证授权对应的数据库
mongo -u root -p root --authenticationDatabase "admin"
```

### 6. Nodejs 方式连接服务器
这里以 Node.js 代码为例
```js
// config.js 文件

let packageJson = require('../../package.json');

let config = {
  // 无密码，连接 admin 数据库
	mongodbUrl: 'mongodb://127.0.0.1:27017',
  // 使用用户名和密码，连接 kuku 数据库
  mongodbUrl: 'mongodb://kuku:kuku@127.0.0.1:27017/kuku'
	version: packageJson.version
}
module.exports = config

// mongodb.js
let config = require('../config/index.js');
// mongoose 链接
let mongoose = require('mongoose');
mongoose.connect(config.mongodbUrl,{useMongoClient: true});
// 链接错误
let db = mongoose.connection;
db.on('error', console.error.bind(console, 'connection error:'));
db.once('open', function() {
  console.log(`mongodb are connected!`);
});

```

### 7. 命令行查看数据库
```bash
cd x:\xx\mongodb\bin\
mongo
use blog // blog 是数据库 名称
db.users.find() //users 是集合数据库中的 集合 名称。
```

### 8. 使用图形工具查看
使用 [Robomongo](https://robomongo.org/) 工具查看，是一个基于 Shell 的跨平台开源 MongoDB 的视图管理工具.

## 二、MongoDB Linux 环境安装

### 1. 查看 linux 服务器版本
```bash
[root@fapiao ~]# cat /proc/version
Linux version 3.10.0-514.2.2.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.5 20150623 (Red Hat 4.8.5-11) (GCC) ) #1 SMP Tue Dec 6 23:06:41 UTC 2016
```
- [选择版本下载](https://www.mongodb.com/download-center#community)
> 如果没有匹配的版本，可以选择 `legacy Linux 64`

### 2. 解压
```bash
tar -zxvf mongodb-linux-x86_64-3.0.6.tgz 
// 将解压包拷贝到指定目录
mv  mongodb-linux-x86_64-3.0.6/ /usr/local/mongodb 

// 可执行文件加入环境变量-永久有效
vi /etc/profile
// 在文件的尾部添加：PATH=$PATH:/usr/local/mongodb/bin
source /etc/profile // 可以让刚设置马上生效
```

### 3. 创建数据和日志存放目录
```bash
mkdir /var/mongodb           // 软件安装位置
mkdir /var/mongodb/data      // 数据存放位置
mkdir /var/mongodb/logs      // 日志存放位置
```

### 4. 添加 MongoDB 到 CentOS 开机启动项：
打开 `rc.local` 文件
```bash
vim /etc/rc.d/rc.local

//将 mongodb 启动命令追加到本文件中，让 mongodb 开机自启动：
// 这里通过 --dbpath 设置了 mongodb 数据存放位置,上面的步骤已经把 mongodb 的 bin 目录加到了环境变量中，所以可以直接使用 mongod，否则需要用绝对路径代替(/usr/local/mongodb/bin/mongod)
mongod --dbpath=/var/mongodb/data --logpath /var/mongodb/logs/log.log -fork

//关闭 vim(:wq) 后，直接手动启动 mongodb
mongod --dbpath=/var/mongodb/data --logpath /var/mongodb/logs/log.log -fork
```
> MongoDB 的数据存储在 data 目录的  db目录下，但是这个目录在安装过程不会自动创建，所以你需要手动创建data目录，并在data目录中创建db目录。
以下实例中我们将 data 目录创建于 **根目录下(/)**。
注意：/data/db 是 MongoDB 默认的启动的数据库路径(--dbpath)。

## 三、MongoDB 用户管理

>1. MongoDB 是没有默认管理员账号，所以要先添加管理员账号，再开启权限认证
2. 切换到 admin 数据库，添加的账号才是管理员账号
3. 用户只能在用户所在数据库登录，包括管理员账号
4. 管理员可以管理所有数据库，但是不能直接管理其他数据库，要先在 admin 数据库认证后才可以
5. MongoDB 默认会创建 `admin` 和 `local` 两个数据库

### 1. 角色概念
#### 1. 读写权限: read/readWrite 读写库的权限
#### 2. 数据库管理角色：    
- dbAdmin  某数据库管理权限
- userAdmin  某数据库用户的管理权限，包括创建用户，授权的管理
- dbOwner 某数据库的所有者，拥有该库的所有权限，包括 readWrite，dbAdmin 和 userAdmin 权限

#### 3. 集群权限
- 备份和恢复角色：bakcup/restore
- 所有数据库角色：    
	- readAnyDatabase
	- readWriteAnyDatabase
	- dbAdminAnyDatabase
	- userAdminAnyDatabase

#### 4. 超级用户角色：root
#### 5. 内部角色 ：__system  不建议使用 

### 2. 用户操作
#### 1. 创建管理用户
语法：`db.createUser({user:"UserName",pwd:"Password",roles:[{role:"RoleName",db:"Target_DBName"}]})`
```bash
use admin //admin 数据库
db.createUser({
	user: "root", // 用户名
	pwd:"root",// 密码
	roles:[{role:"root",db:"admin"}] //超级管理员
})
```
#### 2. 创建普通用户
```bash
use test
db.createUser({
	user: "test", // 用户名
	pwd:"test",// 密码
	roles:[{role:"readWrite",db:"test"}] //超级管理员
})
```
**创建完用户密码，需要重新启动数据库服务才能生效**

#### 3. 使用账号密码登入 
```bash
// 方式一
mongo -u root -p root --authenticationDatabase "admin"
// 方式二
mongo
use admin // 默认链接的是 test
db.auth("用户名","密码");
1 // 返回 1 表示验证成功，其它值失败
```

#### 4. 修改密码
```bash
db.changeUserPassword("root","ROOT")
```

#### 5. 添加角色
```bash
db.grantRolesToUser("test", [{ role:"write",db:"test"}])
```

#### 6. 回收角色
```bash
db.revokeRolesFromUser("test",[{ role: "read",db:"admin"}])
```

#### 7. 删除用户
```bash
首先进入目标库：use test
db.dropUser("test")
```

> [mongodb role 权限定义](https://docs.mongodb.com/manual/reference/built-in-roles/#built-in-roles)


## 四、常用命令
通过 `mongo` 工具进入 mongodb 后台管理 shell

### 1. 查看所有数据库
```bash
// 显示所有数据库
show dbs

// 显示某个数据库下 -> 表数
use xxx
show tables
```

### 2. 切换数据库
```bash
// 如果数据库不存在则会自动创建一个,这时通过　`show dbs` 查看并没有显示，需要插入数据时才会实际创建。
use kuku(数据库名称)
```

### 3. 删除数据库数据
```bash
// 删除当前选择的数据库所有数据
use test
db.dropDatabase() // 删除的是 test 整个数据库

// 删除数据库下的 -> 某个表数据
db.kuku_collection.drop();
```
> `kuku_collection` 是表名称，下面实例中相同

### 4. 查询数据
```bash
// 查询所有
db.kuku_collection.find(); 

// 只查询 name:'jesse',age:18 符合的数据
db.kuku_collection.find({name:'jesse',age:18}); 

// 查询跳过前面 2 条，并限制输出 4 条数据,并使用 age 进行排序({x:1} 升序，{x:-1} 降序)
db.kuku_collection.find().skip(2).limit(4).sort({age:1});
```

### 5. 数据更新
数据更新需要两个条件，一个查询条件，一个是更新数据
```bash
// 这里会把新的数据覆盖掉原有的数据(原来的数据全部没了)。
db.kuku_collection.update({age:18},{age:10,name:'jesse'});

// 如果更新的数据不存在，需要创建，需要设置第 三 个参数为 true
db.kuku_collection.update({age:99},{age:100},true)
WriteResult({
      "nMatched" : 0,
      "nUpserted" : 1,
      "nModified" : 0,
      "_id" : ObjectId("59b869a779a1211c60e23f00")
})

// 如果更新是查询结果有多条满足条件，只会更新第一条，这是为了避免误操作，如果需要更新全部满足条件的数据，只能用 $set 操作符，并设置第 四 个参数为 true
db.kuku_collection.update({age:18},{$set:{age:10,name:'jesse'}},false,true);

// 使用 $set 操作符可以只更新存在的数据，其它值保存不变;
db.kuku_collection.update({age:18},{$set:{age:10,name:'jesse'}});

```

### 6. 数据删除
删除操作必须带 `参数`,且会删除所有满足条件的数据(和更新操作有所差别)
```bash
// 不带参数会报错
db.kuku_collection.remove();
2017-09-13T07:19:21.139+0800 E QUERY    Error: remove needs a query
  at Error (<anonymous>)
  at DBCollection._parseRemove (src/mongo/shell/collection.js:305:32)
  at DBCollection.remove (src/mongo/shell/collection.js:328:23)
  at (shell):1:20 at src/mongo/shell/collection.js:305

// 正确使用
db.kuku_collection.remove({age:18});
WriteResult({ "nRemoved" : 2 })

// 删除数据库中某个 -> 表数据
db.kuku_collection.drop();

```

### 7. 数据索引
默认的只 `_id` 一个索引，如果数据库过大，查询某个值时就会变得缓慢，这时就需要更加查询的值建立新的索引。
```bash

// 查看当前索引值
db.kuku_collection.getIndexes();
[
      {
              "v" : 1,
              "key" : {
                      "_id" : 1
              },
              "name" : "_id_",
              "ns" : "kuku.kuku_collection"
      }
]

// 创建索引,1:安装升序 -1:降序(类似 sort 参数)
db.kuku_collection.ensureIndex({age:1}); 
{
      "createdCollectionAutomatically" : false,
      "numIndexesBefore" : 1,
      "numIndexesAfter" : 2,
      "ok" : 1
}

// 再次查看索引值
db.kuku_collection.getIndexes()
[
        {
                "v" : 1,
                "key" : {
                        "_id" : 1
                },
                "name" : "_id_",
                "ns" : "kuku.kuku_collection"
        },
        {
                "v" : 1,
                "key" : {
                        "age" : 1
                },
                "name" : "age_1",
                "ns" : "kuku.kuku_collection"
        }
]

// 创建复合索引名字
db.kuku_collection.ensureIndex({name:1,age:1,weight:1},{name:'haha'})
{
      "v" : 1,
      "key" : {
              "name" : 1,
              "age" : 1,
              "weight" : 1
      },
      "name" : "haha", // 复合索引名称
      "ns" : "kuku.kuku_collection"
}

// 删除索引
db.kuku_collection.dropIndex('haha'); // 通过索引名称

// 创建唯一索引
// 可以实现，如果已经存在则不插入，不存在则插入
db.kuku_collection.ensureIndex({age:1},{unique:true});
```

### 8. 过期索引
1. 过期索引字段值必须是指定的时间类型(ISODate或者ISODate 数组),不能是时间戳。例如: {time:new Date()}
2. 如果指定了 ISODate 数组，则按照最小的时间进行删除。
3. 过期索引不能是复合索引。
4. 删除时间不是精确的，删除过程是有后台程序每 60S 跑一次，且删除需要一些时间，所有存在误差。
```bash
// 创建 loginTime 字段索引，过期时间为 10S
db.kuku_collection.ensureIndex({loginTime:1},{expireAfterSeconds:10});
db.kuku_collection.insert({loginTime:new Date(),age:888,name:'haha'});
```

### 9. 全文索引
当我们需要根据页面中的文字信息查询数据库时，就需要用到全文索引;
一张表只能创建一个全文索引。
**注意这里创建的全文索 引，是在数据下面，不是 `_collection` 下面**
* db.kuku_collection.ensureIndex({key:'text'}); // 创建当个全文索引
* db.kuku_collection.ensureIndex({key_1:'text',key_2:'text'});// 创建符号全文索引
* db.kuku_collection.ensureIndex({'$**':'text'});// 创建不同 key 之间的全文索引

```bash
// 创建了 name 字段的全文索引
db.kuku_collection.ensureIndex({name:'text'});

// 全文索引查询
db.kuku_collection.find({$text:{$search:'phone'}});
db.kuku_collection.find({$text:{$search:'aa bb cc'}}); // aa bb cc 是或的关系
db.kuku_collection.find({$text:{$search:'"aa" "bb" "cc"'}});// "aa" "bb" "cc" 是与的关系(多了一层引号)
db.kuku_collection.find({$text:{$search:'aa bb -cc'}});// aa||bb 不包含 cc 
```

### 10. 全文搜索相似度
类似百度搜索排名
```bash
// 查询结果中会多一个相似度数值 score:xx
db.kuku_collection.find({$text:{$search:'aa bb'}},{score:{$meta:'textScore'}});
{ "_id" : ObjectId("59b87bb35988ad826faef97d"), "name" : "aa bb", "score" : 1.5}
{ "_id" : ObjectId("59b87bb65988ad826faef97e"), "name" : "aa bb cc", "score" : 1.3333333333333333 }
{ "_id" : ObjectId("59b878b45988ad826faef975"), "name" : "aa bb cc", "score" : 1.3333333333333333 }
{ "_id" : ObjectId("59b87bbd5988ad826faef980"), "name" : "aa rr cc dd", "score": 0.625 }
{ "_id" : ObjectId("59b87bb85988ad826faef97f"), "name" : "aa bb cc dd", "score": 1.25 }
{ "_id" : ObjectId("59b878bb5988ad826faef976"), "name" : "aa bb cc dd ", "score": 1.25 }
{ "_id" : ObjectId("59b878ca5988ad826faef979"), "name" : "aa xx uu dd rr", "score" : 0.6 }
{ "_id" : ObjectId("59b878c15988ad826faef978"), "name" : "aa xx cc dd rr", "score" : 0.6 }
{ "_id" : ObjectId("59b878be5988ad826faef977"), "name" : "aa bb cc dd rr", "score" : 1.2 }

// 可以按相似度排序
db.kuku_collection.find({$text:{$search:'aa bb'}},{score:{$meta:'textScore'}}).sort(score:{$meta:'textScore'});
{ "_id" : ObjectId("59b87bb35988ad826faef97d"), "name" : "aa bb", "score" : 1.5}
{ "_id" : ObjectId("59b878b45988ad826faef975"), "name" : "aa bb cc", "score" : 1.3333333333333333 }
{ "_id" : ObjectId("59b87bb65988ad826faef97e"), "name" : "aa bb cc", "score" : 1.3333333333333333 }
{ "_id" : ObjectId("59b878bb5988ad826faef976"), "name" : "aa bb cc dd ", "score": 1.25 }
{ "_id" : ObjectId("59b87bb85988ad826faef97f"), "name" : "aa bb cc dd", "score": 1.25 }
{ "_id" : ObjectId("59b878be5988ad826faef977"), "name" : "aa bb cc dd rr", "score" : 1.2 }
{ "_id" : ObjectId("59b87bbd5988ad826faef980"), "name" : "aa rr cc dd", "score": 0.625 }
{ "_id" : ObjectId("59b878c15988ad826faef978"), "name" : "aa xx cc dd rr", "score" : 0.6 }
{ "_id" : ObjectId("59b878ca5988ad826faef979"), "name" : "aa xx uu dd rr", "score" : 0.6 }
```

### 11. 全文索引限制
* 每次查询只能指定一个 `$text`
* `$text` 查询不能出现在 `$nor` 查询中
* 查询中包含 `$text`,则 `hint` 不再起作用
* 不支持 中文


## 参考
- [Nodejs 部署到阿里云全过程](http://www.jianshu.com/p/0496ef49b2a5)
- [MongoDB Windows 环境安装及配置](http://www.cnblogs.com/lzrabbit/p/3682510.html)
- [MongoDB 生态 – 可视化管理工具](http://www.mongoing.com/archives/3651)