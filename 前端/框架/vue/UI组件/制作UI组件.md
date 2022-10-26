# 开发UI组件

> 参考视频 https://www.bilibili.com/video/BV1nJ411V75n
>
> [hucongcong/heima-uui (github.com)](https://github.com/hucongcong/heima-uui)
>
>  [构建目标 | Vue CLI (vuejs.org)](https://cli.vuejs.org/zh/guide/build-targets.html#库)
>
> 仿制Element-UI组件的文件目录



## 开发准备

> 使用 vue 脚手架创建项目

### 修改文件目录

> 创建
>
> - packages :组件的文件夹
>   - index.js:写成注册packages组件的的方法
> - .npmignore : npm 提交忽略目录 
> - vue.config.js: 修改webpack的打包方法
>
> 修改
>
> - src => examples 

#### packages / index.js

```js
// 整个包的入口
// 定义 install 方法，接收 Vue 作为参数。如果使用 use 注册插件，则所有的组件都将被注册
// 统一导出
// 导入颜色选择器组件
import Button from './button'
...

// 存储组件列表
const components = [
  Button,
  
]
const install = function (Vue) {
  // 全局注册所有的组件
  components.forEach((item) => {
    Vue.component(item.name, item)
  })
}

// 判断是否是直接引入文件,如果是，就不用调用 Vue.use()
if (typeof window !== 'undefined' && window.Vue) {
  install(window.Vue)
}

export default {
  install
}

```



#### .npmignore

```
# 忽略目录 
examples/
packages/
public/

# 忽略指定文件
vue.config.js
babel.config.js
*.map
.gitignore
```



#### vue.config.js

```js
const path = require('path')
module.exports = {
	pages: {
		index: {
			// 修改项目的入口文件
			entry: 'examples/main.js',
			template: 'public/index.html',
			filename: 'index.html'
		}
	},
	// 扩展 webpack 配置，使 packages 加入编译
	chainWebpack: config => {
		config.module
			.rule('js')
			.include.add(path.resolve(__dirname, 'packages')).end()
			.use('babel')
			.loader('babel-loader')
			.tap(options => {
				// 修改它的选项...
				return options
			})
	}
}
```

### 其他修改

#### 添加构建dist指令

```json
{
  ...
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "lib": "vue-cli-service build --target lib packages/index.js"
  }
  ...
}
```



## 发布

```npm
npm login
npm publish
```



## 使用

main.js

```js
import RHUI from 'rh-vue-ui'
import 'rh-vue-ui/dist/rh-vue-ui.css'
```

使用

```vue
<RH-Button>我是按钮</RH-Button>
```



