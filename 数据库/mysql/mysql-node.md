---
title: mysql-node
date: 2021-01-20 16:23:33
tags:
- node
- mysql
---



# Node.js 连接 MySQL

> https://github.com/mysqljs/mysql

## 安装驱动

```shell
$ cnpm install mysql
```

## 连接数据库

在以下实例中根据你的实际配置修改数据库用户名、及密码及数据库名：

```js
var mysql = require('mysql'); 
var connection = mysql.createConnection({  
  host     : 'localhost',  
  user     : 'root',  
  password : '123456',  
  database : 'test' });  
connection.connect();  
connection.query('SELECT 1 + 1 AS solution', function (error, results, fields) {  
  if (error) throw error;  
  console.log('The solution is: ', results[0].solution); 
});
```



执行以下命令输出结果为：

```shell
$ node test.js
The solution is: 2
```



### 数据库连接参数说明：

| 参数               | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| host               | 主机地址 （默认：localhost）                                 |
| user               | 用户名                                                       |
| password           | 密码                                                         |
| port               | 端口号 （默认：3306）                                        |
| database           | 数据库名                                                     |
| charset            | 连接字符集（默认：'UTF8_GENERAL_CI'，注意字符集的字母都要大写） |
| localAddress       | 此IP用于TCP连接（可选）                                      |
| socketPath         | 连接到unix域路径，当使用 host 和 port 时会被忽略             |
| timezone           | 时区（默认：'local'）                                        |
| connectTimeout     | 连接超时（默认：不限制；单位：毫秒）                         |
| stringifyObjects   | 是否序列化对象                                               |
| typeCast           | 是否将列值转化为本地JavaScript类型值 （默认：true）          |
| queryFormat        | 自定义query语句格式化方法                                    |
| supportBigNumbers  | 数据库支持bigint或decimal类型列时，需要设此option为true （默认：false） |
| bigNumberStrings   | supportBigNumbers和bigNumberStrings启用 强制bigint或decimal列以JavaScript字符串类型返回（默认：false） |
| dateStrings        | 强制timestamp,datetime,data类型以字符串类型返回，而不是JavaScript Date类型（默认：false） |
| debug              | 开启调试（默认：false）                                      |
| multipleStatements | 是否许一个query中有多个MySQL语句 （默认：false）             |
| flags              | 用于修改连接标志                                             |
| ssl                | 使用ssl参数（与crypto.createCredenitals参数格式一至）或一个包含ssl配置文件名称的字符串，目前只捆绑Amazon RDS的配置文件 |

------

## 数据库操作( CURD )

```js
let sql = ''; // 相当于在sql中执行的语句 [可以写insert, delete, select, update ]
let Params = [];// 当 sql 有?, Params 中的参数和sql一一对应 
connection.query(sql, [Params], function (err, result) { }
```



### 查询数据

```js
var mysql  = require('mysql');    
var connection = mysql.createConnection({       
  host     : 'localhost',         
  user     : 'root',                
  password : '123456',         
  port: '3306',                     
  database: 'test'  });   
connection.connect();  
var  sql = 'SELECT * FROM websites'; 
//查 
connection.query(sql,function (err, result) {        
  if(err){          
    console.log('[SELECT ERROR] - ',err.message);          
    return;        
  }        
  console.log('--------------------------SELECT----------------------------');       	   		console.log(result);       
  console.log('------------------------------------------------------------\n\n');   
});  
connection.end();
```



执行以下命令输出就结果为：

```shell
$ node test.js
--------------------------SELECT----------------------------
[ RowDataPacket {
    id: 1,
    name: 'Google',
    url: 'https://www.google.cm/',
    alexa: 1,
    country: 'USA' },
  RowDataPacket {
    id: 2,
    name: '淘宝',
    url: 'https://www.taobao.com/',
    alexa: 13,
    country: 'CN' },
  RowDataPacket {
    id: 3,
    name: '菜鸟教程',
    url: 'http://www.runoob.com/',
    alexa: 4689,
    country: 'CN' },
  RowDataPacket {
    id: 4,
    name: '微博',
    url: 'http://weibo.com/',
    alexa: 20,
    country: 'CN' },
  RowDataPacket {
    id: 5,
    name: 'Facebook',
    url: 'https://www.facebook.com/',
    alexa: 3,
    country: 'USA' } ]
------------------------------------------------------------
```

### 插入数据

