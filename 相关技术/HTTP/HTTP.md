# HTTP

## 1.1 什么是 HTTP

HTTP 是一个连接客户端，网关和服务器的一个协议。

## 7.2 特点

支持客户/服务器模式：可以连接客户端和服务端；
 简单快速：请求只需传送请求方法，路径和请求主体；
 灵活：传输数据类型灵活；
 无连接：请求结束立即断开；
 无状态：无法记住上一次请求。

## 7.3 怎么解决无状态和无连接

无状态：HTTP 协议本身无法解决这个状态，只有通过 cookie 和 session 将状态做贮存，常见的场景是登录状态保持；

无连接：可以通过自身属性 Keep-Alive。

## 7.4 请求过程

HTTP(S) 请求地址 → DNS 解析 → 三次握手 → 发送请求 → 四次挥手

三次握手过程图片来源 CSDN） ![3 次握手.jpg](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d65b93d07fd74758a60a0ecb9c117200~tplv-k3u1fbpfcp-zoom-1.image) 在这里插入图片描述

1. 四次挥手过（图片来源 CSDN）

![image](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2a12c33ac8d04fec8f21eb5c4fc482ab~tplv-k3u1fbpfcp-zoom-1.image) 在这里插入图片描述

## 7.5 HTTP 0.9~3.0 对比

### 7.5.1 HTTP 0.9

只允许客户端发送 GET 这一种请求；
 且不支持请求头，协议只支持纯文本；
 无状态性，每个访问独立处理，完成断开；
 无状态码。

### 7.5.2 HTTP 1.0

有身份认证，三次握手； 请求与响应支持头域； 请求头内容； |属性名|含义| |--|--|--| |Accept |可接受的 MIME 类型| |Accept-Encoding| 数据可解码的格式| |Accept-Language| 可接受语言| |Connection| 值 keep-alive 是长连接| |Host| 主机和端口| |Pragma| 是否缓存,指定 no-cache 返回刷新| |Referer| 页面路由| |If-Modified-Since| 值为时间|

响应头内容；

|属性名| 含义| |-|-|-| |Connection| 值 keep-alive 是长连接| |Content-Type| 返回文档类型,常见的值有 text/plain,text/html,text/json| |Date| 消息发送的时间| |Server| 服务器名字| |Last-Modified| 值为时间,s 返回的最后修改时间| |Expires| 缓存过期时间,b 和 s 时间做对比| 注意

expires 是响应头内容，返回一个固定的时间,缺陷是时间到了服务器要重新设置;
 请求头中如果有 If-Modified-Since，服务器会将时间与 last-modified 对比，相同返回 304;
 响应对象以一个响应状态行开始;
 响应对象不只限于超文本;
 支持 GET、HEAD、POST 方法;
 有状态码;
 支持长连接（但默认还是使用短连接）、缓存机制以及身份认证。

### 7.5.3 HTTP 1.1

请求头增加 Cache-Control

|属性名| 含义| |-|-|-| |Cache-Control| 在1.1 引入的方法,指定请求和响应遵循的缓存机制,值有:public(b 和 s 都缓存),private(b 缓存),no-cache(不缓存),no-store(不缓存),max-age(缓存时间,s 为单位),min-fresh(最小更新时间),max-age=3600| |If-None-Match | 上次请求响应头返回的 etag 值响应头增加 Cache-Control，表示所有的缓存机制是否可以缓存及哪种类型 etag 返回的哈希值,第二次请求头携带去和服务器值对比|

注意

Cache-Control 的 max-age 返回是缓存的相对时间 Cache-Control 优先级比 expires 高 缺点：不能第一时间拿到最新修改文件

### 7.5.4 HTTP 2.0

采用二进制格式传输;
 多路复用，其实就是将请求数据分成帧乱序发送到 TCP 中。TCP 只能有一个 steam，所以还是会阻塞;
 报头压缩;
 服务器推送主动向 B 端发送静态资源，避免往返延迟。

### 7.5.5 HTTP 3.0

1.是基于 QUIC 协议，基于 UDP
 2.特点:
 自定义连接机制：TCP 以 IP/端口标识,变化重新连接握手，UDP 是一 64 位 ID 标识，是无连接；
 自定义重传机制：TCP 使用序号和应答传输，QUIC 是使用递增序号传输； 无阻塞的多路复用：同一条 QUIC 可以创建多个 steam。

### 7.5.6 HTTPS

1.https 是在 http 协议的基础上加了个 SSL；
 2.主要包括：握手(凭证交换和验证)和记录协议(数据进行加密)。

### 7.5.7 缓存

1.按协议分：协议层缓存和非 http 协议缓存：
 1.1协议层缓存：利用 http 协议头属性值设置；
 1.2非协议层缓存：利用 meta 标签的 http-equiv 属性值 Expires,set-cookie。

2.按缓存分：强缓存和协商缓存：
 2.1强缓存：利用 cache-control 和 expires 设置，直接返回一个过期时间，所以在缓存期间不请求，If-modify-since；
 2.2协商缓存：响应头返回 etag 或 last-modified 的哈希值，第二次请求头 If-none-match 或 IF-modify-since 携带上次哈希值，一致则返回 304。

3.协商缓存对比： etag 优先级高于 last-modified；
 4.etag 精度高，last-modified 精度是 s，1s 内 etag 修改多少次都会被记录； last-modified 性能好，etag 要得到 hash 值。

5.浏览器读取缓存流程： 会先判断强缓存；再判断协商缓存 etag(last-modified)是否存在；
 存在利用属性 If-None-match(If-Modified-since)携带值；
 请求服务器,服务器对比 etag(last-modified)，生效返回 304。

F5 刷新会忽略强缓存不会忽略协商缓存，ctrl+f5 都失效

### 7.5.8 状态码

|序列| 详情| |-|-|-| |1XX(通知)|| |2XX(成功)| 200(成功)、201(服务器创建)、202(服务器接收未处理)、203(非授权信息)、204(未返回内容)、205(重置内容)、206(部分内容)| |3XX(重定向)| 301(永久移动)、302(临时移动)、303(查看其他位置)、304(未修改)、305(使用代理)、307(临时重定向)| |4XX(客户端错误) |400(错误请求)、401(未授权)、403(禁止)、404(未找到)、405(方法禁用)、406(不接受)、407（需要代理授权）| |5XX(服务器错误)| 500(服务器异常)、501（尚未实施）、502（错误网关）、503（服务不可用）、504（网关超时）、505（HTTP 版本不受支持）|

### 7.5.9 浏览器请求分析

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0fc332eae3354d06a9802e8a7713594c~tplv-k3u1fbpfcp-zoom-1.image)

### 7.5.10 总结

协议

|版本 |内容| |-|-|-| |http0.9| 只允许客户端发送 GET 这一种请求;且不支持请求头,协议只支持纯文本;无状态性,每个访问独立处理,完成断开;无状态码 http1.0 解决 0.9 的缺点,增加 If-modify-since(last-modify)和 expires 缓存属性| |http1.x| 增加 cache-control 和 If-none-match(etag)缓存属性| |http2.0| 采用二进制格式传输;多路复用;报头压缩;服务器推送| |http3.0| 采用 QUIC 协议,自定义连接机制;自定义重传机制;无阻塞的多路复用| 缓存

|类型| 特性| |-|-|-| |强缓存| 通过 If-modify-since(last-modify)、expires 和 cache-control 设置，属性值是时间，所以在时间内不用请求| |协商缓存| 通过 If-none-match(etag)设置，etag 属性是哈希值，所以要请求和服务器值对比|
