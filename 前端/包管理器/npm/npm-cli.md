# npm CLI

## 目录

- 本地模式：npm 将包安装到当前项目目录，默认为当前工作目录。
  - 软件包安装到 `./node_modules`，并将 bin 安装到`./node_modules/.bin`

- 全局模式：npm 将包安装到安装前缀到 `$npm_config_prefix/lib/node_modules`和 `$npm_config_prefix/bin`

## Hooks

- `prepublish`: 在publish该包之前执行。(在包目录下执行npm install时也会执行)
- `postpublish`: 在该包publish之后执行
- `preinstall`: 在该包被install之前执行
- `postinstall`: 在该包被install之后执行
- `preuninstall`: 在该包被uninstall之前执行
- `postuninstall`: 在该包被uninstall之后执行
- `preversion`: 在修改该包的version之前执行
- `postversion`: 在修改该包的version之后执行
- `pretest`, `posttest`: 在该包内执行test时执行，其中pretest先于posttest
- `prestop`, `poststop`: 在该包内执行stop时执行，其中prestop先于poststop
- `prestart`,`poststart`: 在该包内执行start时执行，其中prestart先于poststart

- `prerestart`, `postrestart`: 在该包内执行`restart`脚本时执行，其中prerestart先于postrestart。注意: 如果没有在scripts里显示指定restart脚本，则会自动调用stop，然后再start

- 上面这些Hooks都是npm预定义好的，也就是说，当你执行npm install时，如果你在scripts里定义了preinstall和postinstall，那它们分别会在npm install之前/后自动执行
