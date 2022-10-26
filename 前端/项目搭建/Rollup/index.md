# Roadhog

> [roadhog 介绍 - echo丶若梦 - 博客园 (cnblogs.com)](https://www.cnblogs.com/gaoht/p/9400386.html)

## 安装

`npm install rollup -g`

## 插件

### babel

#### rollup-plugin-babel

> 使用未被浏览器或node.js支持的js特性

```js
// rollup.config.js
import resolve from 'rollup-plugin-node-resolve';
import babel from 'rollup-plugin-babel';

export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  },
  plugins: [
    resolve(),
    babel({
      exclude: 'node_modules/**' // 只编译我们的源代码
    })
  ]
};
```

```js
// .babelrc
{
  "presets": [
    ["latest", {
      "es2015": {
        "modules": false
      }
    }]
  ],
  "plugins": ["external-helpers"]
}

```

### the-answer

> 将commonjs 装换为ES2015

### rollup-plugin-commonjs​

> 导入es6模块

### rollup-plugin-node-resolve​

> 告诉Rollup如何查找外部模块

```js
// rollup.config.js
import resolve from 'rollup-plugin-node-resolve';

export default {
  input: 'src/main.js',
  output: {
    file: 'bundle.js',
    format: 'cjs'
  },
  plugins: [resolve({
    // 将自定义选项传递给解析插件
    customResolveOptions: {
      moduleDirectory: 'node_modules'
    }
  })],
  // 指出应将哪些模块视为外部模块
  external: ['lodash']
};
```

### json相关

> 解析json文件
> `rollup-plugin-json`
> `@rollup/plugin-json` (推荐)

```js
import json from "rollup-plugin-json"
// 或
import json from "@rollup/plugin-json"

export default {
 // ...
 plugins:[json()]
 // 或
 // JSON文件可能会变得很大，您可能不需要所有内容。您可以将参数 compact 作为 传递，
 // 它将生成最少量的代码rue
 plugins:[ json({ compact: true })]
}
```

## 配置文件

```js
// rollup.config.js
export default {
  // 核心选项
  input,     // 必须
  external,
  plugins,

  // 额外选项
  onwarn,

  // danger zone
  acorn,
  context,
  moduleContext,
  legacy

  output: {  // 必须 (如果要输出多个，可以是一个数组)
    // 核心选项
    file,    // 必须
    format,  // 必须
    name,
    globals,

    // 额外选项
    paths,
    banner,
    footer,
    intro,
    outro,
    sourcemap,
    sourcemapFile,
    interop,

    // 高危选项
    exports,
    amd,
    indent
    strict
  },
};
```

### 使用配置文件

> `--config` 简写 `-c`

```shell
# 默认使用rollup.config.js

$ rollup --config

# 或者, 使用自定义的配置文件，这里使用my.config.js作为配置文件

$ rollup --config my.config.js
```

## rollup.watch

> 检测某个模块已经改变, 就会重新构建文件束
> `--watch` : 此函数会被内部使用

```js
const rollup = require('rollup');

const watchOptions = {...};
const watcher = rollup.watch(watchOptions);

watcher.on('event', event => {
  // event.code 会是下面其中一个：
  //   START        — 监听器正在启动（重启）
  //   BUNDLE_START — 构建单个文件束
  //   BUNDLE_END   — 完成文件束构建
  //   END          — 完成所有文件束构建
  //   ERROR        — 构建时遇到错误
  //   FATAL        — 遇到无可修复的错误
});

// 停止监听
watcher.close();
```

## 选项表

### 监听参数 (watchOptions)
>
> - 输入(input -i/--input)​
> - String 这个包的入口点 (例如：你的 main.js 或者 app.js 或者 index.js)
>
> - 文件(file -o/--output.file)​
> - String 要写入的文件。也可用于生成 sourcemaps，如果适用
>
> - 格式(format -f/--output.format)​
> - String 生成包的格式。 下列之一:
>   - amd – 异步模块定义，用于像RequireJS这样的模块加载器
>   - cjs – CommonJS，适用于 Node 和 Browserify/Webpack
>   - esm – 将软件包保存为 ES 模块文件，在现代浏览器中可以通过 `<script type=module>` 标签引入
>   - iife – 一个自动执行的功能，适合作为`<script>`标签。（如果要为应用程序创建一个捆绑包，您可能想要使用它，因为它会使文件大小变小。）
>   - umd – 通用模块定义，以amd，cjs 和 iife 为一体
>   - system - SystemJS 加载器格式
>
### 生成包名称(name -n/--name)​

String 变量名，代表你的 iife/umd 包，同一页上的其他脚本可以访问它。

```js
// rollup.config.js
export default {
  ...,
  output: {
    file: 'bundle.js',
    format: 'iife',
    name: 'MyBundle'
  }
};

// -> var MyBundle = (function () {...
```

### 插件(plugins)​

> 插件对象 数组 Array (或一个插件对象) – 有关详细信息请参阅 插件入门。记住要调用导入的插件函数(即 commonjs(), 而不是 commonjs).

```js
// rollup.config.js
import resolve from 'rollup-plugin-node-resolve';
import commonjs from 'rollup-plugin-commonjs';

export default {
  entry: 'main.js',
  plugins: [
    resolve(),
    commonjs()
  ]
};
```

### 外链(external -e/--external)​

> 两者任一 Function 需要一个 id 并返回 true（外部引用）或 false（不是外部的引用）， 或者 Array 应该保留在bundle的外部引用的模块ID。ID应该是：

外部依赖的名称
一个已被找到路径的ID（像文件的绝对路径）

```js
// rollup.config.js
import path from 'path';

export default {
  ...,
  external: [
    'some-externally-required-library',
    path.resolve( './src/some-local-file-that-should-not-be-bundled.js' )
  ]
};

```

当作为命令行参数给出时，它应该是以逗号分隔的ID列表：

`rollup -i src/main.js ... -e foo,bar,baz`

全局模块(globals -g/--globals)​
Object 形式的 id: name 键值对，用于umd/iife包。例如：在这样的情况下...

```js

import $ from 'jquery';

// ...我们想告诉 Rollup jquery 模块的id等同于 $ 变量:

// rollup.config.js
export default {
  ...,
  format: 'iife',
  name: 'MyBundle',
  globals: {
    jquery: '$'
  }
};

/*
var MyBundle = (function ($) {
  // 代码到这里
}(window.jQuery));
*/.
```

或者，提供将外部模块ID转换为全局模块的功能。

当作为命令行参数给出时，它应该是一个逗号分隔的“id：name”键值对列表：

`rollup -i src/main.js ... -g jquery:$,underscore:_`

```js
const watchOptions = {
  ...inputOptions,
  output: [outputOptions],
  watch: {
    chokidar,
    include,
    exclude
  }
};
```

### 路径(paths)

> Function，它获取一个ID并返回一个路径，或者id：path对的Object。
> 在提供的位置，这些路径将被用于生成的包而不是模块ID，从而允许您（例如）从CDN加载依赖关系：

```js
// app.js
import { selectAll } from 'd3';
selectAll('p').style('color', 'purple');
// ...

// rollup.config.js
export default {
  input: 'app.js',
  external: ['d3'],
  output: {
    file: 'bundle.js',
    format: 'amd',
    paths: {
      d3: 'https://d3js.org/d3.v4.min'
    }
  }
};

// bundle.js
define(['https://d3js.org/d3.v4.min'], function (d3) {

  d3.selectAll('p').style('color', 'purple');
  // ...

});
```

### intro/outro​

> String类似于 banner和footer，除了代码在内部任何特定格式的包装器(wrapper)

```js
export default {
  ...,
  intro: 'var ENVIRONMENT = "production";'
};
```

### 缓存(cache)​

> Object 以前生成的包。使用它来加速后续的构建——Rollup只会重新分析已经更改的模块。

### onwarn​

Function 将拦截警告信息。如果没有提供，警告将被复制并打印到控制台。

警告是至少有一个code 和 message属性的对象，这意味着您可以控制如何处理不同类型的警告：

```js

onwarn (warning) {
  // 跳过某些警告
  if (warning.code === 'UNUSED_EXTERNAL_IMPORT') return;

  // 抛出异常
  if (warning.code === 'NON_EXISTENT_EXPORT') throw new Error(warning.message);

  // 控制台打印一切警告
  console.warn(warning.message);
}
```

许多警告也有一个loc属性和一个frame，你可以定位到警告的来源：

```js

onwarn ({ loc, frame, message }) {
  // 打印位置（如果适用）
  if (loc) {
    console.warn(`${loc.file} (${loc.line}:${loc.column}) ${message}`);
    if (frame) console.warn(frame);
  } else {
    console.warn(message);
  }
}
```

### sourcemap -m/--sourcemap​

> 如果 true，将创建一个单独的sourcemap文件。如果 inline，sourcemap将作为数据URI附加到生成的output文件中。

### sourcemapFile​

> String生成的包的位置。如果这是一个绝对路径，sourcemap中的所有源代码路径都将相对于它。 map.file属性是sourcemapFile的基本名称(basename)，因为sourcemap的位置被假定为与bundle相邻
> 如果指定 output，sourcemapFile 不是必需的，在这种情况下，将通过给bundle输出文件添加 “.map” 后缀来推断输出文件名。

### interop​

> Boolean 是否添加'interop块'。默认情况下（interop：true），为了安全起见，如果需要区分默认和命名导出，则Rollup会将任何外部依赖项“default”导出到一个单独的变量。这通常只适用于您的外部依赖关系（例如与Babel）（如果您确定不需要它），则可以使用“interop：false”来节省几个字节。

### Watch options​

> 这些选项仅在运行 Rollup 时使用 --watch 标志或使用 rollup.watch 时生效。

#### watch.chokidar​

> 一个 Boolean 值表示应该使用 chokidar 而不是内置的 fs.watch，或者是一个传递给 chokidar 的选项对象。
> 如果你希望使用它，你必须单独安装chokidar。

#### watch.include​

> 限制文件监控至某些文件

```js

// rollup.config.js
export default {
  ...,
  watch: {
    include: 'src/**'
  }
};

```

#### watch.exclude​

> 防止文件被监控：

```js


// rollup.config.js
export default {
  ...,
  watch: {
    exclude: 'node_modules/**'
  }
};
```

## 命令行参数

```shell
-i, --input <filename>      要打包的文件（必须）
-o, --file <output>         输出的文件 (如果没有这个参数，则直接输出到控制台)
-f, --format <format>       输出的文件类型 (amd, cjs, esm, iife, umd)
-e, --external <ids>        将模块ID的逗号分隔列表排除
-g, --globals <pairs>       以`module ID:Global` 键值对的形式，用逗号分隔开 
                              任何定义在这里模块ID定义添加到外部依赖
-n, --name <name>           生成UMD模块的名字
-h, --help                  输出 help 信息
-m, --sourcemap             生成 sourcemap (`-m inline` for inline map)
--amd.id                    AMD模块的ID，默认是个匿名函数
--amd.define                使用Function来代替`define`
--no-strict                 在生成的包中省略`"use strict";`
--no-conflict               对于UMD模块来说，给全局变量生成一个无冲突的方法
--intro                     在打包好的文件的块的内部(wrapper内部)的最顶部插入一段内容
--outro                     在打包好的文件的块的内部(wrapper内部)的最底部插入一段内容
--banner                    在打包好的文件的块的外部(wrapper外部)的最顶部插入一段内容
--footer                    在打包好的文件的块的外部(wrapper外部)的最底部插入一段内容
--interop                   包含公共的模块（这个选项是默认添加的）
```
