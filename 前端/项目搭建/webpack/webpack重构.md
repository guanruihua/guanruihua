---
title: webpack重构
date: 2021-02-12 23:39:46
tags:
- webpack
- 重构
---

# webpack区分生产环境和开发环境

> webpack 的配置一般写在webpack.config.js文件夹中, 但是生产环境的配置和开发环境不一样, 需要将两个环境分开



## cross-env

在node里，我们有一个process对象，它里面包括了node的一些信息，env和它的一个属性，但是并没有process.env.NODE_ENV，这是我们自己添加的一个用来区分环境的变量，我们通过这个来区分生产开发环境。

但是不同电脑上设置的方式是不一样的，所以cross-env就来了，它可以跨平台设置环境和使用环境变量。

我们需要在控制台执行：

```shell
yarn add cross-env -D
```

然后我们在package.json里配置：

```shell
"build": "cross-env NODE_ENV=production webpack",
"dev": "cross-env NODE_ENV=development webpack-dev-server"
```

我们在webpack.config.js里添加：

```shell
const NODE_ENV=process.env.NODE_ENV;
console.log("--------"+NODE_ENV+"-----------");
```

然后去控制台执行，当执行yarn run build时：

![在这里插入图片描述](webpack重构.assets/16903cbe8ec23b4e)

控制台打印出了我们设置的production。vim

执行yarn run dev的时候：

![在这里插入图片描述](webpack重构.assets/16903cbe8ed36e78)

打印出了devlopment，说明我们已经设置完成了。

## webpack-merge

设置了环境之后我们需要将配置分开，我们先在根目录下新建==webpack.config.dev.js==（开发环境），==webpack.config.prod.js==（生产环境），将原本的webpack.config.js修改成==webpack.config.common.js==（公共）。

分离开的环境需要和common里的代码合并使用，所以我们就需要用到webapck-merge插件，我们在控制台执行：

```shell
yarn add webpack-merge -D
```

下载好后先去package.json里修改配置：

```shell
//--config是可以设置我们执行哪个webpack文件，默认是执行webpack.config.js,但是我们现在修改文件名了，所以我们要设置一下
"build": "cross-env NODE_ENV=production webpack --config webpack.config.prod.js",
"dev": "cross-env NODE_ENV=development webpack-dev-server --config webpack.config.dev.js"
```

我们将一些开发环境用到的东西移到==webpack.config.dev.js==里：

```js
const path=require('path');
const webpack=require('webpack');
const merge=require('webpack-merge');//这里引入merge
const common=require('./webpack.config.common.js');//这里引入公共代码

module.exports=merge(common,{//注意这里的写法
    mode:'development',
    devtool:'cheap-module-eval-source-map',
    module:{
        rules:[

        ]
    },
    devServer: {
        contentBase: path.join(__dirname, "dist"),
        compress: true,//开启gzip压缩
        port: 8080,
        open:true,
        hot:true,
        overlay:true,
    },
    plugins:[
        new webpack.HotModuleReplacementPlugin(),
    ]
})
```

生产环境的移到==webpack.config.prod.js==：

```js
const merge=require('webpack-merge');
const webpack=require('webpack');
const common=require('./webpack.config.common.js');
const MiniCssExtractPlugin=require('mini-css-extract-plugin');
const { BundleAnalyzerPlugin } = require('webpack-bundle-analyzer');
const UglifyJsPlugin = require("uglifyjs-webpack-plugin");
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin");
const CleanWebpackPlugin  = require('clean-webpack-plugin');

module.exports=merge(common,{
    mode:'production',
    module:{
        rules:[

        ]
    },
    plugins:[
        new MiniCssExtractPlugin({//提取css
            filename:'css/main.css'
        }),
        new CleanWebpackPlugin('./dist'),//删除dist目录下的文件
        new BundleAnalyzerPlugin({ analyzerPort: 8090 }),

    ]
})
```

然后去==webpack.config.common.js==里将相关代码删除就行了。

这里还有个注意点，在使用MiniCssExtractPlugin.loader的时候是不支持热更新的，所以我们需要根据环境来区分这个，我们在==webpack.config.common.js==里修改一下：

```json
//开发环境使用style-loader
{
	loader:NODE_ENV==="production" ? MiniCssExtractPlugin.loader : "style-loader"
}
```