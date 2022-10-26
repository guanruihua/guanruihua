# Location

## 属性

### ancestorOrigins

> [Location](https://developer.mozilla.org/zh-CN/docs/Web/API/Location)接口的 **`ancestorOrigins`** 只读属性是一个静态的[DOMStringList](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMStringList)，倒序排列了此[Location](https://developer.mozilla.org/zh-CN/docs/Web/API/Location)接口的 **`ancestorOrigins`** 只读属性是一个静态的[`DOMStringList`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMStringList)，倒序排列了此[Location](https://developer.mozilla.org/zh-CN/docs/Web/API/Location)对象所属文档先前所有浏览上下文的来源。
>
> 你可以在脚本中使用`location.ancestorOrigins`来检测你的网页是否被你不希望的对象嵌入了。你也可以使用它让网页在被特定站点嵌入时做出不同的表现。

```js
const ancestors = location.ancestorOrigins;
```

### hash

> Location 接口的 hash 属性返回一个 USVString，其中会包含URL标识中的 '#' 和 后面URL片段标识符。
>
> 这里 fragment 不会经过百分比编码（URL编码）。如果 URL 中没有 fragment，该属性会包含一个空字符串，""

```js
// string = object.hash;
// object.hash = string;

<a id="myAnchor" href="/en-US/docs/Location.href#Examples">Examples</a>
<script>
  var anchor = document.getElementById("myAnchor");
  console.log(anchor.hash); // 返回'#Examples'
</script>
```

### host

> Location 接口的 host 属性是包含了主机的一段 USVString，其中包含：主机名，如果 URL 的端口号是非空的，还会跟上一个 ':' ，最后是 URL 的端口号。

```js

// string = object.host;
// object.host = string;

var anchor = document.createElement("a");

anchor.href = "https://developer.mozilla.org/en-US/Location.host"
anchor.host == "developer.mozilla.org"

anchor.href = "https://developer.mozilla.org:443/en-US/Location.host"
anchor.host == "developer.mozilla.org"
// 这里 host 中没有包含端口号，因为 443 是 https协议的默认端口号

anchor.href = "https://developer.mozilla.org:4097/en-US/Location.host"
anchor.host == "developer.mozilla.org:4097"
```

### hostname

> Location的 hostname 属性是包含了域名的一段 USVString。

```js
// string = object.hostname;
// object.hostname = string;

// 在文档流中声明了一个元素： <a id="myAnchor" href="https://developer.mozilla.org/en-US/docs/Location.hostname">
var anchor = document.getElementById("myAnchor");
var result = anchor.hostname; // Returns:'developer.mozilla.org'
```

### href

> Location 接口的 href 属性是一个字符串化转换器(stringifier), 返回一个包含了完整 URL 的 USVString 值, 且允许 href 的更新.

```js
// string = object.href;
// object.href = string;

// 假设文档中包含标签： <a id="myAnchor" href="https://developer.mozilla.org/en-US/Location/href">
var anchor = document.getElementById("myAnchor");
var result = anchor.href; // 返回: 'https://developer.mozilla.org/en-US/Location/href'
```

### origin

> The origin read-only property of the Location interface is a USVString containing the Unicode serialization of the origin of the represented URL.
>
> - for URL using the http or https, the scheme followed by '://', followed by the domain, followed by ':', followed by the port (the default port, 80 and 443 respectively, if explicitly specified);
> - for URL using file: scheme, the value is browser dependant;
> - for URL using the blob: scheme, the origin of the URL following blob:. E.g "blob:<https://mozilla.org>" will have "https://mozilla.org".
> - Note: This feature is available in Web Workers

```js
// string = object.origin;
// Copy to Clipboard
// On this page, returns the origin
var result = window.location.origin; // Returns:'https://developer.mozilla.org'
```

### password

> Deprecated: This feature is no longer recommended. Though some browsers might still support it, it may have already been removed from the relevant web standards, may be in the process of being dropped, or may only be kept for compatibility purposes. Avoid using it, and update existing code if possible; see the compatibility table at the bottom of this page to guide your decision. Be aware that this feature may cease to work at any time.
>
> The password property of the Location interface is a USVString containing the password specified before the domain name.
>
> If it is set without first setting the username property, it silently fails.

```js
// string = object.password;
// object.password = string;
// Let's <a id="myAnchor" href="https://anonymous:flabada@developer.mozilla.org/en-US/docs/location.username"> be in the document
var anchor = document.getElementByID("myAnchor");
var result = anchor.password; // Returns:'flabada'
```

### pathname

> The pathname property of the Location interface is a USVString containing the path of the URL for the location, which will be the empty string if there is no path.

```js
// string = object.pathname;
// object.pathname = string;
// Let's an <a id="myAnchor" href="/en-US/docs/Location.pathname"> element be in the document
var anchor = document.getElementById("myAnchor");
var result = anchor.pathname; // Returns:'/en-US/docs/Location.pathname'
```

### port

> The port property of the Location interface is a USVString containing the port number of the URL. If the URL does not contain an explicit port number, it will be set to ''.

```js
// string = object.port;
// object.port = string;
// Let's an <a id="myAnchor" href="https://developer.mozilla.org:443/en-US/docs/Location.port"> element be in the document
var anchor = document.getElementByID("myAnchor");
var result = anchor.port; // Returns:'443'
```

### protocol

> The protocol property of the Location interface is a USVString representing the protocol scheme of the URL, including the final ':'.

```js
// string = object.protocol;
// object.protocol = string;
// Let's an <a id="myAnchor" href="https://developer.mozilla.org/en-US/Location.protocol"> element be in the document
var anchor = document.getElementById("myAnchor");
var result = anchor.protocol; // Returns:'https:'
```

### search

> The search property of the Location interface is a search string, also called a query string; that is, a USVString containing a '?' followed by the parameters of the URL.
>
> Modern browsers provide URLSearchParams and URL.searchParams to make it easy to parse out the parameters from the querystring.

```js
string = object.search;
object.search = string;
// Let an <a id="myAnchor" href="/en-US/docs/Location.search?q=123"> element be in the document
var anchor = document.getElementById("myAnchor");
var queryString = anchor.search; // Returns:'?q=123'
// Further parsing:
let params = new URLSearchParams(queryString);
let q = parseInt(params.get("q")); // is the number 123
```

### username

> Deprecated: This feature is no longer recommended. Though some browsers might still support it, it may have already been removed from the relevant web standards, may be in the process of being dropped, or may only be kept for compatibility purposes. Avoid using it, and update existing code if possible; see the compatibility table at the bottom of this page to guide your decision. Be aware that this feature may cease to work at any time.
>
> The username property of the Location interface is a USVString containing the username specified before the domain name.

```js
string = object.username;
object.username = string;
// Let's <a id="myAnchor" href="https://anonymous:flabada@developer.mozilla.org/en-US/docs/Location.username"> be in the document
var anchor = document.getElementByID("myAnchor");
var result = anchor.username; // Returns:'anonymous'
```

## 方法

### location.assign()

> Location.assign() 方法会触发窗口加载并显示指定的URL的内容。
> 如果由于安全原因无法执行跳转，那么会抛出一个 SECURITY_ERROR 类型的 DOMException。当调用此方法的脚本来源和页面的 Location 对象中定义的来源隶属于不同域的时候，就会抛出上述错误。
> 如果传入了一个无效的 URL，则会抛出一个 SYNTAX_ERROR 类型的 DOMException。

```js
location.assign(url);
// 跳转到 Location.reload() 这篇文章
  document.location.assign('https://developer.mozilla.org/zh-CN/docs/Web/API/Location/reload');
```

### Location.reload()

> Location.reload() 方法用来刷新当前页面。该方法只有一个参数，当值为 true 时，将强制浏览器从服务器加载页面资源，当值为 false 或者未传参时，浏览器则可能从缓存中读取页面。
> 该方法在跨域调用（执行该方法的脚本文件的域和 Location 对象所在页面的跨不同）时，将会抛出 DOMException 异常。

```js
object.reload(forcedReload);

// 无缓存刷新页面（但页面引用的资源还是可能使用缓存，
// 大多数浏览器可以通过设置在打开开发者工具时禁用缓存实现无缓存需求）
window.location.reload(true);
// forcedReload 可选
// 该参数要求为 布尔 (en-US) 类型，当取值为 true 时，将强制浏览器从服务器重新获取当前页面资源，而不是从浏览器的缓存中读取，如果取值为 false 或不传该参数时，浏览器则可能会从缓存中读取当前页面。
```

### Location.replace()

> Location.replace() 方法以给定的URL来替换当前的资源。 与assign() 方法 不同的是，调用 replace() 方法后，`当前页面不会保存到会话历史`中（session History），这样，用户点击回退按钮时，将不会再跳转到该页面。
> 因违反安全规则导致的赋值失败，浏览器将会抛出类型为 SECURITY_ERROR 的 DOMException 异常。当调用该方法的脚本所属的源与拥有 Location 对象所属源不同时，通常情况会发生这种异常,此时通常该脚本是存在不同的域下。
> 如果 URL 无效，浏览器也会抛出 SYNTAX_ERROR 类型的 DOMException 异常。

```JS
// object.replace(url);
// 参数
// url
//  DOMString 类型，指定所导航到的页面的 URL 地址。
// 示例
// Navigate to the Location.reload article by replacing this page
window.location.replace('https://developer.mozilla.org/en-US/docs/Web/API/Location/reload');
```

### Location: toString()

> toString()Location接口的stringifier方法返回包含整个URL的USVString}。它是Location.href的只读版本。

```js
// string = object.toString();
// 例子
// Let's imagine an <a id="myAnchor" href="https://developer.mozilla.org/en-US/docs/Location/toString"> element is in the document
var anchor = document.getElementById("myAnchor");
var result = anchor.toString(); // Returns: 'https://developer.mozilla.org/en-US/docs/Location/toString'
```
