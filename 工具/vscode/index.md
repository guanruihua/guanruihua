# VSCode 插件开发

> [Your First Extension | Visual Studio Code Extension API](https://code.visualstudio.com/api/get-started/your-first-extension)
>
> `npm install -g yo generator-code`

## 组件开发

```shell
yo code
```

### 选择要开发的类型

```
New Extension(TypeScript) // 基于TS的插件
New Extension(JavaScript) // 基于JS的插件
New Color Theme // 颜色主题
New Language Support // 语言支持
New Code Snippets // 代码片段
New Keymap // 键值映射
New Extension Pack 
New Lanuage Pack(Localization) // 语言包
New Notebook Renderer(TypeScript)
```

### 填写对应信息

1. 插件的名称
2. 插件的id，最终会和你的开发者账号做拼接。比如大帅的开发者账号叫ezshine，最终会拼接为 `ezshine.插件id`
3. 插件的介绍
4. 是否开启类型检查
5. 初始化git仓库（不是非要开启，以后手动开也行）
6. 选择使用npm还是yarn来管理依赖
