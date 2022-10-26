---
title: egg 
date: 2020-09-30 11:11:36 
tags:
	- egg
	- front-end
	- 日志
---

# Egg

> [官网](https://eggjs.org/zh-cn/intro/quickstart.html)

##  特性

- 提供基于 Egg [定制上层框架](https://eggjs.org/zh-cn/advanced/framework.html)的能力
- 高度可扩展的[插件机制](https://eggjs.org/zh-cn/basics/plugin.html)
- 内置[多进程管理](https://eggjs.org/zh-cn/advanced/cluster-client.html)
- 基于 [Koa](http://koajs.com/) 开发，性能优异
- 框架稳定，测试覆盖率高
- [渐进式开发](https://eggjs.org/zh-cn/tutorials/progressive.html)

## 环境搭建

```basic
$ mkdir egg-example && cd egg-example
$ npm init egg --type=simple
$ npm i
# 启动
$ npm run dev
$ open http://localhost:7001
```