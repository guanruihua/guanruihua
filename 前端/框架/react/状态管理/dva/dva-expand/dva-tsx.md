---
title: dva-tsx
data: 2020-12-11 14:33:34
tags:
- dva
- typescript
---

# dva-typescript

> [相关资料](https://github.com/typescript-cheatsheets/react)
>
> [相关资料](https://github.com/zhongsp/TypeScript)

## 环境搭建

全局安装

```shell
npm i typescript -g
```

项目内部安装

```shell
npm i link typescript
npm i --save-dev ts-loader source-map-loader
npm i --save-dev @types/react @types/react-dom
```

根目录下添加tsconfig.json文件

```json
{
  "compilerOptions": {
    "strictNullChecks": true,
    "moduleResolution": "node",
    "allowSyntheticDefaultImports": true,
    "experimentalDecorators": true,
    "jsx": "preserve",
    "noUnusedParameters": true,
    "noUnusedLocals": true,
    "target": "es6",
    "lib": [
      "dom",
      "es7"
    ]
  },
  "exclude": [
    "node_modules",
    "lib",
    "es"
  ]
}

```



demo

```tsx
import * as React from 'react'
import { render } from 'react-dom'

interface IAppProps {}
interface IAppState {}

class App extends React.Component<IAppProps, IAppState> {
  public render(): JSX.Element {
    return (
      <div>
        这是typescript了哦~
      </div>
    )
  }
}

export default App;
```

> 然后直接在路由页面吧该组件展示出去