```js
var mysql  = require('mysql');    
var connection = mysql.createConnection({       
  host     : 'localhost',         
  user     : 'root',                
  password : '123456',         
  port: '3306',                     
  database: 'test'  });   
connection.connect();  
var  addSql = 'INSERT INTO websites(Id,name,url,alexa,country) VALUES(0,?,?,?,?)'; 
var  addSqlParams = ['菜鸟工具', 'https://c.runoob.com','23453', 'CN']; 
//增 
connection.query(addSql, addSqlParams,function (err, result) {        
  if(err){         
    console.log('[INSERT ERROR] - ',err.message);         
    return;        
  }                
  console.log('--------------------------INSERT----------------------------');       //console.log('INSERT ID:',result.insertId);               
  console.log('INSERT ID:',result);               
  console.log('-----------------------------------------------------------------\n\n');   });  
connection.end();
```



执行以下命令输出就结果为：

```shell
$ node test.js
--------------------------INSERT----------------------------
INSERT ID: OkPacket {
  fieldCount: 0,
  affectedRows: 1,
  insertId: 6,
  serverStatus: 2,
  warningCount: 0,
  message: '',
  protocol41: true,
  changedRows: 0 }
-----------------------------------------------------------------
```

执行成功后，查看数据表，即可以看到添加的数据：

![img](https://www.runoob.com/wp-content/uploads/2017/03/1E2D27C5-45F3-4E57-90D3-4AE35AEB17F7.jpg)

### 更新数据

```js
var mysql  = require('mysql');    
var connection = mysql.createConnection({       
  host     : 'localhost',         
  user     : 'root',                
  password : '123456',         
  port: '3306',                     
  database: 'test'  });   
connection.connect();  
var modSql = 'UPDATE websites SET name = ?,url = ? WHERE Id = ?'; 
var modSqlParams = ['菜鸟移动站', 'https://m.runoob.com',6]; 
//改 
connection.query(modSql, modSqlParams,function (err, result) {   
  if(err){         
    console.log('[UPDATE ERROR] - ',err.message);         
    return;   
  }          
  console.log('--------------------------UPDATE----------------------------');  console.log('UPDATE affectedRows',result.affectedRows);  
  console.log('-----------------------------------------------------------------\n\n'); });  
connection.end();
```



执行以下命令输出就结果为：

```
--------------------------UPDATE----------------------------
UPDATE affectedRows 1
-----------------------------------------------------------------
```

执行成功后，查看数据表，即可以看到更新的数据：

![img](https://www.runoob.com/wp-content/uploads/2017/03/A5C8911B-E07B-48DD-9C3E-28F1E73A1A30.jpg)

### 删除数据

```js
var mysql  = require('mysql');    
var connection = mysql.createConnection({       
  host     : 'localhost',         
  user     : 'root',                
  password : '123456',         
  port: '3306',                     
  database: 'test'  });   
connection.connect();  
var delSql = 'DELETE FROM websites where id=6'; 
//删 
connection.query(delSql,function (err, result) {        
  if(err){          
    console.log('[DELETE ERROR] - ',err.message);          
    return;        
  }                
  console.log('--------------------------DELETE----------------------------');       			console.log('DELETE affectedRows',result.affectedRows);       
  console.log('-----------------------------------------------------------------\n\n');   });  
connection.end();
```



执行以下命令输出就结果为：

```
--------------------------DELETE----------------------------
DELETE affectedRows 1
-----------------------------------------------------------------
```

执行成功后，查看数据表，即可以看到 id=6 的数据已被删除：

![img](https://www.runoob.com/wp-content/uploads/2017/03/27BEB36A-0DCE-4C75-961F-93D55CBECC54.jpg)



## websites.sql

```sql
/*
 Navicat MySQL Data Transfer

 Source Server         : 127.0.0.1
 Source Server Version : 50621
 Source Host           : localhost
 Source Database       : RUNOOB

 Target Server Version : 50621
 File Encoding         : utf-8

 Date: 05/18/2016 11:44:07 AM
*/

SET NAMES utf8;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
--  Table structure for `websites`
-- ----------------------------
DROP TABLE IF EXISTS `websites`;
CREATE TABLE `websites` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` char(20) NOT NULL DEFAULT '' COMMENT '站点名称',
  `url` varchar(255) NOT NULL DEFAULT '',
  `alexa` int(11) NOT NULL DEFAULT '0' COMMENT 'Alexa 排名',
  `country` char(10) NOT NULL DEFAULT '' COMMENT '国家',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8;

-- ----------------------------
--  Records of `websites`
-- ----------------------------
BEGIN;
INSERT INTO `websites` VALUES ('1', 'Google', 'https://www.google.cm/', '1', 'USA'), ('2', '淘宝', 'https://www.taobao.com/', '13', 'CN'), ('3', '菜鸟教程', 'http://www.runoob.com/', '4689', 'CN'), ('4', '微博', 'http://weibo.com/', '20', 'CN'), ('5', 'Facebook', 'https://www.facebook.com/', '3', 'USA');
COMMIT;

SET FOREIGN_KEY_CHECKS = 1;

```

