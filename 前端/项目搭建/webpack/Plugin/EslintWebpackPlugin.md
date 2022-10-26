# EslintWebpackPlugin

> eslint-webpack-plugin 3.0 仅支持 webpack 5。对于 webpack 4 请查看 2.x 分支。
>
> 该插件使用 eslint 来查找和修复 JavaScript 代码中的问题。

## 开始

> 首先，需要安装 eslint-webpack-plugin：

`npm install eslint-webpack-plugin --save-dev`
> 注意: 如果未安装 eslint >= 7 ，你还需先通过 npm 安装：

`npm install eslint --save-dev`

然后把插件添加到你的 webpack 配置。例如：

```js

const ESLintPlugin = require('eslint-webpack-plugin');

module.exports = {
  // ...
  plugins: [new ESLintPlugin(options)],
  // ...
};
```

## 选项

> 你可以传入 eslint 参数。
> 注意你提供的配置选项会传给 ESLint 类。 这是一组和你在 package.json 与 .eslintrc 所指定选项不同的选项。 查阅 eslint 文档 获取更多详情。
> 警告: 在 eslint-webpack-plugin 1.x 版本中，配置项会传递给已废弃的 CLIEngine。

### context

> 类型：String
> 默认值：compiler.context
> 指定文件根目录，类型为字符串。

### eslintPath

> 类型：String
> 默认值：eslint
> 用于 linting 的 eslint 实例的路径。如果 eslintPath 是类似官方 eslint 的目录，或者指定了 formatter 选项，那么就不需要安装 eslint 了。

### extensions

> 类型：String|Array[String]
> 默认值：'js'
> 指定需要检查的扩展名。

### exclude

> 类型：String|Array[String]
> 默认值：'node_modules'
> 指定需要排除的文件及目录。必须是相对于 options.context 的相对路径。

### files

> 类型：String|Array[String]
> 默认值：null
> 指定目录、文件或 globs ，必须是相对于 options.context 的相对路径。 如果是目录则递归查找所有匹配 options.extensions 选项的文件。 如果是文件或 globs 则忽略 options.extensions 选项。

### fix

> 类型：Boolean
> 默认值：false
> 启用 ESLint 自动修复特性。

小心: 该选项会修改源文件。

### formatter

> 类型：String|Function
> 默认值：'stylish'
> 接受一个有单一参数的函数：该参数为 eslint 消息（一个对象）的数组。函数必须返回字符串格式的 output。可以使用官方的 eslint formatters。

### lintDirtyModulesOnly

> 类型：Boolean
> 默认值：false
> 只对内容修改了的文件进行 lint，启动时跳过 lint 。

### threads

> 类型：Boolean | Number
> 默认值：false
> 以线程池方式运行 lint 。线程池大小是自动的，除非你指定一个数值。

错误以及警告
该插件默认会根据 eslint 错误/警告的数量自动调整错误报告（error reporting）。 你也可以通过 emitError 或 emitWarning 强制开启错误报告行为：

### emitError

> 类型：Boolean
> 默认值：true
> 总是发送发现的错误 ，设置为 false 以禁用。

### emitWarning

> 类型：Boolean
> 默认值：true
> 总是发送发现的警告，设置为 false 以禁用。

### failOnError

> 类型：Boolean
> 默认值：true
> 任何错误都会导致模块构建（module build）失败，设置为 false 禁用。

### failOnWarning

> 类型：Boolean
> 默认值：false
> 当设置为 true 时，任何警告都会导致模块构建（module build）失败。

### quiet

> 类型：Boolean
> 默认值：false
> 设置为 true 后，仅处理和报告错误，忽略警告。

### outputReport

> 类型：Boolean|Object
> 默认值：false
> 把错误输出到一个文件，例如在 Jenkins CI 使用 checkstyle xml 文件。

filePath 为绝对路径或者相对于 webpack 配置: output.path 的相对路径。 你可以为输出文件传入不同的 formatter 。 如果没有传入，则使用默认的或已配置的 formatter 。
