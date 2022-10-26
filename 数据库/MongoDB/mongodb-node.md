---
title: mongodb-node
date: 2021-01-20 16:42:55
tags:
- mongodb
- node
---

# Node.js 连接 MongoDB

>  [Mongoose v5.13.2: API docs (mongoosejs.com)](https://mongoosejs.com/docs/api.html)
>
>  MongoDB是一种文档导向数据库管理系统，由C++撰写而成。

### 安装驱动

```shell
$ cnpm install mongodb
```

------

## 创建数据库

要在 MongoDB 中创建一个数据库，首先我们需要创建一个 MongoClient 对象，然后配置好指定的 URL 和 端口号。

如果数据库不存在，MongoDB 将创建数据库并建立连接。

## 创建连接

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://localhost:27017/runoob";  
MongoClient.connect(url, function(err, db) {  
  if (err) throw err;  
  console.log("数据库已创建!");  
  db.close(); 
});
```



------

## 创建集合

我们可以使用 createCollection() 方法来创建集合：

## 创建集合

```js
var MongoClient = require('mongodb').MongoClient; 
var url = 'mongodb://localhost:27017/runoob'; 
MongoClient.connect(url, function (err, db) {    
  if (err) throw err;    
  console.log('数据库已创建');    
  var dbase = db.db("runoob");    
  dbase.createCollection('site', function (err, res) {        
    if (err) throw err;        
    console.log("创建集合!");        
    db.close();    
  }); 
});
```



------

## 数据库操作( CURD )

与 MySQL 不同的是 MongoDB 会自动创建数据库和集合，所以使用前我们不需要手动去创建。

### 插入数据

以下实例我们连接数据库 runoob 的 site 表，并插入一条数据条数据，使用 **insertOne()**：

## 插入一条数据

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://localhost:27017/";  
MongoClient.connect(url, function(err, db) {    
  if (err) throw err;    
  var dbo = db.db("site");    
  var myobj = { name: "grh", url: "ruihua" };    
  dbo.collection("site").insertOne(myobj, function(err, res) {        
    if (err) throw err;        
    console.log("文档插入成功");        
    db.close();    
  }); 
});
```



执行以下命令输出就结果为：

```shell
$ node test.js
文档插入成功
```

从输出结果来看，数据已插入成功。

我们也可以打开 MongoDB 的客户端查看数据，如：

```shell
> show dbs
runoob  0.000GB          # 自动创建了 runoob 数据库
> show tables
site                     # 自动创建了 site 集合（数据表）
> db.site.find()
{ "_id" : ObjectId("5a794e36763eb821b24db854"), "name" : "菜鸟教程", "url" : "www.runoob" }
> 
```

如果要插入多条数据可以使用 **insertMany()**：

## 插入多条数据

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://localhost:27017/";  
MongoClient.connect(url, function(err, db) {    
  if (err) throw err;    
  var dbo = db.db("koadb");    
  var myobj =  [        
    { name: 'grh', url: 'https://github.com', type: 'cn'},        
    { name: 'Google', url: 'https://www.google.com', type: 'en'},        
    { name: 'Facebook', url: 'https://www.google.com', type: 'en'}       
  ];    
  dbo.collection("site").insertMany(myobj, function(err, res) {        
    if (err) throw err;        
    console.log("插入的文档数量为: " + res.insertedCount);        
    db.close();    
  }); 
});
```



res.insertedCount 为插入的条数。

### 查询数据

可以使用 find() 来查找数据, find() 可以返回匹配条件的所有数据。 如果未指定条件，find() 返回集合中的所有数据。

## find()

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://localhost:27017/";  
MongoClient.connect(url, function(err, db) {    
  if (err) throw err;    
  var dbo = db.db("koadb");    
  dbo.collection("site"). find({}).toArray(function(err, result) { 
    // 返回集合中所有数据        
    if (err) throw err;        
    console.log(result);        
    db.close();    
  }); 
});
```



以下实例检索 name 为 "菜鸟教程" 的实例：

## 查询指定条件的数据

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://localhost:27017/";  
MongoClient.connect(url, function(err, db) {    
  if (err) throw err;    
  var dbo = db.db("koadb");     
  var whereStr = {"name":'菜鸟教程'};  // 查询条件 
  dbo.collection("site").find(whereStr).toArray(function(err, result) {        
    if (err) throw err;        
    console.log(result);        
    db.close();    
  }); 
});
```



执行以下命令输出就结果为：

```shell
[ { _id: 5a794e36763eb821b24db854,
    name: '菜鸟教程',
    url: 'www.runoob' } ]
```

### 更新数据

我们也可以对数据库的数据进行修改，以下实例将 name 为 "菜鸟教程" 的 url 改为 https://www.runoob.com：

## 更新一条数据

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://localhost:27017/";  
MongoClient.connect(url, function(err, db) {    
  if (err) throw err;    
  var dbo = db.db("runoob");    
  var whereStr = {"name":'菜鸟教程'};  
  // 查询条件    
  var updateStr = {$set: { "url" : "https://www.runoob.com" }};    
  dbo.collection("site").updateOne(whereStr, updateStr, function(err, res) {        
    if (err) throw err;        
    console.log("文档更新成功");        
    db.close();    
  }); 
});
```



执行成功后，进入 mongo 管理工具查看数据已修改：

```shell
> db.site.find().pretty()
{
    "_id" : ObjectId("5a794e36763eb821b24db854"),
    "name" : "菜鸟教程",
    "url" : "https://www.runoob.com"     // 已修改为 https
}
```

如果要更新所有符合条的文档数据可以使用 **updateMany()**：

## 更新多条数据

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://localhost:27017/";  
MongoClient.connect(url, function(err, db) {    
  if (err) throw err;    
  var dbo = db.db("runoob");    
  var whereStr = {"type":'en'};  // 查询条件    
  var updateStr = {$set: { "url" : "https://www.runoob.com" }};    dbo.collection("site").updateMany(whereStr, updateStr, function(err, res) {        
    if (err) throw err;         
    console.log(res.result.nModified + " 条文档被更新");        
    db.close();    
  }); 
});
```



result.nModified 为更新的条数。

### 删除数据

以下实例将 name 为 "菜鸟教程" 的数据删除 :

## 删除一条数据

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://localhost:27017/";  
MongoClient.connect(url, function(err, db) {    
  if (err) throw err;    
  var dbo = db.db("runoob");    
  var whereStr = {"name":'菜鸟教程'};  // 查询条件 
  dbo.collection("site").deleteOne(whereStr, function(err, obj) {        
    if (err) throw err;        
    console.log("文档删除成功");        
    db.close();    
  }); 
});
```



执行成功后，进入 mongo 管理工具查看数据已删除：

```shell
> db.site.find()
> 
```

如果要删除多条语句可以使用 **deleteMany()** 方法

以下实例将 type 为 en 的所有数据删除 :

## 删除多条数据

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://localhost:27017/";  
MongoClient.connect(url, function(err, db) {    
  if (err) throw err;    
  var dbo = db.db("runoob");    
  var whereStr = { type: "en" };  // 查询条件   
  dbo.collection("site").deleteMany(whereStr, function(err, obj) {        
    if (err) throw err;        
    console.log(obj.result.n + " 条文档被删除");        
    db.close();    
  }); 
});
```



obj.result.n 删除的条数。

### 排序

排序 使用 sort() 方法，该方法接受一个参数，规定是升序(1)还是降序(-1)。

例如：

```shell
{ type: 1 }  // 按 type 字段升序
{ type: -1 } // 按 type 字段降序
```

按 type 升序排列:

## 排序

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://localhost:27017/";  
MongoClient.connect(url, function(err, db) {    
  if (err) throw err;    
  var dbo = db.db("runoob");    
  var mysort = { type: 1 };    
  dbo.collection("site").find().sort(mysort).toArray(function(err, result) {        
    if (err) throw err;        
    console.log(result);        
    db.close();    
  }); 
});
```



### 查询分页

如果要设置指定的返回条数可以使用 **limit()** 方法，该方法只接受一个参数，指定了返回的条数。

## limit()：读取两条数据

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://localhost:27017/";  
MongoClient.connect(url, function(err, db) {    
  if (err) throw err;    
  var dbo = db.db("runoob");    
  dbo.collection("site").find().limit(2).toArray(function(err, result) {        
    if (err) throw err;        
    console.log(result);        
    db.close();  
  }); 
});
```



如果要指定跳过的条数，可以使用 **skip()** 方法。

## skip(): 跳过前面两条数据，读取两条数据

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://localhost:27017/";  
MongoClient.connect(url, function(err, db) {    
  if (err) throw err;    
  var dbo = db.db("runoob");    
  dbo.collection("site").find().skip(2).limit(2).toArray(function(err, result) {        
    if (err) throw err;        
    console.log(result);        
    db.close();  
  }); 
});
```



### 连接操作

mongoDB 不是一个关系型数据库，但我们可以使用 **$lookup** 来实现左连接。

例如我们有两个集合数据分别为：

集合1：orders

```js
[
  { _id: 1, product_id: 154, status: 1 }
]
```

集合2：products

```js
[
  { _id: 154, name: '笔记本电脑' },
  { _id: 155, name: '耳机' },
  { _id: 156, name: '台式电脑' }
]
```

## $lookup 实现左连接

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://127.0.0.1:27017/";  
MongoClient.connect(url, function(err, db) {  
  if (err) throw err;  
  var dbo = db.db("runoob");  
  dbo.collection('orders').aggregate([    
    { 
      $lookup:{         
        from: 'products',            // 右集合         
         localField: 'product_id',    // 左集合 join 字段         
         foreignField: '_id',         // 右集合 join 字段         
         as: 'orderdetails'           // 新生成字段（类型array）       
       }     
    }    
  ]).toArray(function(err, res) {    
    if (err) throw err;    
    console.log(JSON.stringify(res));    
    db.close();  
  }); 
});
```



### 删除集合

我们可以使用 **drop()** 方法来删除集合：

## drop()

```js
var MongoClient = require('mongodb').MongoClient; 
var url = "mongodb://localhost:27017/";  
MongoClient.connect(url, function(err, db) {    
  if (err) throw err;    
  var dbo = db.db("test");    // 删除 test 集合
  dbo.collection("test").drop(function(err, delOK) {  // 执行成功 delOK 返回 true，否则返回 false        
    if (err) throw err;        
    if (delOK) console.log("集合已删除");        
    db.close();    
  }); 
});
```



------

## 使用 Promise

Promise 是一个 ECMAScript 6 提供的类，目的是更加优雅地书写复杂的异步任务。

如果你还不了解 Promise，可以参考 [JavaScript Promise](https://www.runoob.com/js/js-promise.html)。

以下实例使用 Promise 创建集合：

## 实例

```js
const MongoClient = require("mongodb").MongoClient; 
const url = "mongodb://localhost/runoob"; 
MongoClient.connect(url).then((conn) => {    
  console.log("数据库已连接");    
  var dbase = conn.db("runoob");    
  dbase.createCollection("site").then((res) => {        
    console.log("已创建集合");    
  }).catch((err) => {        
    console.log("数据库操作错误");    
  }).finally(() => {        
    conn.close();    
  }); 
}).catch((err) => {    
  console.log("数据库连接失败"); 
});
```



### Promise 数据操作

现在我们在一个程序中实现四个连续操作：增加 、查询 、更改 、删除。

## 实例

```js
const MongoClient = require("mongodb").MongoClient; 
const url = "mongodb://localhost/"; 
MongoClient.connect(url).then((conn) => {    
  console.log("数据库已连接");    
  const test = conn.db("testdb").collection("test");    // 增加    
  test.insertOne({ "site": "runoob.com" }).then((res) => {        // 查询        
    return test.find().toArray().then((arr) => {            
      console.log(arr);        
    });    
  }).then(() => {        // 更改        
    return test.updateMany({ "site": "runoob.com" },{ $set: { "site": "example.com" } }); 
  }).then((res) => {        // 查询        
    return test.find().toArray().then((arr) => {            
      console.log(arr);        
    });    
  }).then(() => {        // 删除        
    return test.deleteMany({ "site": "example.com" });   
  }).then((res) => {        // 查询        
    return test.find().toArray().then((arr) => {            
      console.log(arr);        
    });    
  }).catch((err) => {        
    console.log("数据操作失败" + err.message);    
  }).finally(() => {        
    conn.close();    
  }); 
}).catch((err) => {    
  console.log("数据库连接失败"); 
});
```



执行结果：

```js
数据库已连接
[ { _id: 5f1664966833e531d83d3ac6, site: 'runoob.com' } ]
[ { _id: 5f1664966833e531d83d3ac6, site: 'example.com' } ]
[]
```

### 用异步函数实现相同的数据操作

## 实例

```js
const MongoClient = require("mongodb").MongoClient; 
const url = "mongodb://localhost/";  
async function dataOperate() {    
  var conn = null;    
  try {        
    conn = await MongoClient.connect(url);        
    console.log("数据库已连接");        
    const test = conn.db("testdb").collection("test");        
    // 增加        
    await test.insertOne({ "site": "runoob.com" });        
    // 查询        
    var arr = await test.find().toArray();        
    console.log(arr);        
    // 更改        
    await test.updateMany({ "site": "runoob.com" },{ $set: { "site": "example.com" } });     // 查询        
    arr = await test.find().toArray();        
    console.log(arr);        
    // 删除        
    await test.deleteMany({ "site": "example.com" });        
    // 查询        
    arr = await test.find().toArray();        
    console.log(arr);    
  } catch (err) {        
    console.log("错误：" + err.message);    
  } finally {        
    if (conn != null) conn.close();    
  } 
}  
dataOperate();
```



运行结果：

```js
数据库已连接
[ { _id: 5f169006a2780f0cd4ea640b, site: 'runoob.com' } ]
[ { _id: 5f169006a2780f0cd4ea640b, site: 'example.com' } ]
[]
```