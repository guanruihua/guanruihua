---
title: yarn
date: 2020-10-31 14:02:19
tags:
- yarn
- tool
---

# yarn

**安装 Yarn**

> npm install -g yarn

**安装 CYarn**

> npm install -g cyarn

或者使用 cnpm 源：

> npm install -g cyarn –registry=[https://registry.npm.taobao.org](https://registry.npm.taobao.org/)

**1、初始化一个新的项目**

> yarn init = 》 **npm init**

**2、添加一个依赖包**

> `yarn add [package] = 》**npm install [package] - -save**
> yarn add [package]@[version]
> yarn add [package]@[tag]
> yarn add [package] - -dev = 》npm install [package]- -save-dev
> yarn global add [package] = 》npm install [package]- -global `

**3、更新一个依赖包**

> `yarn upgrade [package] =》npm update - -save
> yarn upgrade [package]@[version]
> yarn upgrade [package]@[tag] `

**4、删除一个依赖包**

> `yarn remove [package] =》npm uninstall [package]- -save`

**5、安装所有的依赖包**

> yarn or yarn install =》**npm install**

**6、 运行脚本**

> yarn run =》 **npm run**

**7、package缓存**

> yarn cache ls 列出每个缓存的包
> yarn cache clean 清除本地缓存

**8、yarn配置文件**

> yarn config list 列出所有配置项
> yarn config get xxx 显示某项配置的值
> yarn config set xxx xxx 修改配置
> yarn config delete xxx 删除配置

更改路径配置（换成淘宝镜像）

> `yarn config set registry [https://registry.npm.taobao.org](https://registry.npm.taobao.org/)`

**9、显示包信息**

> `yarn info React`

```powershell
npm init                                 ---- yarn init
npm install                               ---- yarn add
npm install xxx@1.1.1 -g                  		 ---- yarn global add xxx@1.1.1
npm install xxx@1.1.1 --save            			  	---- yarn add xxx@1.1.1
npm install xxx@1.1.1 --save-dev    							 ---- yarn add xxx@1.1.1 --dev
npm uninstall xxx --save(-dev)         					 ----yarn remove xxx
npm run xxx                              ---- yarn run xxxx
```

