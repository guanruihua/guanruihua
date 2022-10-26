# webpack5 手动搭建React TS项目

## 1. 前言

前段时间突发奇想，使用React这么长时间了，不是使用cra搭建项目就是使用antdpro， 都是基本上帮你配置好了所有的配置项，你只需要写业务代码就好了。所以闲下来的时间打算手动从0开始一步一步搭建一个基于webpack5的React Ts项目，于是就有了下面的实践，由于本人小白一名，所以有做得不对的地方欢迎大佬指正。

## 2. 初始化项目

找一个空白的目录然后执行下面代码:

```bash
mkdir webpack5-react-ts-tempalte
cd webpack5-react-ts-tempalte
npm init -y

```

## 3. 初始化typeScript配置

```bash
npm install --save-dev typescript ts-loader

```

新建一个`tsconfig.json`文件，我这里直接采用的是webpack官网的tsconfig配置， 可以通过查看[TypeScript官方文档](https://link.juejin.cn/?target=https%3A%2F%2Fwww.typescriptlang.org%2Fdocs%2Fhandbook%2Ftsconfig-json.html)了解更多功能。

```json
{
  "compilerOptions": {
    "outDir": "./dist/",
    "noImplicitAny": true,
    "module": "es6",
    "target": "es5",
    "jsx": "react",
    "allowJs": true,
    "moduleResolution": "node",
    "allowSyntheticDefaultImports": true
  }
}

```

## 4.配置webpack相关

```bash
npm i webpack webpack-cli webpack-merge html-webpack-plugin clean-webpack-plugin --dev

```

由于我们的项目基本上都是分为开发环境和生产环境的，所以对于webpack的配置，个人新建了三个文件来存放不同的配置。

新建`config` 文件来存放webpack的配置文件：

- `webpack.config.base.js` 存放webpack的基本配置
- `webpack.config.dev.js` 存放webpack的开发环境配置
- `webpack.config.prod.js` 存放webpack的生产环境配置

```jsx
//  config/webpack.config.base.js
const path = require('path
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
const HtmlWebpackPlugin = require('html-webpack-plugin')
/**
 * @type {import('webpack').Configuration}
 */

module.exports = {
  entry: {
    app: './src/index.tsx',
  },
  output: {
    path: path.resolve(__dirname, '../dist'),
    filename: '[name].[hash].js',
  },
  resolve: {
    extensions: ['.ts', '.tsx', '.js', '.jsx'],
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: '管理后台',
      template: path.resolve(__dirname, '../index.html'),
      filename: 'index.html',
    }),
    new CleanWebpackPlugin(),
  ],
}

```

### 合并配置项

由于我们是分文件配置webpack，所以就会存在合并配置项这个操作。上面安装的`webpack-merge` 就是实现这个功能的插件。

```jsx
//   config/webpack.config.dev.js

const webpackMerge = require('webpack-merge')
const baseConfig = require('./webpack.config.base')
const path = require('path')

/**
 * @type {import('webpack').WebpackOptionsNormalized}
 */
const devServer = {
  port: 3000,
  host: 'localhost',
  contentBase: path.join(__dirname, '../publich'),
  watchContentBase: true,
  publicPath: '/',
  compress: true,
  historyApiFallback: true,
  hot: true,
  clientLogLevel: 'error',
  // open: true,
  watchOptions: {
    ignored: /node_modules/,
  },
}

const devConfig = {
  mode: 'development',
  devServer: devServer,
}

module.exports = webpackMerge.merge(baseConfig, devConfig)

//   config/webpack.config.prod.js

const webpackMerge = require('webpack-merge')
const baseConfig = require('./webpack.config.base')
/**
 * @type {import('webpack').WebpackOptionsNormalized}
 */
const prodConfig = {
  mode: 'production',
}

module.exports = webpackMerge.merge(baseConfig, prodConfig)

```

## 5. 配置babel

安装依赖

```bash
npm i babel-loader babel-plugin-import @babel/cli @babel/core @babel/preset-env @babel/preset-react @babel/preset-typescript --dev

```

在根目录新建`.babelrc` 文件

```json
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}

```

`webpack.config.base.js`中添加mode配置

```jsx
module.exports = {
    ...
    module: {
        rules: [
              { test: /\.(js|jsx)$/, loader: 'babel-loader', exclude: /node_modules/ },
              { test: /\.(ts|tsx)$/, loader: 'ts-loader', exclude: /node_modules/ },
        ],
    },
};

```

## 6. 配置React

### 安装依赖

```bash
npm i react react-dom react-router-dom
npm i @types/react @types/react-dom @types/react-router-dom --dev

```

### 新建html文件

根目录新建`index.html`

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>
    <%= htmlWebpackPlugin.options.title %>
  </title>
</head>

<body>
  <div id="root"></div>
</body>

</html>

```

### 新建工程入口文件

新建`src` 目录下新建`App.tsx`  `index.tsx`

```jsx
// App.tsx

import React from 'react'

const App = () => {
  return <div>1234</div>
}

export default App

// index.tsx
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'
ReactDOM.render(<App />, document.getElementById('root'))

```

## 7. 功能性配置

### 样式文件解析

```bash
npm i style-loader sass-loader sass css-loader postcss-loader postcss-normalize autoprefixer postcss-preset-env -D

```

因为我们平时开发中不只是使用scss，也会使用到scss module，所以同时配置一下，安装依赖

```bash
npm i react-dev-utils resolve-url-loader -D

```

`webpack.config.base.js`中添加mode配置,

```jsx
const getCSSModuleLocalIdent = require('react-dev-utils/getCSSModuleLocalIdent')

module.exports = {
    ...
    module: {
        rules: [
          ...
          {
            test: /\.(css|scss)$/,
            exclude: /\.module\.scss$/,
            use: ['style-loader', 'css-loader', 'postcss-loader', 'sass-loader'],
          },
          {
            test: /\.module\.scss$/,
            use: [
              'style-loader',
              {
                loader: 'css-loader',
                options: {
                  modules: {
                    getLocalIdent: getCSSModuleLocalIdent,
                  },
                },
              },
              'postcss-loader',
              'sass-loader',
            ],
          },
        ],
    },
};


```

新增`postcss.config.js`文件并配置

```jsx
const postcssNormalize = require('postcss-normalize')

module.exports = {
  plugins: [
    [
      'postcss-preset-env',
      {
        autoprefixer: {
          flexbox: 'no-2009',
        },
        stage: 3,
      },
    ],
    postcssNormalize(),
    require('autoprefixer')({
      overrideBrowserslist: ['last 2 version', '>1%', 'ios 7'],
    }),
  ],
}

```

### 图片地址解析

webpack5 内置 assets 类型，我们不需要额外安装插件就可以进行图片等资源文件的解析，配置如下：

```jsx
{
  test: /\.(jpe?g|png|gif|svg|woff|woff2|eot|ttf|otf)$/i,
  type: "asset/resource",
},

```

## 8. 性能优化

webpack5 引入了缓存来提高二次构建速度，我们只需要在 webpack 配置文件中加入如下代码即可开心缓存

```jsx
cache: {
  type: 'filesystem',
  // 可选配置
  buildDependencies: {
    config: [__filename], // 当构建依赖的config文件（通过 require 依赖）内容发生变化时，缓存失效
  },
  name: 'development-cache',
},

```

## 9. 完整配置

### webpack.config.base.js

```jsx
// webpack.config.base.js
const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')
const getCSSModuleLocalIdent = require('react-dev-utils/getCSSModuleLocalIdent')
const { CleanWebpackPlugin } = require('clean-webpack-plugin')
/**
 * @type {import('webpack').Configuration}
 */

module.exports = {
  entry: {
    app: './src/index.tsx',
  },
  output: {
    path: path.resolve(__dirname, '../dist'),
    filename: '[name].[hash].js',
  },
  module: {
    rules: [
      { test: /\.(js|jsx)$/, loader: 'babel-loader', exclude: /node_modules/ },
      { test: /\.(ts|tsx)$/, loader: 'ts-loader', exclude: /node_modules/ },
      {
        test: /\.(css|scss)$/,
        exclude: /\.module\.scss$/,
        use: ['style-loader', 'css-loader', 'postcss-loader', 'sass-loader'],
      },
      {
        test: /\.module\.scss$/,
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: {
              modules: {
                getLocalIdent: getCSSModuleLocalIdent,
              },
            },
          },
          'postcss-loader',
          'sass-loader',
        ],
      },
      {
        test: /\.(jpe?g|png|gif|svg|woff|woff2|eot|ttf|otf)$/i,
        type: 'asset/resource',
      },
    ],
  },
  resolve: {
    extensions: ['.tsx', '.ts', '.js', '.jsx'],
  },
  plugins: [
    new HtmlWebpackPlugin({
      title: '管理后台',
      template: path.resolve(__dirname, '../index.html'),
      filename: 'index.html',
    }),
    new CleanWebpackPlugin(),
  ],
  cache: {
    type: 'filesystem',
    // 可选配置
    buildDependencies: {
      config: [__filename], // 当构建依赖的config文件（通过 require 依赖）内容发生变化时，缓存失效
    },
    name: 'development-cache',
  },
}

