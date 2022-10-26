# NPM包制作

## 前置工作

> 1. 先去<https://www.npmjs.com/signup> 注册一个用户
> 2. 在本地登录好自己的账号`npm login`

### 登录失败

> 指向源出错

```shell
npm login --registry=https://registry.npmjs.org/
```

### 开始

> 1. 创建项目
> 2. 创建``package.json`:`npm init`
> 3. 开始编辑您的组件

### 发布

> 1. 发布`npm publish`
> 2. 然后就可以到官网搜索发布的包

### 添加链接访问功能

```json
{
  "unpkg": "dist/bundle.js",
  "jsdelivr": "dist/bundle.js"
}
```

### npm 上传白名单

> 在``package.json`添加代码

```js
"files":[
 "src",
  "demo",
  "index.js",
]
```

### npm上传黑名单

> 在项目根目录下添加`.npmignore`文件: 定义即使在白名单中, 也会被忽略
>
> 和`gitignore`文件写法类似

```js
/demo
```

### npm包删除和弃用

> - 撤销发布的包( 24 小时内 ): `npm unpublish`
>
> - 弃用特定版本以及版本范围: `npm deprecate <pkg>[@version] <message>`

### 发布问题

> - 必须要有``package.json`, 而且里面的`version` 要有改变
>
> - 手动更改版本号:
>
>   ```bash
>   // version : 1.1.1
>   npm version patch // 第三位+1
>   npm version minor // 第二位+1
>   npm version major // 第一位+1
>   ```
>
>

### 使用

> - `npm install [包名]`
