---
title: koa
date: 2020-10-11 20:22:34
tags:
	- koa
	- plug-in
	- front-end
---

# koa

> nodejs平台web开发框架
>
> [Koa项目基础框架搭建](https://blog.csdn.net/weixin_36554693/article/details/105683996)

##  简介

Koa 是一个新的 web 框架，由 Express 幕后的原班人马打造， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。 通过利用 async 函数，Koa 帮你丢弃回调函数，并有力地增强错误处理。 Koa 并没有捆绑任何中间件， 而是提供了一套优雅的方法，帮助您快速而愉快地编写服务端应用程序。

## 安装

Koa 依赖 **node v7.6.0** 或 ES2015及更高版本和 async 方法支持.

你可以使用自己喜欢的版本管理器快速安装支持的 node 版本：

```bash
$ nvm install 7
$ npm i koa
$ node my-koa-app.js
```

### 使用 Babel 实现 Async 方法

要在 node < 7.6 版本的 Koa 中使用 `async` 方法, 我们推荐使用 [babel's require hook](https://www.babeljs.cn/docs/usage/babel-register/).

```js
require('babel-register');
// 应用的其余 require 需要被放到 hook 后面
const app = require('./app');
```

要解析和编译 async 方法, 你至少应该有 [transform-async-to-generator](https://www.babeljs.cn/docs/plugins/transform-async-to-generator/) 或 [transform-async-to-module-method](https://www.babeljs.cn/docs/plugins/transform-async-to-module-method/) 插件.

例如, 在你的 `.babelrc` 文件中, 你应该有:

```json
{
  "plugins": ["transform-async-to-generator"]
}
```

你也可以用 [env preset](https://www.babeljs.cn/docs/plugins/preset-env/) 的 target 参数 `"node": "current"` 替代.

## 应用程序

Koa 应用程序是一个包含一组中间件函数的对象，它是按照类似堆栈的方式组织和执行的。 Koa 类似于你可能遇到过的许多其他中间件系统，例如 Ruby 的 Rack ，Connect 等，然而，一个关键的设计点是在其低级中间件层中提供高级“语法糖”。 这提高了互操作性，稳健性，并使书写中间件更加愉快。

这包括诸如内容协商，缓存清理，代理支持和重定向等常见任务的方法。 尽管提供了相当多的有用的方法 Koa 仍保持了一个很小的体积，因为没有捆绑中间件。

必修的 hello world 应用:

```js
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

### 级联

Koa 中间件以更传统的方式级联，您可能习惯使用类似的工具 - 之前难以让用户友好地使用 node 的回调。然而，使用 async 功能，我们可以实现 “真实” 的中间件。对比 Connect 的实现，通过一系列功能直接传递控制，直到一个返回，Koa 调用“下游”，然后控制流回“上游”。

下面以 “Hello World” 的响应作为示例，当请求开始时首先请求流通过 `x-response-time` 和 `logging` 中间件，然后继续移交控制给 `response` 中间件。当一个中间件调用 `next()` 则该函数暂停并将控制传递给定义的下一个中间件。当在下游没有更多的中间件执行后，堆栈将展开并且每个中间件恢复执行其上游行为。

```js
const Koa = require('koa');
const app = new Koa();

// logger

app.use(async (ctx, next) => {
  await next();
  const rt = ctx.response.get('X-Response-Time');
  console.log(`${ctx.method} ${ctx.url} - ${rt}`);
});

// x-response-time

app.use(async (ctx, next) => {
  const start = Date.now();
  await next();
  const ms = Date.now() - start;
  ctx.set('X-Response-Time', `${ms}ms`);
});

// response

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);
```

### 设置

应用程序设置是 `app` 实例上的属性，目前支持如下：

- `app.env` 默认是 **NODE_ENV** 或 "development"
- `app.keys` 签名的 cookie 密钥数组
- `app.proxy` 当真正的代理头字段将被信任时
- 忽略 `.subdomains` 的 `app.subdomainOffset` 偏移量，默认为 2
- `app.proxyIpHeader` 代理 ip 消息头, 默认为 `X-Forwarded-For`
- `app.maxIpsCount` 从代理 ip 消息头读取的最大 ips, 默认为 0 (代表无限)

您可以将设置传递给构造函数:

```js
  const Koa = require('koa');
  const app = new Koa({ proxy: true });
```

或动态的:

```js
  const Koa = require('koa');
  const app = new Koa();
  app.proxy = true;
```

### app.listen(...)

Koa 应用程序不是 HTTP 服务器的1对1展现。 可以将一个或多个 Koa 应用程序安装在一起以形成具有单个HTTP服务器的更大应用程序。

创建并返回 HTTP 服务器，将给定的参数传递给 `Server#listen()`。这些内容都记录在 [nodejs.org](http://nodejs.org/api/http.html#http_server_listen_port_hostname_backlog_callback).

以下是一个无作用的 Koa 应用程序被绑定到 `3000` 端口：

```js
const Koa = require('koa');
const app = new Koa();
app.listen(3000);
```

这里的 `app.listen(...)` 方法只是以下方法的语法糖:

```js
const http = require('http');
const Koa = require('koa');
const app = new Koa();
http.createServer(app.callback()).listen(3000);
```

这意味着您可以将同一个应用程序同时作为 HTTP 和 HTTPS 或多个地址：

```js
const http = require('http');
const https = require('https');
const Koa = require('koa');
const app = new Koa();
http.createServer(app.callback()).listen(3000);
https.createServer(app.callback()).listen(3001);
```

### app.callback()

返回适用于 `http.createServer()` 方法的回调函数来处理请求。你也可以使用此回调函数将 koa 应用程序挂载到 Connect/Express 应用程序中。

### app.use(function)

将给定的中间件方法添加到此应用程序。`app.use()` 返回 `this`, 因此可以链式表达.

```js
app.use(someMiddleware)
app.use(someOtherMiddleware)
app.listen(3000)
```

它等同于

```js
app.use(someMiddleware)
  .use(someOtherMiddleware)
  .listen(3000)
```

参阅 [Middleware](https://github.com/koajs/koa/wiki#middleware) 获取更多信息.

### app.keys=

设置签名的 Cookie 密钥。

这些被传递给 [KeyGrip](https://github.com/crypto-utils/keygrip)，但是你也可以传递你自己的 `KeyGrip` 实例。

例如，以下是可以接受的：

```js
app.keys = ['im a newer secret', 'i like turtle'];
app.keys = new KeyGrip(['im a newer secret', 'i like turtle'], 'sha256');
```

这些密钥可以倒换，并在使用 `{ signed: true }` 参数签名 Cookie 时使用。

```js
ctx.cookies.set('name', 'tobi', { signed: true });
```

### app.context

`app.context` 是从其创建 `ctx` 的原型。您可以通过编辑 `app.context` 为 `ctx` 添加其他属性。这对于将 `ctx` 添加到整个应用程序中使用的属性或方法非常有用，这可能会更加有效（不需要中间件）和/或 更简单（更少的 `require()`），而更多地依赖于`ctx`，这可以被认为是一种反模式。

例如，要从 `ctx` 添加对数据库的引用：

```js
app.context.db = db();

app.use(async ctx => {
  console.log(ctx.db);
});
```

注意:

- `ctx` 上的许多属性都是使用 `getter` ，`setter` 和 `Object.defineProperty()` 定义的。你只能通过在 `app.context` 上使用 `Object.defineProperty()` 来编辑这些属性（不推荐）。查阅 https://github.com/koajs/koa/issues/652.
- 安装的应用程序目前使用其父级的 `ctx` 和设置。 因此，安装的应用程序只是一组中间件。

#### 错误处理

默认情况下，将所有错误输出到 stderr，除非 `app.silent` 为 `true`。 当 `err.status` 是 `404` 或 `err.expose` 是 `true` 时默认错误处理程序也不会输出错误。 要执行自定义错误处理逻辑，如集中式日志记录，您可以添加一个 “error” 事件侦听器：

```js
app.on('error', err => {
  log.error('server error', err)
});
```

如果 req/res 期间出现错误，并且 _无法_ 响应客户端，`Context`实例仍然被传递：

```js
app.on('error', (err, ctx) => {
  log.error('server error', err, ctx)
});
```

当发生错误 _并且_ 仍然可以响应客户端时，也没有数据被写入 socket 中，Koa 将用一个 500 “内部服务器错误” 进行适当的响应。在任一情况下，为了记录目的，都会发出应用级 “错误”。

### 上下文(Context)

Koa Context 将 node 的 `request` 和 `response` 对象封装到单个对象中，为编写 Web 应用程序和 API 提供了许多有用的方法。 这些操作在 HTTP 服务器开发中频繁使用，它们被添加到此级别而不是更高级别的框架，这将强制中间件重新实现此通用功能。

_每个_ 请求都将创建一个 `Context`，并在中间件中作为接收器引用，或者 `ctx` 标识符，如以下代码片段所示：

```js
app.use(async ctx => {
  ctx; // 这是 Context
  ctx.request; // 这是 koa Request
  ctx.response; // 这是 koa Response
});
```

为方便起见许多上下文的访问器和方法直接委托给它们的 `ctx.request`或 `ctx.response` ，不然的话它们是相同的。 例如 `ctx.type` 和 `ctx.length` 委托给 `response` 对象，`ctx.path` 和 `ctx.method` 委托给 `request`。

### API

`Context` 具体方法和访问器.

### ctx.req

Node 的 `request` 对象.

### ctx.res

Node 的 `response` 对象.

绕过 Koa 的 response 处理是 **不被支持的**. 应避免使用以下 node 属性：

- `res.statusCode`
- `res.writeHead()`
- `res.write()`
- `res.end()`

### ctx.request

koa 的 `Request` 对象.

### ctx.response

koa 的 `Response` 对象.

### ctx.state

推荐的命名空间，用于通过中间件传递信息和你的前端视图。

```js
ctx.state.user = await User.find(id);
```

### ctx.app

应用程序实例引用

### ctx.app.emit

Koa 应用扩展了内部 [EventEmitter](https://nodejs.org/dist/latest-v11.x/docs/api/events.html)。`ctx.app.emit` 发出一个类型由第一个参数定义的事件。对于每个事件，您可以连接 "listeners"，这是在发出事件时调用的函数。有关更多信息，请参阅[错误处理文档](https://koajs.com/#error-handling)。

### ctx.cookies.get(name, [options])

通过 `options` 获取 cookie `name`:

- `signed` 所请求的cookie应该被签名

koa 使用 [cookies](https://github.com/pillarjs/cookies) 模块，其中只需传递参数。

### ctx.cookies.set(name, value, [options])

通过 `options` 设置 cookie `name` 的 `value` :

- ```
  maxAge
  ```

  : 一个数字, 表示从

   

  ```
  Date.now()
  ```

   

  得到的毫秒数.

  - `expires`: 一个 `Date` 对象, 表示 cookie 的到期日期 (默认情况下在会话结束时过期).
  - `path`: 一个字符串, 表示 cookie 的路径 (默认是`/`).
  - `domain`: 一个字符串, 指示 cookie 的域 (无默认值).
  - `secure`: 一个布尔值, 表示 cookie 是否仅通过 HTTPS 发送 (HTTP 下默认为 `false`, HTTPS 下默认为 `true`). [阅读有关此参数的更多信息](https://github.com/pillarjs/cookies#secure-cookies).
  - `httpOnly`: 一个布尔值, 表示 cookie 是否仅通过 HTTP(S) 发送，, 且不提供给客户端 JavaScript (默认为 `true`).
  - `sameSite`: 一个布尔值或字符串, 表示该 cookie 是否为 "相同站点" cookie (默认为 `false`). 可以设置为 `'strict'`, `'lax'`, `'none'`, 或 `true` (映射为 `'strict'`).
  - `signed`: 一个布尔值, 表示是否要对 cookie 进行签名 (默认为 `false`). 如果为 `true`, 则还会发送另一个后缀为 `.sig` 的同名 cookie, 使用一个 27-byte url-safe base64 SHA1 值来表示针对第一个 [Keygrip](https://www.npmjs.com/package/keygrip) 键的 *cookie-name*=*cookie-value* 的哈希值. 此签名密钥用于检测下次接收 cookie 时的篡改.
  - `overwrite`: 一个布尔值, 表示是否覆盖以前设置的同名的 cookie (默认是 `false`). 如果是 true, 在同一个请求中设置相同名称的所有 Cookie（无论路径或域）是否在设置此Cookie 时从 Set-Cookie 消息头中过滤掉.

koa 使用传递简单参数的 [cookies](https://github.com/pillarjs/cookies) 模块。

### ctx.throw([status], [msg], [properties])

用来抛出一个包含 `.status` 属性错误的帮助方法，其默认值为 `500`。这样 Koa 就可以做出适当地响应。

允许以下组合：

```js
ctx.throw(400);
ctx.throw(400, 'name required');
ctx.throw(400, 'name required', { user: user });
```

例如 `ctx.throw(400, 'name required')` 等效于:

```js
const err = new Error('name required');
err.status = 400;
err.expose = true;
throw err;
```

请注意，这些是用户级错误，并用 `err.expose` 标记，这意味着消息适用于客户端响应，这通常不是错误消息的内容，因为您不想泄漏故障详细信息。

你可以根据需要将 `properties` 对象传递到错误中，对于装载上传给请求者的机器友好的错误是有用的。这用于修饰其人机友好型错误并向上游的请求者报告非常有用。

```js
ctx.throw(401, 'access_denied', { user: user });
```

koa 使用 [http-errors](https://github.com/jshttp/http-errors) 来创建错误。`status` 只应作为第一个参数传递。

### ctx.assert(value, [status], [msg], [properties])

当 `!value` 时抛出一个类似 `.throw` 错误的帮助方法。这与 node 的 [assert()](http://nodejs.org/api/assert.html) 方法类似.

```js
ctx.assert(ctx.state.user, 401, 'User not found. Please login!');
```

koa 使用 [http-assert](https://github.com/jshttp/http-assert) 作为断言。

### ctx.respond

为了绕过 Koa 的内置 response 处理，你可以显式设置 `ctx.respond = false;`。 如果您想要写入原始的 `res` 对象而不是让 Koa 处理你的 response，请使用此参数。

请注意，Koa _不_ 支持使用此功能。这可能会破坏 Koa 中间件和 Koa 本身的预期功能。使用这个属性被认为是一个 hack，只是便于那些希望在 Koa 中使用传统的 `fn(req, res)` 功能和中间件的人。

## Request 别名

以下访问器和 [Request](https://koa.bootcss.com/#request) 别名等效：

- `ctx.header`
- `ctx.headers`
- `ctx.method`
- `ctx.method=`
- `ctx.url`
- `ctx.url=`
- `ctx.originalUrl`
- `ctx.origin`
- `ctx.href`
- `ctx.path`
- `ctx.path=`
- `ctx.query`
- `ctx.query=`
- `ctx.querystring`
- `ctx.querystring=`
- `ctx.host`
- `ctx.hostname`
- `ctx.fresh`
- `ctx.stale`
- `ctx.socket`
- `ctx.protocol`
- `ctx.secure`
- `ctx.ip`
- `ctx.ips`
- `ctx.subdomains`
- `ctx.is()`
- `ctx.accepts()`
- `ctx.acceptsEncodings()`
- `ctx.acceptsCharsets()`
- `ctx.acceptsLanguages()`
- `ctx.get()`

### Response 别名

以下访问器和 [Response](https://koa.bootcss.com/#response) 别名等效：

- `ctx.body`
- `ctx.body=`
- `ctx.status`
- `ctx.status=`
- `ctx.message`
- `ctx.message=`
- `ctx.length=`
- `ctx.length`
- `ctx.type=`
- `ctx.type`
- `ctx.headerSent`
- `ctx.redirect()`
- `ctx.attachment()`
- `ctx.set()`
- `ctx.append()`
- `ctx.remove()`
- `ctx.lastModified=`
- `ctx.etag=`

## 请求(Request)

Koa `Request` 对象是在 node 的 原生请求对象之上的抽象，提供了诸多对 HTTP 服务器开发有用的功能。

### API

### request.header

请求头对象。这与 node [`http.IncomingMessage`](https://nodejs.org/api/http.html#http_class_http_incomingmessage) 上的 [`headers`](https://nodejs.org/api/http.html#http_message_headers) 字段相同

### request.header=

设置请求头对象。

### request.headers

请求头对象。别名为 `request.header`.

### request.headers=

设置请求头对象。别名为 `request.header=`.

### request.method

请求方法。

### request.method=

设置请求方法，对于实现诸如 `methodOverride()` 的中间件是有用的。

### request.length

返回以数字返回请求的 Content-Length，或 `undefined`。

### request.url

获取请求 URL.

### request.url=

设置请求 URL, 对 url 重写有用。

### request.originalUrl

获取请求原始URL。

### request.origin

获取URL的来源，包括 `protocol` 和 `host`。

```js
ctx.request.origin
// => http://example.com
```

### request.href

获取完整的请求URL，包括 `protocol`，`host` 和 `url`。

```js
ctx.request.href;
// => http://example.com/foo/bar?q=1
```

### request.path

获取请求路径名。

### request.path=

设置请求路径名，并在存在时保留查询字符串。

### request.querystring

根据 `?` 获取原始查询字符串.

### request.querystring=

设置原始查询字符串。

### request.search

使用 `?` 获取原始查询字符串。

### request.search=

设置原始查询字符串。

### request.host

存在时获取主机（hostname:port）。当 `app.proxy` 是 **true** 时支持 `X-Forwarded-Host`，否则使用 `Host`。

### request.hostname

存在时获取主机名。当 `app.proxy` 是 **true** 时支持 `X-Forwarded-Host`，否则使用 `Host`。

如果主机是 IPv6, Koa 解析到 [WHATWG URL API](https://nodejs.org/dist/latest-v8.x/docs/api/url.html#url_the_whatwg_url_api), *注意* 这可能会影响性能。

### request.URL

获取 WHATWG 解析的 URL 对象。

### request.type

获取请求 `Content-Type`, 不含 "charset" 等参数。

> 译者注: 这里其实是只获取 *mime-type*, 详见[源码及其注释](https://github.com/koajs/koa/blob/eda27608f7d39ede86d7b402aae64b1867ce31c6/lib/request.js#L639)

```js
const ct = ctx.request.type;
// => "image/png"
```

### request.charset

存在时获取请求字符集，或者 `undefined`：

```js
ctx.request.charset;
// => "utf-8"
```

### request.query

获取解析的查询字符串, 当没有查询字符串时，返回一个空对象。请注意，此 getter _不_ 支持嵌套解析。

例如 "color=blue&size=small":

```js
{
  color: 'blue',
  size: 'small'
}
```

### request.query=

将查询字符串设置为给定对象。 请注意，此 setter _不_ 支持嵌套对象。

```js
ctx.query = { next: '/login' };
```

### request.fresh

检查请求缓存是否“新鲜”，也就是内容没有改变。此方法用于 `If-None-Match` / `ETag`, 和 `If-Modified-Since` 和 `Last-Modified` 之间的缓存协商。 在设置一个或多个这些响应头后应该引用它。

```js
// 新鲜度检查需要状态20x或304
ctx.status = 200;
ctx.set('ETag', '123');

// 缓存是好的
if (ctx.fresh) {
  ctx.status = 304;
  return;
}

// 缓存是陈旧的
// 获取新数据
ctx.body = await db.find('something');
```

### request.stale

与 `request.fresh` 相反.

### request.protocol

返回请求协议，“https” 或 “http”。当 `app.proxy` 是 **true** 时支持 `X-Forwarded-Proto`。

### request.secure

通过 `ctx.protocol == "https"` 来检查请求是否通过 TLS 发出。

### request.ip

请求远程地址。 当 `app.proxy` 是 **true** 时支持 `X-Forwarded-Proto`。

### request.ips

当 `X-Forwarded-For` 存在并且 `app.proxy` 被启用时，这些 ips 的数组被返回，从上游 - >下游排序。 禁用时返回一个空数组。

例如，如果值是 "client, proxy1, proxy2"，将会得到数组 `["client", "proxy1", "proxy2"]`。

大多数反向代理（nginx）都通过 `proxy_add_x_forwarded_for` 设置了 x-forwarded-for，这带来了一定的安全风险。恶意攻击者可以通过伪造 `X-Forwarded-For` 请求头来伪造客户端的ip地址。 客户端发送的请求具有 'forged' 的 `X-Forwarded-For` 请求头。 在由反向代理转发之后，`request.ips` 将是 ['forged', 'client', 'proxy1', 'proxy2']。

Koa 提供了两种方式来避免被绕过。

如果您可以控制反向代理，则可以通过调整配置来避免绕过，或者使用 koa 提供的 `app.proxyIpHeader` 来避免读取 `x-forwarded-for` 获取 ips。

```js
const app = new Koa({
  proxy: true,
  proxyIpHeader: 'X-Real-IP',
});
```

如果您确切知道服务器前面有多少个反向代理，则可以通过配置 `app.maxIpsCount` 来避免读取用户的伪造的请求头：

```js
const app = new Koa({
  proxy: true,
  maxIpsCount: 1, // 服务器前只有一个代理
});

// request.header['X-Forwarded-For'] === [ '127.0.0.1', '127.0.0.2' ];
// ctx.ips === [ '127.0.0.2' ];
```

### request.subdomains

以数组形式返回子域。

子域是应用程序主域之前主机的点分隔部分。默认情况下，应用程序的域名假定为主机的最后两个部分。这可以通过设置 `app.subdomainOffset` 来更改。

例如，如果域名为“tobi.ferrets.example.com”：

如果 `app.subdomainOffset` 未设置, `ctx.subdomains` 是 `["ferrets", "tobi"]`. 如果 `app.subdomainOffset` 是 3, `ctx.subdomains` 是 `["tobi"]`.

### request.is(types...)

检查传入请求是否包含 `Content-Type` 消息头字段， 并且包含任意的 mime `type`。 如果没有请求主体，返回 `null`。 如果没有内容类型，或者匹配失败，则返回 `false`。 反之则返回匹配的 content-type。

```js
// 使用 Content-Type: text/html; charset=utf-8
ctx.is('html'); // => 'html'
ctx.is('text/html'); // => 'text/html'
ctx.is('text/*', 'text/html'); // => 'text/html'

// 当 Content-Type 是 application/json 时
ctx.is('json', 'urlencoded'); // => 'json'
ctx.is('application/json'); // => 'application/json'
ctx.is('html', 'application/*'); // => 'application/json'

ctx.is('html'); // => false
```

例如，如果要确保仅将图像发送到给定路由：

```js
if (ctx.is('image/*')) {
  // 处理
} else {
  ctx.throw(415, 'images only!');
}
```

### 内容协商

Koa 的 `request` 对象包括由 [accepts](http://github.com/expressjs/accepts) 和 [negotiator](https://github.com/federomero/negotiator) 提供的内容协商实用函数。

这些实用函数是：

- `request.accepts(types)`
- `request.acceptsEncodings(types)`
- `request.acceptsCharsets(charsets)`
- `request.acceptsLanguages(langs)`

如果没有提供类型，则返回 **所有** 可接受的类型。

如果提供多种类型，将返回最佳匹配。 如果没有找到匹配项，则返回一个`false`，你应该向客户端发送一个`406 "Not Acceptable"` 响应。

如果接收到任何类型的接收头，则会返回第一个类型。 因此，你提供的类型的顺序很重要。

### request.accepts(types)

检查给定的 `type(s)` 是否可以接受，如果 `true`，返回最佳匹配，否则为 `false`。 `type` 值可能是一个或多个 mime 类型的字符串，如 `application/json`，扩展名称如 `json`，或数组 `["json", "html", "text/plain"]`。

```js
// Accept: text/html
ctx.accepts('html');
// => "html"

// Accept: text/*, application/json
ctx.accepts('html');
// => "html"
ctx.accepts('text/html');
// => "text/html"
ctx.accepts('json', 'text');
// => "json"
ctx.accepts('application/json');
// => "application/json"

// Accept: text/*, application/json
ctx.accepts('image/png');
ctx.accepts('png');
// => false

// Accept: text/*;q=.5, application/json
ctx.accepts(['html', 'json']);
ctx.accepts('html', 'json');
// => "json"

// No Accept header
ctx.accepts('html', 'json');
// => "html"
ctx.accepts('json', 'html');
// => "json"
```

你可以根据需要多次调用 `ctx.accepts()`，或使用 switch：

```js
switch (ctx.accepts('json', 'html', 'text')) {
  case 'json': break;
  case 'html': break;
  case 'text': break;
  default: ctx.throw(406, 'json, html, or text only');
}
```

### request.acceptsEncodings(encodings)

检查 `encodings` 是否可以接受，返回最佳匹配为 `true`，否则为 `false`。 请注意，您应该将`identity` 作为编码之一！

```js
// Accept-Encoding: gzip
ctx.acceptsEncodings('gzip', 'deflate', 'identity');
// => "gzip"

ctx.acceptsEncodings(['gzip', 'deflate', 'identity']);
// => "gzip"
```

当没有给出参数时，所有接受的编码将作为数组返回：

```js
// Accept-Encoding: gzip, deflate
ctx.acceptsEncodings();
// => ["gzip", "deflate", "identity"]
```

请注意，如果客户端显式地发送 `identity;q=0`，那么 `identity` 编码（这意味着没有编码）可能是不可接受的。 虽然这是一个边缘的情况，你仍然应该处理这种方法返回 `false` 的情况。

### request.acceptsCharsets(charsets)

检查 `charsets` 是否可以接受，在 `true` 时返回最佳匹配，否则为 `false`。

```js
// Accept-Charset: utf-8, iso-8859-1;q=0.2, utf-7;q=0.5
ctx.acceptsCharsets('utf-8', 'utf-7');
// => "utf-8"

ctx.acceptsCharsets(['utf-7', 'utf-8']);
// => "utf-8"
```

当没有参数被赋予所有被接受的字符集将作为数组返回：

```js
// Accept-Charset: utf-8, iso-8859-1;q=0.2, utf-7;q=0.5
ctx.acceptsCharsets();
// => ["utf-8", "utf-7", "iso-8859-1"]
```

### request.acceptsLanguages(langs)

检查 `langs` 是否可以接受，如果为 `true`，返回最佳匹配，否则为 `false`。

```js
// Accept-Language: en;q=0.8, es, pt
ctx.acceptsLanguages('es', 'en');
// => "es"

ctx.acceptsLanguages(['en', 'es']);
// => "es"
```

当没有参数被赋予所有接受的语言将作为数组返回：

```js
// Accept-Language: en;q=0.8, es, pt
ctx.acceptsLanguages();
// => ["es", "pt", "en"]
```

### request.idempotent

检查请求是否是幂等的。

### request.socket

返回请求套接字。

### request.get(field)

返回请求头(header), `field` 不区分大小写.

## 响应(Response)

Koa `Response` 对象是在 node 的原生响应对象之上的抽象，提供了诸多对 HTTP 服务器开发有用的功能。

### API

### response.header

响应头对象。

### response.headers

响应头对象。别名是 `response.header`。

### response.socket

响应套接字。 作为 `request.socket` 指向 net.Socket 实例。

### response.status

获取响应状态。默认情况下，`response.status` 设置为 `404` 而不是像 node 的 `res.statusCode` 那样默认为 `200`。

### response.status=

通过数字代码设置响应状态：

- 100 "continue"
- 101 "switching protocols"
- 102 "processing"
- 200 "ok"
- 201 "created"
- 202 "accepted"
- 203 "non-authoritative information"
- 204 "no content"
- 205 "reset content"
- 206 "partial content"
- 207 "multi-status"
- 208 "already reported"
- 226 "im used"
- 300 "multiple choices"
- 301 "moved permanently"
- 302 "found"
- 303 "see other"
- 304 "not modified"
- 305 "use proxy"
- 307 "temporary redirect"
- 308 "permanent redirect"
- 400 "bad request"
- 401 "unauthorized"
- 402 "payment required"
- 403 "forbidden"
- 404 "not found"
- 405 "method not allowed"
- 406 "not acceptable"
- 407 "proxy authentication required"
- 408 "request timeout"
- 409 "conflict"
- 410 "gone"
- 411 "length required"
- 412 "precondition failed"
- 413 "payload too large"
- 414 "uri too long"
- 415 "unsupported media type"
- 416 "range not satisfiable"
- 417 "expectation failed"
- 418 "I'm a teapot"
- 422 "unprocessable entity"
- 423 "locked"
- 424 "failed dependency"
- 426 "upgrade required"
- 428 "precondition required"
- 429 "too many requests"
- 431 "request header fields too large"
- 500 "internal server error"
- 501 "not implemented"
- 502 "bad gateway"
- 503 "service unavailable"
- 504 "gateway timeout"
- 505 "http version not supported"
- 506 "variant also negotiates"
- 507 "insufficient storage"
- 508 "loop detected"
- 510 "not extended"
- 511 "network authentication required"

**注意**: 不用太在意记住这些字符串, 如果你写错了,可以查阅这个列表随时更正.

由于 `response.status` 默认设置为 `404`，因此发送没有 body 且状态不同的响应的操作如下：

```js
ctx.response.status = 200;

// 或其他任何状态
ctx.response.status = 204;
```

### response.message

获取响应的状态消息. 默认情况下, `response.message` 与 `response.status` 关联.

### response.message=

将响应的状态消息设置为给定值。

### response.length=

将响应的 Content-Length 设置为给定值。

### response.length

以数字返回响应的 Content-Length，或者从`ctx.body`推导出来，或者`undefined`。

### response.body

获取响应主体。

### response.body=

将响应体设置为以下之一：

- `string` 写入
- `Buffer` 写入
- `Stream` 管道
- `Object` || `Array` JSON-字符串化
- `null` 无内容响应

如果 `response.status` 未被设置, Koa 将会自动设置状态为 `200` 或 `204`。

Koa 没有防范作为响应体的所有内容 - 函数没有有意义地序列化，返回布尔值可能会根据您的应用程序而有意义。并且当错误生效时，它可能无法正常工作 错误的属性无法枚举。 我们建议在您的应用中添加中间件，以确定每个应用的正文类型。 示例中间件可能是：

```js
app.use(async (ctx, next) => {
  await next()

  ctx.assert.equal('object', typeof ctx, 500, '某些开发错误')
})
```

#### String

Content-Type 默认为 `text/html` 或 `text/plain`, 同时默认字符集是 utf-8。Content-Length 字段也是如此。

#### Buffer

Content-Type 默认为 `application/octet-stream`, 并且 Content-Length 字段也是如此。

#### Stream

Content-Type 默认为 `application/octet-stream`。

每当流被设置为响应主体时，`.onerror` 作为侦听器自动添加到 `error` 事件中以捕获任何错误。此外，每当请求关闭（甚至过早）时，流都将被销毁。如果你不想要这两个功能，请勿直接将流设为主体。例如，当将主体设置为代理中的 HTTP 流时，你可能不想要这样做，因为它会破坏底层连接。

参阅: https://github.com/koajs/koa/pull/612 获取更多信息。

以下是流错误处理的示例，而不会自动破坏流：

```js
const PassThrough = require('stream').PassThrough;

app.use(async ctx => {
  ctx.body = someHTTPStream.on('error', (err) => ctx.onerror(err)).pipe(PassThrough());
});
```

#### Object

Content-Type 默认为 `application/json`. 这包括普通的对象 `{ foo: 'bar' }` 和数组 `['foo', 'bar']`。

### response.get(field)

不区分大小写获取响应头字段值 `field`。

```js
const etag = ctx.response.get('ETag');
```

### response.has(field)

如果当前在响应头中设置了由名称标识的消息头，则返回 `true`. 消息头名称匹配不区分大小写.

```js
const rateLimited = ctx.response.has('X-RateLimit-Limit');
```

### response.set(field, value)

设置响应头 `field` 到 `value`:

```js
ctx.set('Cache-Control', 'no-cache');
```

### response.append(field, value)

用值 `val` 附加额外的消息头 `field`。

```js
ctx.append('Link', '<http://127.0.0.1/>');
```

### response.set(fields)

用一个对象设置多个响应头`fields`:

```js
ctx.set({
  'Etag': '1234',
  'Last-Modified': date
});
```

这将委托给 [setHeader](https://nodejs.org/dist/latest/docs/api/http.html#http_request_setheader_name_value) ，它通过指定的键设置或更新消息头，并且不重置整个消息头。

### response.remove(field)

删除消息头 `field`。

### response.type

获取响应 `Content-Type`, 不含 "charset" 等参数。

> 译者注: 这里其实是只获取 *mime-type*, 详见[源码及其注释](https://github.com/koajs/koa/blob/eda27608f7d39ede86d7b402aae64b1867ce31c6/lib/response.js#L371)

```js
const ct = ctx.type;
// => "image/png"
```

### response.type=

设置响应 `Content-Type` 通过 mime 字符串或文件扩展名。

```js
ctx.type = 'text/plain; charset=utf-8';
ctx.type = 'image/png';
ctx.type = '.png';
ctx.type = 'png';
```

注意: 在适当的情况下为你选择 `charset`, 比如 `response.type = 'html'` 将默认是 "utf-8". 如果你想覆盖 `charset`, 使用 `ctx.set('Content-Type', 'text/html')` 将响应头字段设置为直接值。

### response.is(types...)

非常类似 `ctx.request.is()`. 检查响应类型是否是所提供的类型之一。这对于创建操纵响应的中间件特别有用。

例如, 这是一个中间件，可以削减除流之外的所有HTML响应。

```js
const minify = require('html-minifier');

app.use(async (ctx, next) => {
  await next();

  if (!ctx.response.is('html')) return;

  let body = ctx.body;
  if (!body || body.pipe) return;

  if (Buffer.isBuffer(body)) body = body.toString();
  ctx.body = minify(body);
});
```

### response.redirect(url, [alt])

执行 [302] 重定向到 `url`.

字符串 “back” 是特别提供 Referrer 支持的，当 Referrer 不存在时，使用 `alt` 或 “/”。

```js
ctx.redirect('back');
ctx.redirect('back', '/index.html');
ctx.redirect('/login');
ctx.redirect('http://google.com');
```

要更改 “302” 的默认状态，只需在该调用之前或之后给 `status` 赋值。要变更主体请在此调用之后:

```js
ctx.status = 301;
ctx.redirect('/cart');
ctx.body = 'Redirecting to shopping cart';
```

### response.attachment([filename], [options])

将 `Content-Disposition` 设置为 “附件” 以指示客户端提示下载。(可选)指定下载的 `filename` 和部分 [参数](https://github.com/jshttp/content-disposition#options)。

### response.headerSent

检查是否已经发送了一个响应头。 用于查看客户端是否可能会收到错误通知。

### response.lastModified

将 `Last-Modified` 消息头返回为 `Date`, 如果存在。

### response.lastModified=

将 `Last-Modified` 消息头设置为适当的 UTC 字符串。您可以将其设置为 `Date` 或日期字符串。

```js
ctx.response.lastModified = new Date();
```

### response.etag=

设置包含 `"` 包裹的 ETag 响应， 请注意，没有相应的 `response.etag` getter。

```js
ctx.response.etag = crypto.createHash('md5').update(ctx.body).digest('hex');
```

### response.vary(field)

设置 `field` 的 `vary`。

### response.flushHeaders()

刷新任何设置的消息头，然后是主体(body)。

## 参与翻译

如果您希望贡献力量完善本中文文档，请前往 https://github.com/demopark/koa-docs-Zh-CN.git 仓库。

## 相关资源

Community links to discover third-party middleware for Koa, full runnable examples, thorough guides and more! If you have questions join us in IRC!

- [GitHub repository](https://github.com/koajs/koa)
- [Examples](https://github.com/koajs/examples)
- [Middleware](https://github.com/koajs/koa/wiki)
- [Wiki](https://github.com/koajs/koa/wiki)
- [G+ Community](https://plus.google.com/communities/101845768320796750641)
- [Mailing list](https://groups.google.com/forum/#!forum/koajs)
- [Guide](https://github.com/koajs/koa/blob/master/docs/guide.md)
- [FAQ](https://github.com/koajs/koa/blob/master/docs/faq.md)
- **#koajs** on freenode