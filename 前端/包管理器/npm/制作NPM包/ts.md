# 使用TS制作NPM包

## 前置工作

> - `npm  init` 初始化一个`package.json`文件

```json
# package.json

{
  "name": "ts-hi",
  "version": "0.0.1",
  "description": "create npm package with typescript",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/youthcity/ts-hi.git"
  },
  "keywords": [
    "typescript"
  ],
  "author": "youthcity",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/youthcity/ts-hi/issues"
  },
  "homepage": "https://github.com/youthcity/ts-hi#readme"
}
```

## 配置==Typescript==

> - 安装`Typescript`
>
>   - 方法一 : `npm i typescript -D`
>
>   - 方法二 : `yarn add typescript -D`
>
> - 配置`tsconfig.json`
>
>   - 方法一 : 直接自己创建改文件
>
>   - 方法二 :
>     - 全局安装`typescript`包: `npm i typescript -g`
>     - 命令行创建`tsc --init`

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "declaration": true,
    "outDir": "./dist",
    "strict": true
  }
}
```

## 开始制作

> 1. 创建`lib`文件夹
> 2. 创建`index.ts`

```tsx
# 非常简单的加法函数
export function add(a:number, b:number) : number {
  return a + b;
}
```

## 修改==package.json==

```json
{
  "name": "ts-hi",
  "version": "0.0.1",
  "description": "create npm package with typescript",
  "main": "index.js",
  "scripts": {
    "build": "tsc" # 增加 ts 编译命令
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/youthcity/ts-hi.git"
  },
  "keywords": [
    "typescript"
  ],
  "author": "youthcity",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/youthcity/ts-hi/issues"
  },
  "homepage": "https://github.com/youthcity/ts-hi#readme",
  "devDependencies": {
    "typescript": "^3.0.3"
  }
}
```

> 然后运行命令编译: `npm run build`

![image-20210603112139825](ts.assets/image-20210603112139825-1622690502121.png)

## 编写测试

> 安装测试框架和断言库
>
> - `npm i mocha -D`
>
> - `npm i chai -D`
>
> 创建测试文件目录和文件
>
> - `mkdir test`
> - `touch test/test.js`

```js
'use strict';
const expect = require('chai').expect;
const add = require('../dist/index').add;

describe('ts-hi function test', () => {
  it('should return 2', () => {
    const result = add(1, 1);
    expect(result).to.equal(2);
  });
});
```

### 添加测试脚本

```js
  "scripts": {
    "build": "tsc",
    "test": "mocha --reporter spec"
  },
```

> 运行测试脚本`npm run test`

## 提交 和 推送

```shell
touch .gitignore // 创建 .gitignore 文件，并添加 node_modules/ 避免将node_modules 添加到版本控制中
git add . 
git commit -m “Initial release”
git tag v0.1.0  # 修改一下 package.json中的版本号为 0.1.0
git push origin master --tags
```

### 添加提交忽略文件

> 创建`.npmignore`文件

```shell
# 忽略 lib
lib/
```

## 发布

> 1. 在`https://www.npmjs.com/` 注册或登录账号
> 2. `npm adduser`
> 3. `Username: [name]`
> 4. `Password:[pwd]`
> 5. `Email:(this IS public )[填写邮箱]`
> 6. 发布包 :`npm publish`( 已经登录了账号的, 可直接执行这一步 )
