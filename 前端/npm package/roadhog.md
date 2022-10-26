# roadhog

> cli工具

## 命令

### server

> 本地开发: `roadhog server`

### build

> 打包发布: `roadhog server`

### test

> 测试: `roadhog server`

## 环境变量

可环境变量临时配置一些参数，包括：

PORT，端口号，默认8000
HOST，默认localhost
HTTPS，是否开启https，默认关闭
BROWSER，设为none 时，不自动打开浏览器
CLEAR_CONSOLE，设为none时清屏

```shell

//OS X, Linux
$ PORT=3000 roadhog server

//Windows
$ set PORT=3000&&roadhog server
```
