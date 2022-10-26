---
title: vscode
date: 2020-09-08 16:50:06
tags: 
- vscode
- 配置
---

# vscode

> [Visual Studio Code Extension API](https://code.visualstudio.com/api/references/extension-guidelines)

## Launch

> vscode 调试 react 项目

```json
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://localhost:3000",
            "webRoot": "${workspaceRoot}"
        }
    ]
}
```

## 修改快捷键

| 快捷键         | 描述     |
| -------------- | -------- |
| ctrl + alt + s | 触发建议 |
| alt + ,        | 展开折叠 |
| alt + .        | 折叠代码 |
|                |          |

## 窗口布局

| 快捷键                | 描述                 |
| --------------------- | -------------------- |
| `ctrl + shift + b` | 活动栏的显隐(自定义) |
| <code>ctrl + `</code> | 缩略图显隐(自定义) |
| `ctrl + shift + f` | 全局搜索 |
| `ctrl + shift + e` | 定位到文件树 |
| `ctrl + shift + x` | 打开插件(拓展) |
| `ctrl + \` | 分屏打开当前文件 |

## 光标

| 快捷键                | 描述                 |
| --------------------- | -------------------- |
| `ctrl + 0`            | 将光标移动到文件树   |
| `alt + [数字]`        | 切换当前窗口对应文件 |
| `ctrl + [数字(非零)]` | 切换分屏             |
|`ctrl + shift + >`| 通过大纲进行跳转 |
| | |
| | |

## 文件树

> 添加

| 快捷键                | 描述                 |
| --------------------- | -------------------- |
| `ctrl + shift + e` | 定位到文件树 |
| `h | j` | 文件树 |
| `a` | 新建文件 |
| `A` | 新建文件夹 |
| `o` | 打开文件 |
| `ctrl + 1` | 分屏打开文件 |

## 添加代码块

> 文件/首选项/用户片段

```json
{
 "React Component":{
    "prefix": "rc",    
    "body": [
   "import React, { Component } from 'react'",
   "",
   "class Index extends Component {",
   "\trender() {",
   "\t\treturn (",
   "\t\t\t<div>",
   "\t\t\t\tIndex",
   "\t\t\t</div>",
   "\t\t)",
   "\t}",
   "}",
   "",
   "export default Index;",
  ],
  "description": "react component"   
  },
  "React function" : {
   "prefix": "rf",    
   "body": [
    "import React, { Component } from 'react'",
    "",
    "function Index() {",
    "\treturn (",
    "\t\t<div>",
    "\t\t\tIndex",
    "\t\t</div>",
    "\t)",
    "}",
    "",
    "export default Index;",
   ],
   "description": "react component"   
   },
   "React const " : {
    "prefix": "rco",    
    "body": [
     "import React, { Component } from 'react'",
     "",
     "const Index = () => {",
     "\treturn (",
     "\t\t<div>",
     "\t\t\tIndex",
     "\t\t</div>",
     "\t)",
     "}",
     "",
     "export default Index;",
    ],
    "description": "react component"   
    }

}
```

## 快捷键

### 主命令框

| 快捷键                 | 描述                                           |
| ---------------------- | ---------------------------------------------- |
| `F1` 或 `ctrl + shift + p` |  `ctrl+J`|  ctrl+ \` | 打开命令面板                                   |
| `ctrl + p`             | 下拉菜单<br>再输入`>`进入ctrl + shift + p 模式 |
|`ctrl + k, ctrl + 0[数字]`|折叠所有代码块|
|`ctrl + k, ctrl + j`|展示所有代码块|
|`ctrl + k, ctrl + f`|自动格式代码|

### 边界器与窗口管理

| 快捷键           | 描述                             |
| ---------------- | -------------------------------- |
| ctrl  + n        | 新建文件                         |
| ctrl + tap       | 文件之间切换                     |
| ctrl + shift + n | 打开一个新的vscode编辑器         |
| ctrl + w         | 关闭当前窗口                     |
| ctrl + shift + w | 关闭当前vscode编辑器             |
| ctrl + \         | 切出一个新的编辑器窗口(最多三个) |
| ctrl + 1/2/3     | 切换到3个便捷窗口                |

### 代码编辑

| 快捷键                                           | 描述                 |
| ------------------------------------------------ | -------------------- |
| shift + alt + f                                  | 代码格式化           |
| alt + up 或 alt + down                           | 向上或向下移动一行   |
| shift + alt + up 或 shift + alt + down           | 向上或向下复制一行   |
| ctrl + enter                                     | 在下方插入一行       |
| ctrl + shift + enter                             | 在前一行插入一行     |
| alt + shift + 鼠标 左键  或  Ctrl+ alt + down/up | 多行编辑             |
| Shift+Alt+Right 和 Shift+Alt+Left                | 扩展/缩小选取范围：  |
| ctrl + delete                                    | 删除光标后面内容     |
| Ctrl + u                                         | 回退上一个光标的操作 |
| Ctrl + h                                         | 查找替换             |

### 显示相关

| 快捷键            | 描述           |
| ----------------- | -------------- |
| ctrl + b          | 侧栏显示或隐藏 |
| ctrl + shift  + d | 显示 debug     |
| ctrl + shift + u  | 显示 output    |

## 修改默认配置

### 自动保存

`file -> autoSave`或Ctrl + shift + p ,输入auto

### 修改默认快捷键

打开默认键盘快捷方式设置：**File** -> **Preferences** -> **Keyboard Shortcuts**( 中文界面时：“**文件**”->"**首选项**"->"**键盘快捷方式**")，或者：Alt+F -> p -> k -> Enter，进入后如下图一所示。

## 修改上下左右快捷键

```js
// 将键绑定放在此文件中以覆盖默认值
[
  // ctrl+shift+/多行注释
  {
    "key": "ctrl+shift+/",
    "command": "editor.action.blockComment",
    "when": "editorTextFocus"
  },
  // 上
  {
    "key": "alt+k",
    "command": "cursorUp",
    "when": "textInputFocus"
  },
  {
    "key": "up",
    "command": "cursorUp",
    "when": "textInputFocus"
  },
  // 下
  {
    "key": "alt+j",
    "command": "cursorDown",
    "when": "textInputFocus"
  },
  {
    "key": "down",
    "command": "cursorDown",
    "when": "textInputFocus"
  },

  // 左
  {
    "key": "alt+h",
    "command": "cursorLeft",
    "when": "textInputFocus"
  },
  {
    "key": "left",
    "command": "cursorLeft",
    "when": "textInputFocus"
  },
  // 右
  {
    "key": "alt+l",
    "command": "cursorRight",
    "when": "textInputFocus"
  },
  {
    "key": "right",
    "command": "cursorRight",
    "when": "textInputFocus"
  },

  {
    "key": "down",
    "command": "list.focusDown",
    "when": "listFocus && !inputFocus"
  },
  {
    "key": "down",
    "command": "selectNextSuggestion",
    "when": "suggestWidgetMultipleSuggestions && suggestWidgetVisible && textInputFocus"
  },
  {
    "key": "alt+k",
    "command": "selectNextSuggestion",
    "when": "suggestWidgetMultipleSuggestions && suggestWidgetVisible && textInputFocus"
  },
  {
    "key": "up",
    "command": "selectPrevSuggestion",
    "when": "suggestWidgetMultipleSuggestions && suggestWidgetVisible && textInputFocus"
  },
 k {
    "key": "alt+i",
    "command": "selectPrevSuggestion",
    "when": "suggestWidgetMultipleSuggestions && suggestWidgetVisible && textInputFocus"
  },
]
```

## 插件

### React 快速生成模板插件

#### VS Code ES7 React/Redux/React-Native/JS snippets

> `CTRL + ALT + R` : 查看快捷键
> [*Windows*](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf): `Ctrl+P`  然后输入 `ext install dsznajder.es7-react-js-snippets`

#### React

| jPrefix | Method                                                       |
| :------ | :----------------------------------------------------------- |
| `cwm→`  | `componentWillMount = () => { }` DEPRECATED!!!               |
| `cdm→`  | `componentDidMount = () => { }`                              |
| `cwr→`  | `componentWillReceiveProps = (nextProps) => { }` DEPRECATED!!! |
| `scu→`  | `shouldComponentUpdate = (nextProps, nextState) => { }`      |
| `cwup→` | `componentWillUpdate = (nextProps, nextState) => { }` DEPRECATED!!! |
| `cdup→` | `componentDidUpdate = (prevProps, prevState) => { }`         |
| `cwun→` | `componentWillUnmount = () => { }`                           |

#### PropTypes

> 格式要求 :`[A].[B][C]`
>
> - `A` : `pt (PropTypes)`
> - `B` : `a (array)` | `b (bool)` | `f (func)`| `n(number)` | `o(object)` | `s(string)`| `nd(node)` | `el(element)` | `i (instaneof(name))` | `e(oneof([name]))` | `et(oneOfType([name]))` | `ao(arrayOf(name))` | `oo(objectOf(name))` | `sh(shape({}))` | `any(any)`
> - `C` :`r: isRequired` (非必要)
>
> 特例:`ptypes :static propTypes = {}`

| Prefix    | Method                                   |
| :-------- | :--------------------------------------- |
| `pta→`    | `PropTypes.array`                        |
| `ptar→`   | `PropTypes.array.isRequired`             |
| `ptb→`    | `PropTypes.bool`                         |
| `ptbr→`   | `PropTypes.bool.isRequired`              |
| `ptf→`    | `PropTypes.func`                         |
| `ptfr→`   | `PropTypes.func.isRequired`              |
| `ptn→`    | `PropTypes.number`                       |
| `ptnr→`   | `PropTypes.number.isRequired`            |
| `pto→`    | `PropTypes.object`                       |
| `ptor→`   | `PropTypes.object.isRequired`            |
| `pts→`    | `PropTypes.string`                       |
| `ptsr→`   | `PropTypes.string.isRequired`            |
| `ptnd→`   | `PropTypes.node`                         |
| `ptndr→`  | `PropTypes.node.isRequired`              |
| `ptel→`   | `PropTypes.element`                      |
| `ptelr→`  | `PropTypes.element.isRequired`           |
| `pti→`k   | `PropTypes.instanceOf(name)`             |
| `ptir→`   | `PropTypes.instanceOf(name).isRequired`  |
| `pte→`    | `PropTypes.oneOf([name])`                |
| `pter→`   | `PropTypes.oneOf([name]).isRequired`     |
| `ptet→`   | `PropTypes.oneOfType([name])`            |
| `ptetr→`  | `PropTypes.oneOfType([name]).isRequired` |
| `ptao→`   | `PropTypes.arrayOf(name)`                |
| `ptaor→`  | `PropTypes.arrayOf(name).isRequired`     |
| `ptoo→`   | `PropTypes.objectOf(name)`               |
| `ptoor→`  | `PropTypes.objectOf(name).isRequired`    |
| `ptsh→`   | `PropTypes.shape({ })`                   |
| `ptshr→`  | `PropTypes.shape({ }).isRequired`        |
| `ptany→`  | `PropTypes.any`                          |
| `ptypes→` | `static propTypes = {}`                  |

#### Console

| Prefix | Method                              |
| :----- | :---------------------------------- |
| `clg→` | `console.log(object)`               |
| `clo→` | `console.log("object", object)`     |
| `ctm→` | `console.time("timeId")`            |
| `cte→` | `console.timeEnd("timeId")`         |
| `cer→` | `console.error(object)`             |
| `cwa→` | `console.warn`                      |
| `cin→` | `console.info`                      |

#### React Components

> - 第一个字母:
>
> - `r` : react 项目
>
> - 第二个字母:
>   - `f` : function组件
>   - `c`: class组件
> - `af` : const 定义组件
>
> - 第三个字母
>
>   - `c` : 定义处导出
> - `e` : 结尾处导出
>
> - 第四个字母:
>
>   - `p` : 添加``PropTypes`组件
>
>

##### `rcc`

```javascript
import React, { Component } from 'react'

export default class FileName extends Component {
  render() {
    return <div>$2</div>
  }
}
```

##### `rce`

```javascript
import React, { Component } from 'react'

export class FileName extends Component {
  render() {
    return <div>$2</div>
  }
}

export default $1
```

##### `rcep`

```javascript
import React, { Component } from 'react'
import PropTypes from 'prop-types'

export class FileName extends Component {
  static propTypes = {}

  render() {
    return <div>$2</div>
  }
}

export default $1
```

##### `rccp`

```javascript
import React, { Component } from 'react'
import PropTypes from 'prop-types'

export default class FileName extends Component {
  static propTypes = {
    $2: $3,
  }

  render() {
    return <div>$4</div>
  }
}
```

##### `rfcp`

```javascript
import React from 'react'
import PropTypes from 'prop-types'

function $1(props) {
  return <div>$0</div>
}

$1.propTypes = {}

export default $1
```

##### `rfc`

```javascript
import React from 'react'

export default function $1() {
  return <div>$0</div>
}
```

##### `rfce`

```javascript
import React from 'react'

function $1() {
  return <div>$0</div>
}

export default $1
```

##### `rafcp`

```javascript
import React from 'react'
import PropTypes from 'prop-types'

const $1 = (props) => {
  return <div>$0</div>
}

$1.propTypes = {}

export default $1
```

##### `rafc`

```javascript
import React from 'react'

export const $1 = () => {
  return <div>$0</div>
}
```

##### `rafce`

```javascript
import React from 'react'

const $1 = () => {
  return <div>$0</div>
}

export default $1
```

### vscode&vim

> vim 提供了 3 个基本模式：normal ， insert ,  visual

![](https://upload-images.jianshu.io/upload_images/66696-5bc30a92ae0f73a6.gif?imageMogr2/auto-orient/strip|imageView2/2/w/1024/format/webp)