```

### webpack.config.dev.js

```jsx
const webpackMerge = require('webpack-merge')
const baseConfig = require('./webpack.config.base')
const path = require('path')

/**
 * @type {import('webpack').WebpackOptionsNormalized}
 */
const devServer = {
  port: 3000,
  host: 'localhost',
  contentBase: path.join(__dirname, '../publich'),
  watchContentBase: true,
  publicPath: '/',
  compress: true,
  historyApiFallback: true,
  hot: true,
  clientLogLevel: 'error',
  // open: true,
  watchOptions: {
    ignored: /node_modules/,
  },
}

const devConfig = {
  mode: 'development',
  devServer: devServer,
}

module.exports = webpackMerge.merge(baseConfig, devConfig)

```

### webpack.config.prod.js

```jsx
const webpackMerge = require('webpack-merge')
const baseConfig = require('./webpack.config.base')
/**
 * @type {import('webpack').WebpackOptionsNormalized}
 */

const prodConfig = {
  mode: 'production',
}

module.exports = webpackMerge.merge(baseConfig, prodConfig)

```

## 10. css提取成单独的打包文件

上述配置中的css是打包到js中的，所以如果想要把css单独打包出来，就需要做一下配置，安装依赖

```bash
npm install --save-dev mini-css-extract-plugin

