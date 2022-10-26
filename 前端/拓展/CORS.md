# CORS

> **CORS** （Cross-Origin Resource Sharing，跨域资源共享）是一个系统，它由一系列传输的[HTTP头](https://developer.mozilla.org/zh-CN/docs/Glossary/HTTP_header)组成，这些HTTP头决定浏览器是否阻止前端 JavaScript 代码获取跨域请求的响应。
>
> [同源安全策略](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy) 默认阻止“跨域”获取资源。但是 CORS 给了web服务器这样的权限，即服务器可以选择，允许跨域请求访问到它们的资源

## [Access-Control-Allow-Origin](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)

> 指示请求的资源能共享给哪些域。
>
> `Header type`: `Response header`
>
> `Access-Control-Allow-Origin: *` // 允许所有资源都可以访问您的资源,
> `Access-Control-Allow-Origin: https://developer.mozilla.org` // 允许 <https://developer.mozilla.org>访问您的资源

## [Access-Control-Allow-Credentials](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Credentials)

> 指示当请求的凭证标记为 true 时，是否响应该请求。
>
> 响应头表示是否可以将对请求的响应暴露给页面, 返回true可以, 其他值均不可以;
>
> `Header type`: `Response header`
>
> Credentials可以是 cookies, authorization headers 或 TLS client certificates。
>
> 当作为对预检请求的响应的一部分时，这能表示是否真正的请求可以使用credentials。
>
> 注意简单的`GET` 请求没有预检，所以若一个对资源的请求带了credentials，如果这个响应头没有随资源返回，响应就会被浏览器忽视，不会返回到web内容。

允许credentials:

```
Access-Control-Allow-Credentials: true
```

使用带credentials的 [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) ：

```
var xhr = new XMLHttpRequest();
xhr.open('GET', 'http://example.com/', true);
xhr.withCredentials = true;
xhr.send(null);
```

Copy to Clipboard

使用带credentials的 [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) ：

```js
fetch(url, {
  credentials: 'include'
})
```

## [Access-Control-Allow-Headers](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Headers)

> 用在对预请求的响应中，指示实际的请求中可以使用哪些 HTTP 头。
>
> 响应首部 **`Access-Control-Allow-Headers`** 用于 [preflight request](https://developer.mozilla.org/zh-CN/docs/Glossary/Preflight_request) （预检请求）中，列出了将会在正式请求的 [Access-Control-Request-Headers](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Request-Headers) 字段中出现的首部信息。
>
> 简单首部，如 [simple headers](https://developer.mozilla.org/zh-CN/docs/Glossary/Simple_header)、[Accept](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept)、[Accept-Language](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Language)、[Content-Language](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Language)、[Content-Type](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type) （只限于解析后的值为 `application/x-www-form-urlencoded、multipart/form-data`或 `text/plain 三种MIME类型（不包括参数）），它们始终是被支持的，不需要在这个首部特意列出。`
>
> ```xml
> Access-Control-Allow-Headers: <header-name>[, <header-name>]*
> Access-Control-Allow-Headers: *
> ```
>
> `<header-name>`: 可支持的请求首部名字。请求头会列出所有支持的首部列表，用逗号隔开。

## [Access-Control-Allow-Methods](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Methods)

> 指定对预请求的响应中，哪些 HTTP 方法允许访问请求的资源。
>
> `Access-Control-Allow-Methods: <method>, <method>, ...`
>
> `Access-Control-Allow-Methods: POST, GET, OPTIONS`
>
> `<method>` :  GET, POST, HEAD, PUT, DELETE, CONNECT, OPTIONS, TRACE, PATCH

## [Access-Control-Expose-Headers](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Expose-Headers)

> 指示哪些 HTTP 头的名称能在响应中列出
>
> `Access-Control-Expose-Headers: <header-name>, <header-name>, ...`
>
> 默认情况七种`simple response headers`
>
> - [Cache-Control](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control)
> - [Content-Language](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Language)
> - [Content-Length](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Length)
> - [Content-Type](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type)
> - [Expires](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Expires)
> - [Last-Modified](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Last-Modified)
> - [Pragma](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Pragma)

## [Access-Control-Max-Age](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Max-Age)

> 指示预请求的结果能被缓存多久。(即 [Access-Control-Allow-Methods](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Methods) 和[Access-Control-Allow-Headers](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Headers) 提供的信息） 可以被缓存多久。)
>
> `Access-Control-Max-Age: <delta-seconds>`
>
> `<delta-seconds>`
>
> - 返回结果可以被缓存的最长时间（秒）。
>   在 Firefox 中，[上限是24小时](https://dxr.mozilla.org/mozilla-central/rev/7ae377917236b7e6111146aa9fb4c073c0efc7f4/netwerk/protocol/http/nsCORSListenerProxy.cpp#1131) （即 86400 秒）。
>   在 Chromium v76 之前， [上限是 10 分钟](https://cs.chromium.org/chromium/src/services/network/public/cpp/cors/preflight_result.cc?l=36&rcl=52002151773d8cd9ffc5f557cd7cc880fddcae3e)（即 600 秒)。
>   从 Chromium v76 开始，[上限是 2 小时](https://cs.chromium.org/chromium/src/services/network/public/cpp/cors/preflight_result.cc?l=31&rcl=49e7c0b4886cac1f3d09dc046bd528c9c811a0fa)（即 7200 秒)。
>   Chromium 同时规定了一个默认值 5 秒。
>   如果值为 **-1**，表示禁用缓存，则每次请求前都需要使用 OPTIONS 预检请求。

## [Access-Control-Request-Headers](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Request-Headers)

> 用于发起一个预请求，告知服务器正式请求会使用那些 HTTP 头。
>
> `Access-Control-Request-Headers: <header-name>, <header-name>, ...`
>
> `Access-Control-Request-Headers: X-PINGOTHER, Content-Type`

## [Access-Control-Request-Method](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Request-Method)

> 用于发起一个预请求，告知服务器正式请求会使用哪一种 [HTTP 请求方法](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods)。
>
> `Access-Control-Request-Method: <method>`
>
> `Access-Control-Request-Method: POST`

## [Origin](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Origin)

> 指示获取资源的请求是从什么域发起的
>
> `Origin: ""`
>
> `Origin: <scheme> "://" <host> [ ":" <port> ]`
>
> `Origin: https://developer.mozilla.org`
>
> - `<scheme>`:  请求所使用的的协议, 通常是HTTP协议或他的安全版本HTTPS协议
> - `<host>`:  服务器的域名或IP地址
> - `<port>`:(可选) 服务器正在监听的TCP端口号, 缺省为服务的默认蹲坑(HTTP请求而言, 默认端口为80)

## [Vary - HTTP](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Vary)

> `Header type`: `Response header`
>
> `Vary: *` : 所有请求都被视为唯一并且非缓存
>
> - 使用`Cache-Control: no-store`: 实现更加使用
>
> `Vary: <header-name>, <header-name>...`: 都好分隔的一系列http头部名称, 用户确定缓存是否可用
>
> 动态服务:
>
> - `Vary: User-Agent`: 可以防止客户端使用了桌面端的缓存

## Response header 响应头

> 响应头(Response header) : 定义为被用于http响应中并且和相应信息主题无关的那一类HTTP header

```xml
200 OK
Access-Control-Allow-Origin: *
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
Date: Mon, 18 Jul 2016 16:06:00 GMT
Etag: "c561c68d0ba92bbeb8b0f612a9199f722e3a621a"
Keep-Alive: timeout=5, max=997
Last-Modified: Mon, 18 Jul 2016 02:36:04 GMT
Server: Apache
Set-Cookie: mykey=myvalue; expires=Mon, 17-Jul-2017 16:06:00 GMT; Max-Age=31449600; Path=/; secure
Transfer-Encoding: chunked
Vary: Cookie, Accept-Encoding
X-Backend-Server: developer2.webapp.scl3.mozilla.com
X-Cache-Info: not cacheable; meta data too large
X-kuma-revision: 1085259
x-frame-options: DENY
```

### Age

> `Age: <delta-seconds>`: 表示对象在缓存代理服务器中存储的时长( 单位秒) [delta-seconds非负整数]

### Location

> `Header type`: `Response header`
>
> `Location` 首部指定的是需要将页面重新定向至的地址。一般在响应码为3xx的响应中才会有意义。
>
> 发送新请求，获取Location指向的新页面所采用的方法与初始请求使用的方法以及重定向的类型相关：
>
> - [303](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/303) (See Also) 始终引致请求使用 [GET](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 方法，而，而 [307](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/307) (Temporary Redirect) 和 [308](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/308) (See Also) 始终引致请求使用 [GET](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 方法，而，而 [307](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/307) (Temporary Redirect) 和 [308](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/308) (Permanent Redirect) 则不转变初始请求中的所使用的方法；
> - [301](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/301) (Permanent Redirect) 和 [302](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/302) (Found) 在大多数情况下不会转变初始请求中的方法，不过一些比较早的用户代理可能会引发方法的变更（所以你基本上不知道这一点）。
>
> 状态码为上述之一的所有响应都会带有一个Location首部。
>
> 除了重定向响应之外， 状态码为 [201](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/201) (Created) 的消息也会带有Location首部。它指向的是新创建的资源的地址。
>
> [Location](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Location) 与 `Content-Location`是不同的，前者（[Location](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Location) ）指定的是一个重定向请求的目的地址（或者新创建的文件的URL），而后者（ `Content-Location`） 指向的是经过内容协商后的资源的直接地址，不需要进行进一步的内容协商。Location 对应的是响应，而Content-Location对应的是要返回的实体。

```
Location: <url>
Location: /index.html
- url: 相对地址（相对于要访问的URL）或绝对地址。
```

### Server

> `Header type`: `Response header`
>
> Server: 首部包含请求的源头服务器所用到的软件相关信息
>
> - 尽量避免使用过长或者过于信息的描述作为Server的值, 太长容易泄漏服务器的内部实现细节, 容易被通过已知安全漏洞攻击, 不安全
>
> `Server: <product>`
>
> `Server:Apache/2.4.1(Unix)`
>
> `<product>`: 处理请求的软件或产品或组件产品的名称

## 禁止修改的消息首部

> 禁止修改的消息首部: 不能再代码中修改的HTTP协议消息首部
>
> 用户代理对这些消息首部保留全部控制权,   应用程序无法设置它们
>
> `User-Agent`: 以及从列表冲移除

禁止修改消息首部包括Proxy- 和Sec-开头的消息首部, 以及

```xml
Accept-Charset
Accept-Encoding
Access-Control-Request-Headers
Access-Control-Request-Method
Connection
Content-Length
Cookie
Cookie2
Date
DNT
Expect
Host
Keep-Alive
Origin
Proxy-
Sec-
Referer
TE
Trailer
Transfer-Encoding
Upgrade
Via
```
