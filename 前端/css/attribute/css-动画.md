---
title: css-动画
date: 2020-10-09 15:19:48
tags: 
	- css
	- 动画
---

# css动画

> CSS动画(transform, transition, animation)

## 浏览器渲染原理

[![0DhCRO.md.png](https://s1.ax1x.com/2020/10/09/0DhCRO.md.png)](https://imgchr.com/i/0DhCRO)

### 浏览器渲染过程

1. 根据html标记构建DOM树
2. 根据CSS构建css树(CSSDOM)
3. 将两棵树合并成一棵渲染树(render tree)
4. layout布局(文档流, 盒模型, 大小等)
5. paint绘制(边框颜色, 背景颜色等)
6. compose合成(根据层叠关系展示画面)

### 更新样式的三种方式

#### JS / CSS > 样式 > 布局 > 绘制 > 合成

![img](https://user-gold-cdn.xitu.io/2020/5/4/171dde4a96e82fe1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- 根据浏览器的渲染原理，若是开发者**更新了样式**（即元素的几何属性，类似于宽高，位置等）
- 则浏览器会检查所有属性然后重新绘制，最后再合成。

#### JS / CSS > 样式 > 绘制 > 合成

![img](https://user-gold-cdn.xitu.io/2020/5/4/171dde4fe149e0a2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- 如果开发者只是更新了`paint only`的属性（例如背景，文字颜色等）
- 由于不影响页面布局，则浏览器直接执行绘制。

#### JS / CSS > 样式 > 合成

![img](https://user-gold-cdn.xitu.io/2020/5/4/171dde5405781ebe?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- 在开发者只是更改**一个既不更改布局也不需要绘制的属性时**，
- 浏览器将只执行合成，例如动画和`transform`。
