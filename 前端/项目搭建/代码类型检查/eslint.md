# ESLint

> [rules](http://eslint.cn/docs/rules/)
> <https://juejin.cn/post/6955025103507849223>
>
> - root - 限定配置文件的使用范围
> - parser - 指定eslint的解析器
> - parserOptions - 设置解析器选项
> - extends - 指定eslint规范
> - plugins - 引用第三方的插件
> - env - 指定代码运行的宿主环境
> - rules - 启用额外的规则或覆盖默认的规则
> - globals - 声明在代码中的自定义全局变量

## 取消格式检查

> 单行, 行末 添加 `// eslint-disable-line`
> 单行指定忽略规则, 行末 添加 `// eslint-disable-line [这里添加规则(不用括号)]`
> 单文件, 首行 添加 `/* eslint-disable */`
> 单文件指定忽略规则, 首行 添加 `/* eslint-disable [这里添加规则(不用括号)] */`
