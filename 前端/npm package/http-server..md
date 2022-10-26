# http-server

> - http-server是一个简单的零配置命令行静态 HTTP 服务器
> - 它足够强大，可以用于生产用途，但它足够简单且易于破解，可用于测试，本地开发和学习

## 安装

`npm install --global http-server`

## 运行

`npx http-server [path] [options]`

## 用法

`http-server [path] [options]`

> [path]如果文件夹存在，则默认为，否则。./public./
>
> 现在，您可以访问 <http://localhost:8080> 查看您的服务器
> 注意：默认情况下，缓存处于打开状态。添加为禁用缓存的选项。-c-1

可用选项：
命令 | 描述| 默认
:---:|:---:|:---:|
-p或--port | 要使用的端口。用于查找从 8080 开始的开放端口。它还将从 中读取。-p 0process.env.PORT  | 8080
-a | 要使用的地址 | 0.0.0.0
-d | 显示目录列表 | true
-i | 显示自动索引 | true
-g或--gzip | 启用后，它将代替文件的 gzip 版本存在并且请求接受 gzip 编码时使用。如果还启用了 brotli，它将尝试先提供 brotli。`./public/some-file.js.gz./public/some-file.js` |false
-b或--brotli | 启用后，它将代替文件的 brotli 压缩版本存在并且请求接受编码的情况。如果 gzip 也启用了，它将尝试先提供 brotli。`./public/some-file.js.br./public/some-file.jsbr` |false
-e或--ext |  默认文件扩展名（如果未提供） | html
-s或--silent | 禁止显示输出中的日志消息
--cors | 通过标头启用 CORSAccess-Control-Allow-Origin
-o [path] | 启动服务器后打开浏览器窗口。（可选）提供要打开的 URL 路径。例如： -o /other/dir/
-c |  为缓存控制最大期限标头设置缓存时间（以秒为单位），例如 持续 10 秒钟。要禁用缓存，请使用 。-c10-c-1 | 3600
-U或--utc |  在日志消息中使用 UTC 时间格式。
--log-ip | 启用客户端 IP 地址的日志记录 | false
-P或--proxy | 将所有无法在本地解析的请求代理到给定的 URL。例如：-P <http://someurl.com>
--proxy-options | 使用嵌套的虚线对象传递代理选项。例如： --proxy-options.secure | false
--username | 用于基本身份验证的用户名
--password | 基本身份验证的密码
-S或--tls--ssl | 启用使用 TLS/SSL （HTTPS） 提供安全请求服务 | false
-C或--cert SSL | 证书文件的路径 | cert.pem
-K或--key SSL | 密钥文件的路径  | key.pem
-r或--robots | 自动提供 /robots.txt（其内容默认为User-agent: *\nDisallow: /) | false
--no-dotfiles | 不显示点文件
--mimetypes | 用于自定义 mimetype 定义的 .types 文件的路径
-h或--help | 打印此列表并退出
-v或--version|  打印版本并退出

### 魔术文件

> index.html将作为任何目录请求的默认文件。
> 404.html如果未找到文件，将送达。这可用于单页应用 （SPA） 托管以服务入口页面。

### 捕获所有重定向

> 要实现无限别名重定向，请使用索引页本身作为代理，并执行以下操作：
> `http-server --proxy <http://localhost:8080>?`
> 请注意代理 URL 末尾的

### 断续器/域名解析

> 首先，您需要确保 openssl 已正确安装，并且您拥有 和 文件。您可以使用以下命令生成它们：key.pemcert.pem
>
> `openssl req -newkey rsa:2048 -new -nodes -x509 -days 3650 -keyout key.pem -out cert.pem`
> 输入命令后，系统将提示您几个问题。如果您希望能够在操作系统的根证书存储或浏览器中安装证书，以便它受信任， 则将其用作 值。127.0.0.1Common name
>
> 这将生成一个证书密钥对，有效期为 3650 天（约 10 年）
> 然后，您需要运行用于启用 SSL 和证书文件的服务器。-S-C
`http-server -S -C cert.pem`
> 如果您希望对私钥使用密码，则可以通过 -passout 参数（使用 foobar 的密码）在 openssl 命令中包含一个密码短语
> 例如openssl req -newkey rsa:2048 -passout pass:foobar -keyout key.pem -x509 -days 365 -out cert.pem
> 出于安全原因，密码短语将仅从环境变量中读取。NODE_HTTP_SERVER_SSL_PASSPHRASE
如果成功，应输出以下内容：

```js
Starting up http-server, serving ./ through https

http-server settings:
CORS: disabled
Cache: 3600 seconds
Connection Timeout: 120 seconds
Directory Listings: visible
AutoIndex: visible
Serve GZIP Files: false
Serve Brotli Files: false
Default File Extension: none

Available on:
  <https://127.0.0.1:8080>
  <https://192.168.1.101:8080>
  <https://192.168.1.104:8080>
Hit CTRL-C to stop the server
```