```

修改`webpack.config.base.js` 中的配置

```jsx
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
	...
  plugins: [new MiniCssExtractPlugin({
              filename: 'css/[name].[hash].css',
	})],
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: [MiniCssExtractPlugin.loader, 'css-loader'],
      },
    ],
  },
}

```

## 11. 配置开发服务器

安装依赖

```bash
npm i webpack-dev-server --dev

```

`package.json` 中添加启动命令

```json
"scripts": {
  
    "start": "webpack serve --config ./config/webpack.config.dev.js",
    "build": "webpack --mode=production --config ./config/webpack.config.prod.js"
  },

```

## 12. 问题解决

当完成上述的步骤的时候，当你运行的时候，可能会收到这么一个报错。

```bash
Cannot find module './index.module.scss' or its corresponding type declarations.

```

因为是ts的项目，所以这个时候需要进行声明文件的书写。

新建`declaration.d.ts`

```tsx
declare module '*.scss' {
  const content: Record<string, string>
  export default content
}

```

## 13. 获取良好的css 代码提示

需要使用到一个插件

```bash
npm install -D typescript-plugin-css-modules

```

配置tsconfig.json

```json
{ 
    "compilerOptions": 
	{ 
            "plugins": [{ "name": "typescript-plugin-css-modules" }]
	 }
}
```

如果你使用的是`vscode`，可以跟我一样配置一下

根目录新建.`vscode`文件夹，然后新建`settings.json`

在文件中写入

```json

{
	"typescript.tsdk": "node_modules/typescript/lib",
	"typescript.enablePromptUseWorkspaceTsdk":true
}

```

从工作区设置`TypeScript`版本，从而读取`tsconfig.json`文件

![Untitled.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/45dfc10b082946dbafc95b0209605933~tplv-k3u1fbpfcp-watermark.image)

然后就可以获得良好的代码提示了