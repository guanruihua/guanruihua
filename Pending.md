# Pending

## 地图相关

<https://blog.csdn.net/qq_42683219/article/details/106390151>

## 待处理

> [canvas转图片下载](front-end\html\canvas\Demo\canvas转图片下载.md)

## 组件

> - 滑动登录

## 二维码

<https://cloud.tencent.com/developer/article/1010480>

<https://juejin.cn/post/6900150859171430413>

## 待开始

seo优化 学习, 结合多语言优化
判断元素出现在可视区域中的方案
offsetTop、scrollTop
getBoundingClientRect
IntersectionObserve
> [Cookie](https://juejin.cn/post/6844904102544031757)
> [大文件断点切片](juejin.cn/post/7080527713399750692)
><https://juejin.cn/post/7085654890671767559#heading-9>
> SSR(服务端渲染框架) Remix
>navigator.geolocation.getCurrentPosition
>
> - github action
>
> 地图
>
> - [GeoJSON Maps of the globe](https://geojson-maps.ash.ms/)
> - [yanivam/react-svg-worldmap: A simple, compact and free React SVG world map.](https://github.com/yanivam/react-svg-worldmap)
> - [react、echarts和地图的结合](https://www.jianshu.com/p/29b346e282ac)
><https://flutter.cn/docs/development/ui/layout>

 ```js
data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAJAAAACQAQMAAADdiHD7AAAABlBMVEUAAABTU1OoaSf/AAAAAXRSTlMAQObYZgAAAFJJREFUeF7t0cENgDAMQ9FwYgxG6WjpaIzCCAxQxVggFuDiCvlLOeRdHR9yzjncHVoq3npu+wQUrUuJHylSTmBaespJyJQoObUeyxDQb3bEm5Au81c0pSCD8HYAAAAASUVORK5CYII
 ```

## 计划中

### npm包

#### 组件

> 动态行数textarea

#### 常用Icon

> 通过书写好的svg进行使用

#### 配置生成模板

> - 通过模板生成以及配置, 生成目标文件

### 笔记预览项目
>
> - 将笔记文件进行直接复制`copy-webpack-plugin`, 不用进行打包
> - 文件夹: 为文件tab, 文件头部(或全局)可添加 忽略的名称不加入tab中
> - 先使用node脚本生成目录结构, 再执行webpack脚本

### 数据预览项目

> 支持端: 微信小程序, android, PC(window)
> 通过爬取公众号, 公开网页数据等信息, 进行汇总
> 数据更新频率: 一天更新一次

### 游戏

> 名字: 寄生世界
> 数据基于现实世界数据

### 其他代码

```ts
/**
 * @title getWordAndNumber
 * @description  将提取字母和数字
 * @param str 待处理 字符串
 * @returns RegExpMatchArray( string[] | [] ) | null
 */
export function getWordAndNumber(str: string): RegExpMatchArray | null {
 return str.match(/[a-zA-Z]+|[0-9]+/g)
}
```
