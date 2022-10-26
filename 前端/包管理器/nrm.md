# nrm

## 安装

```shell
npm install -g nrm
```

## 命令

### 查看可选源

```shell
nrm ls                                                                                                                                   
*npm ---- https://registry.npmjs.org/

cnpm --- http://r.cnpmjs.org/

taobao - http://registry.npm.taobao.org/

eu ----- http://registry.npmjs.eu/

au ----- http://registry.npmjs.org.au/

sl ----- http://npm.strongloop.com/

nj ----- https://registry.nodejitsu.com/
```

### 切换

```shell
nrm use taobao
```

### 添加源

```shell
nrm add <registry>
nrm add registry http://registry.npm.frp.trmap.cn/
```

### 删除源

```shell
nrm del <registry>
```

### 测试源的响应时间

```shell
nrm test npm  
```
