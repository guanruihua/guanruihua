# npm

## 常用npm命令

1. `npm -v`：查看npm版本。
2. `npm init`：初始化后会出现一个`package.json`配置文件。可以在后面加上`-y` ，快速跳过问答式界面。
3. `npm install`：会根据项目中的`package.json`文件自动下载项目所需的全部依赖。
4. `npm install 包名 --save-dev`(`npm install 包名 -D`)：安装的包只用于开发环境，不用于生产环境，会出现在`package.json`文件中的`devDependencies`属性中。
5. `npm install 包名 --save`(`npm install 包名 -S`)：安装的包需要发布到生产环境的，会出现在package.json文件中的`dependencies`属性中。
6. `npm list`：查看当前目录下已安装的node包。
7. `npm list -g`：查看全局已经安装过的node包。
8. `npm --help`：查看npm帮助命令。
9. `npm update 包名`：更新指定包。
10. `npm uninstall 包名`：卸载指定包。
11. `npm config list`：查看配置信息。
12. `npm 指定命令 --help`：查看指定命令的帮助。
13. `npm info 指定包名`：查看远程npm上指定包的所有版本信息。
14. `npm config set registry https://registry.npm.taobao.org`： 修改包下载源，此例修改为了淘宝镜像。
15. `npm root`：查看当前包的安装路径。
16. `npm root -g`：查看全局的包的安装路径。
17. `npm ls 包名`：查看本地安装的指定包及版本信息，没有显示empty。
18. `npm ls 包名 -g`：查看全局安装的指定包及版本信息，没有显示empty。

## Config

`npm config ls -l`: 查看一组 npm 内部的配置参数，如果没有指定其他参数，则为默认值。

- `-a`: `--all`
- `--enjoy-by`: `--before`
- `-c`: `--call`
- `--desc`: `--description`
- `-f`: `--force`
- `-g`: `--global`
- `-L`: `--location`
- `-d`: `--loglevel info`
- `-s`: `--loglevel silent`
- `--silent`: `--loglevel silent`
- `--ddd`: `--loglevel silly`
- `--dd`: `--loglevel verbose`
- `--verbose`: `--loglevel verbose`
- `-q`: `--loglevel warn`
- `--quiet`: `--loglevel warn`
- `-l`: `--long`
- `-m`: `--message`
- `--local`: `--no-global`
- `-n`: `--no-yes`
- `--no`: `--no-yes`
- `-p`: `--parseable`
- `--porcelain`: `--parseable`
- `-C`: `--prefix`
- `--readonly`: `--read-only`
- `--reg`: `--registry`
- `-S`: `--save`
- `-B`: `--save-bundle`
- `-D`: `--save-dev`
- `-E`: `--save-exact`
- `-O`: `--save-optional`
- `-P`: `--save-prod`
- `-?`: `--usage`
- `-h`: `--usage`
- `-H`: `--usage`
- `--help`: `--usage`
- `-v`: `--version`
- `-w`: `--workspace`
- `--ws`: `--workspaces`
- `-y`: `--yes`

## 更新安装包

### 手动更新

修改`package.json`中依赖包版本，执行`npm install --force`

### 使用第三方插件

`npm install -g npm-check-updates`
`ncu` // 查看可更新包
`ncu` -u // 更新package.json
`npm install` // 升级到最新版本

### 更新指定包

`npm -g install <name>`

```shell
安装但不写入package.json；
npm install xxx

安装并写入package.json的"dependencies"中
npm install xxx –S 

安装并写入package.json的"devDependencies"中
npm install xxx –D

全局安装
npm install xxx -g

安装指定版本
npm install xxx@1.2.0

更新
先检查更新
npm outdated

执行以上命令，可以看到所有可以更新的模块。
我们需要先更新 package.json文件：
安装"npm-check-updates"模块
npm install -g npm-check-updates

检查可更新的模块
ncu

npm-check-updates

以上两条命令都可检查可更新模块。接下来更新package.json的依赖包到最新版本：
ncu -u

以上命令执行，更新全部模块。但在实际开发中不建议一次全部更新，可以根据实际需要，更新指定的模块，并且可以根据作用范围在后面加上 -D、-S 或 -g
npm update xxx

注意：指定更新需要提前修改 package.json 中的版本号。
为了保险起见，package.json 更新后，可删除整个node_modules目录并重新初始化项目。
npm install

删除
删除指定模块；
npm uninstall xxx 

删除全局模块
npm uninstall -g xxx
```
