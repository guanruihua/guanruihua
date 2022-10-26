# 微信小程序

> [小程序开发文档](https://developers.weixin.qq.com/miniprogram/dev/framework/)

## 搭建环境

> 申请AppID
>
> 编辑器 -> 选择小程序 -> 点击 + ;
>
> 自行创建项目 + 填入appid, 不选择云开发, 然后点击新建

### 目录结构

```basic
├─pages
│  ├─index  // 对应每个页面 内部必须包含以下四个文件
│  │      index.js  // 页面逻辑
│  │      index.json // 页面配置
│  │      index.wxml // 页面结构
│  │      index.wxss // 页面样式
│  └─logs
│          logs.js
│          logs.json
│          logs.wxml
│          logs.wxss
└─utils  // 工具类
│      util.js
│  app.js  // 描述小程序整体逻辑
│  app.json  // 小程序公共配置
│  app.wxss  // 小程序公共样式
│  project.config.json // 项目配置文件
│  sitemap.json
/**
* 需要注意的是 page 中4个文件必须同名
**/
```

## 小程序配置

> 小程序根目录下的 `app.json` 文件用来对微信小程序进行**全局配置**

```json
{ 
    // 配置小程序默认启动页
    "entryPagePath": "pages/logs/logs",
    // 对应小程序中的每一个页面,如果没有 entryPagePath, pages中的第一项为启动页
    "pages": [
        "pages/index/index",
        "pages/logs/logs"
    ],
    "window": {
        // 导航栏背景颜色
        "navigationBarBackgroundColor": "#ccc",
        // 导航栏文字颜色 white black
        "navigationBarTextStyle":"white",
        // 导航栏文字内容
        "navigationBarTitleText": "wx",
        // 下拉 loading 样式 dark light; 需要配置下面这个配置才能开启
        "backgroundTextStyle": "dark",
        // 是否开启全局的下拉刷新
        "enablePullDownRefresh": true,
        // 导航栏样式 custom, default; custom以上配置全部失效,只保留右上角胶囊, topBar需要自己写样式定义
        "navigationStyle": "custom"
    },
    "tabBar": {
        // tabBar 文字颜色
        "color": "#444",
        // tabBar 文字选中颜色
        "selectedColor": "#f1f1f1",
        // tabBar 背景颜色
        "backgroundColor": "#fff",
        // tabBar 上边框颜色
        "borderStyle": "#ccc",
        // 最多配置5个 tab, 最少2个
        "list": [
            // 必须在 pages 中存在
            "pagePath": "pages/index/index",
            // tab 名字
            "text": "homePage",
            // 建议大小 81*81 不支持网络图片
            "iconPath": "",
            "selectedIconPath": ""
        ]
    }
}
```

> 每一个小程序页面也可以使用 `.json` 文件对本页面的窗口表现进行配置 页面配置项会覆盖 `app.json` 的 `window` 中相同的配置项
>
> [页面配置](https://developers.weixin.qq.com/miniprogram/dev/reference/configuration/page.html)

## WXML 语法

### 数据绑定

> WXML 中的动态数据均来自对应 Page 的 data ; 数据绑定使用双大括号语法 与 Vue 基本一致 , 但是也有些许不同 , 简单总结一下就是小程序中的 页面里面所有的表达式都应在  \{\{变量\}\}内部去做

```html
<view>{{ message }}</view>
<!-- 绑定动态属性还是要用大括号 -->
<view class="item-{{ id }}"></view>
Page({
    data: {
        message: "hello world",
        id: 123
    }
})
```

### [列表渲染](https://developers.weixin.qq.com/miniprogram/dev/reference/wxml/conditional.html)

```html
<!-- 小程序内部会自动生成 item 和 index -->
<view wx:for="{{ list }}" wx:key="{{ index }}">
    <text>{{ item.uname }} -- {{ item.uage }}</text>
</view>
Page({
    data: {
        list: [
            { uname: 'zs', uage: 23 },
            { uname: 'ls', uage: 24 },
            { uname: 'we', uage: 25 },
            { uname: 'mz', uage: 26 }
        ]
    }
})
```

### 条件渲染

```html
<view wx:if="{{ isShow }}">是否渲染</view>
<view wx:elif="{{ !isShow }}">分支判断</view>
<view wx:else>否则</view>
```

## WXSS

> WXSS (WeiXin Style Sheets)是一套样式语言，用于描述 WXML 的组件样式 ;
>
> WXSS 具有 CSS 的大部分特性 , 为了更适合小程序开发 , 对 CSS 也做了对应的扩充和修改 **尺寸单位(rpx) 样式导入(@import)**

## 生命周期

> 小程序的生命周期分为三种, 分别是 `app` `page` `component` 这里咱们只说一下前面的两种

### App

- `onLaunch` : 监听小程序初始化
- `onShow` : 小程序启动时和后台切回前台的时候被调用
- `onHide` : 小程序切入到后台时调用

```
App({
    onLaunch() {
        console.log('onLaunch...');
    },
    onShow() {
        console.log('onShow...');
    },
    onHide() {
        console.log('onHide...');
    }
})
```

### Page

- `onLoad` : 页面加载时触发 , 一个页面只会调用一次 , 可以在 onLoad 的参数中获取当前打开页面所携带的参数
- `onShow` : 页面显示时和切入前台时被调用
- `onReady` : 页面初次渲染完成时触发只调用一次 类似于 vue 中的 `Mounted`
- `onHide` : 页面隐藏和切入后台时调用, 如`wx.navgationTo` 或 tab 切换
- `onUnload` : 页面卸载时触发 , 如 `wx.redirectTo`或`wx.navigationBack`到其他页面时

```
Page({
    // 默认获取到的是空对象, 如果传递有参数就是参数对象
    onLoad(params) {
        console.log(params);
    },
    onShow() {
        console.log('page--onShow...');
    },
    onReady() {
        console.log('page--onReady...');
    },
    onHide() {
        console.log('page--onHide...');
    },
    onUnload() {
        console.log('page--onUnload...');
    }
})
```

> App 和 Page 之间的顺序为
>
> 启动时 : App`onLaunch` `onShow` Page `onLoad` `onShow` `onReady`
>
> 切后台 : Page `onHide` App `onHide`
>
> 切前台 : App `onShow` Page `onShow`

## 事件

```js
<button type="primary" bindtap="clickMe">click me</button>
Page({
    clickMe(event) {
        console.log(event)
    }
})
```

## 组件

> 个人理解这个组件可以理解为微信又基于一些HTML做了二次封装 ; 这里内容比较多, 但是大部分都不是很难 , 直接放链接吧 , 建议文档反复观看并实践 [戳这里](https://developers.weixin.qq.com/miniprogram/dev/component/)

## API

> 微信提供的接口还是比较多的 这里也不说了 详情看这里 [👉](https://developers.weixin.qq.com/miniprogram/dev/api/base/wx.canIUse.html)

### 路由

> 介绍三个比较个人认为比较常用的 api 详细看这里 [👉](https://developers.weixin.qq.com/miniprogram/dev/api/route/wx.switchTab.html)

```js
Page({
    clickHandle() {
        // 关闭当前页面, 跳转到应用某个页面 不能到 tabBar 页面
        wx.redirectTo({
            url: '/pages/index/index'
        });
        // 保留当前页面, 跳转到应用内某个页面 不能到 tabBar 页面
        wx:navigateTo({
            url: '/pages/index/index'
        });
        // 关闭当前页面, 返回上一级或多级页面
        wx.navigateBack({
            delta: 1 // 数字大小代表回退几个页面
        });
    }
})
```

## 数据设置

> 在小程序中 设置数据一定要使用 `setData` 方法设置

- 直接修改 `this.data` 而不调用 `setData` 是无法改变页面状态的 , 还会造成数据不一致
- 仅支持设置可 JSON 化的数据
- 单次设置的数据不能超过 `1024KB`
- 不要把 `data` 中的任一数据设置为 `undefined` , 可能存在诡异的 **bug**

```js
<input type="text" bindinput="input" /> 
// 双向绑定示例
Page({
 data: {
  value: ''
 },
 input(event) {
  this.setData({
   value: event.detail.value
  });
 }
})
```

## 数据库

> [mongoDB](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/capabilities.html#数据库)

创建了一个 `cloud-demo` 的 collection 并且添加了一些数据 下面我们读取一下试试

![03](https:////p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c2469b00725549a9a1d3c8077aa6cfc9~tplv-k3u1fbpfcp-zoom-1.image)

```js
Page({
    onLoad() {
        // 初始化数据库
        const db = wx.cloud.database();
        db.collection('cloud-demo').get().then(res => console.log(res));
        // 除了 promise 也可以使用 回调函数
        db.collection('cloud-demo').get({
            success: function(res) {
                console.log(res);
            }
        })
    }
})
```

> 这里给大家说一个小坑 ; 有可能你们读取到的是一个空的数组 , 这是因为你创建的 `collection` 有权限问题 ; 大家可以选择对应的 `collection` 然后勾选上方的**数据权限** 然后选择对应的选线即可 ;

## 存储

> 云开发提供一块存储空间 , 提供上传文件到云端 带权限管理的云端下载功能 , 可以使用小程序和云函数通过 API 使用云存储功能 ; 这里我个人理解就像是一个静态资源服务器, 可以用来存储一些图片等等 ;  演示的话就暂时先演示 小程序的上传下载吧

```js
<button type="primary" bindtap="upload">点击上传</button>
Page({
    // 上传文件
    upload() {
        wx.cloud.uploadFile({
            cloudPath: '这是填云控制台中 存储模块 下面的路径',
            filePath: '/images/create-collection.png',
            success(res) {
                console.log(res);
            }
        })
    },
    // 下载文件
    download() {
        wx.downloadFile({
            url: 'https://xxx.png',
            // res 中会包含 tempPath 找了半天没能保存到本地工程中的api 都是保存到手机的尴尬
            success(res) {
                console.log(res)
            }
        })
    }
})
```

## 云函数

> 云函数 (云端运行的函数) 功能独立 , 只需要编写函数代码并部署到云端可在小程序调用 , 同时云函数之间也可以互相调用 ;
>
> 云函数写法与 javascript 写法基本一致 , 代码运行在云端的 Node.js 环境中 , 可以在这个环境中进行网络请求等操作, 还可以通过云函数后端 SDK 搭配使用多种服务 , 比如使用云函数 SDK 中提供的数据库和存储API进行数据和存储的操作;
>
> 云开发的原函数的独特优势在于与微信鉴权的无缝整合 , 当小程序调用云函数时 , 云函数的传入参数中会被注入小程序用户的 openid , 开发者无需校验 openid 的正确性因为微信已经完成了这部分鉴权 , 开发者可以直接使用openid ;

### 云函数简单示例

> 找到项目根目录 , 在根目录上右键 , 右键菜单中选择 新建Node.js 云函数 , 名字符合规范即可 , 然后进入刚才新建的云函数中的 index.js 文件中

- `wx-server-sdk`: 帮助云函数中操作数据库, 存储以及调用其他云函数的微信提供的库
- `event` :  触发云函数的事件, 当小程序调用时 event 就是小程序调用云函数时传入的参数, 外加后端自动注入的小程序用户的openid 和 appid
- `context`:  包含此处的调用信息和运行状态, 可以用来了解服务运行的状态

```js
// 云函数入口文件
const cloud = require('wx-server-sdk')
cloud.init()
// 云函数入口函数
exports.main = async (event, context) => {
  const wxContext = cloud.getWXContext()
  return {
    // 比如这里我们顺便加个小功能
    sum: event.a + event.b,
    event,
    openid: wxContext.OPENID,
    appid: wxContext.APPID,
    unionid: wxContext.UNIONID,
  }
}
```

云函数书写完成后, 一定要记得鼠标右键在云函数目录上选择上传将云函数打包后在进行测试;

> 小程序中调用云函数

```js
Page({
    onShow() {
        wx.cloud.callFunction({
            name: 'testDemo', // 云函数名称, 文件夹名称,
            data: { // 传入的参数 
                a: 1,
                b: 2
            },
            success(res) {
                // 结果集是放到 result 中的
                console.log(res.result);
            }
        })
        // 也可以使用 promise 方式
        wx.cloud.callFunction({
            name: 'testDemo', // 云函数名称, 文件夹名称,
            data: { // 传入的参数 
                a: 1,
                b: 2
            }
        }).then(res => console.log(res.result))
    }
})
```

当然还提供了很多的功能 , 这里就不细说了 大家看文档 比如 **异步返回结果** **使用npm** **使用wx-server-sdk** 大家可以重点先看下 [戳这里](https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/functions.html#云函数)

## todoList Demo

- 实现了 item 的新增 删除 和更新 使用了 云函数的增删改 这里查询就直接写到本地了
- 实现了获取个人信息

> 个人总结 , 感觉小程序写着好不方便, 有点小坑 , 不过还是建议大家看一下, 熟悉一下微信原生开发, 毕竟小程序这个东西就是微信先出来的, 下一步准备总结一些 uni-app 相关内容, 个人感觉这个要好好看看了 毕竟工作中相信大家很少写原生的小程序, 肯定都是基于 mpvue 或者 uni-app 这些类 vue 框架或者类 react 框架去完成的工作 ; 下面看一下效果吧 !  [代码地址 🤞](https://gitee.com/shuqingxuTest/wx-todo-list)

## 项目演示

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f64760e6b7ea463bb762fd6829f3f4ad~tplv-k3u1fbpfcp-watermark.image)
