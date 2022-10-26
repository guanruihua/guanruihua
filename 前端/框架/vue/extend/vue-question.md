---
title: vue-question
date: 2021-3-20 10:04:44
tags: 
- vue
- question
---



# VUE-QUESTION

## Vue提示报错警告"TypeError: handler.call is not a function"



> 组件中声明了未定义的方法，或者只声明了钩子函数，并未使用。

## ` vue/cle3`项目运行报错`sockjs-node/info`解决方案

### 控制台报错

```shell
get http://localhost:8080/sockjs-node/info?t=1462183700002 net::ERR_CONNECTION_REFUSED
[WDS] Disconnected!
get http://localhost:8080/sockjs-node/info?t=1462183700002 net::ERR_CONNECTION_REFUSED
[WDS] Disconnected!
get http://localhost:8080/sockjs-node/info?t=1462183700002 net::ERR_CONNECTION_REFUSED
[WDS] Disconnected!
...
```

### 解决方案

#### 1. 注释法

  顾名思义，找到依赖包中的源码，将其注释：

1. 进入路径 `/node_modules/sockjs-client/dist/sockjs.js`
2. 代码1605行注释掉：

```javascript
try {
        // self.xhr.send(payload);  //本行注释
    } catch (e) {
        self.emit('finish', 0, '');
        self._cleanup(false);
    }
```

3. 重启项目

#### 2. 配置`vue.config`

  ` vue.config.js`中的`module.xports`中添加如下，然后重启：

```javascript
devServer: {
    proxy: 'http://localhost:8080',
    public: '192.168.xxx.xxx:8080'  // 本地ip
}
```