# MongoDB

> [MongoDB安装](https://blog.csdn.net/weixin_41466575/article/details/105326230)

## 启动与关闭

```js
// 启动
.\bin\mongod -dbpath C:\RUIHUA\env\MongoDB\data\db


// 关闭
.\bin\mongo localhost:27017

use admin;
db.shutdownServer();
exit
```

