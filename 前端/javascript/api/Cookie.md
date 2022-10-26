# Cookie

> Cookie是服务端发送到用户浏览器并且保存到本地的一小块数据，它会在浏览器下次向同一服务器发起请求时，被携带到服务器上
> 作用:
>
> - 经常用来做一些用户会话状态管理、个性化设置等等
> - 前端可以通过document.cookie来访问cookie
> - cookie是跨域的，也就是在不同的域名中，访问的cookie的时候，只能访问对应的域名的cookie
> 特性:
> - http: 会自动懈怠Cookie
> - 携带的Cookie, 还是请求所在域名的Cookie

```js
export const setCookie = function setCookie(name, value) {
    // var Days = 30; 
    var exp = new Date();
    exp.setTime(exp.getTime() + 120 * 60 * 1000);
    document.cookie = name + "=" + escape(value) + ";expires=" + exp.toGMTString() + ";path=/";
}

//读取cookies 
export const getCookie = function getCookie(name) {
    var arr, reg = new RegExp("(^| )" + name + "=([^;]*)(;|$)");
    if (arr = document.cookie.match(reg))
        return unescape(arr[2]);
    else
        return null;
}

//删除cookies 
export const delCookie = function delCookie(name) {
    var exp = new Date();
    exp.setTime(exp.getTime() - 1);
    var cval = getCookie(name);
    if (cval != null)
        document.cookie = name + "=" + cval + ";expires=" + exp.toGMTString() + ";path=/brand";
    document.cookie = name + "=" + cval + ";expires=" + exp.toGMTString() + ";path=/   ";

}
//使用示例 
// setCookie("name","hayden"); 
// alert(getCookie("name")); 
```

## Cookie 和 CSRF

CSRF，中文名叫跨站请求伪造，发生的场景就是，用户登陆了a网站，然后跳转到b网站，b网站直接发送一个a网站的请求，进行一些危险操作，就发生了CSRF攻击！
这时候，懂得这个CSRF了吗？我认为一部分同学依然不懂，因为我看过太多这样的描述了！
因为有这么一些疑惑，为什么在b网站可以仿造a网站的请求？Cookie不是跨域的吗？什么条件下，什么场景下，会发生这样的事情？
这时候，我们要注意上面我对cookie的定义，在发送一个http请求的时候，携带的cookie是这个http请求域的地址的cookie。也就是我在b网站，发送a网站的一个请求，携带的是a网站域名下的cookie！很多同学的误解，就是觉得cookie是跨域的，b网站发送任何一个请求，我只能携带b网站域名下的cookie。
当然，我们在b网站下，读取cookie的时候，只能读取b网站域名下的cookie，这是cookie的跨域限制。所以要记住，不要把http请求携带的cookie，和当前域名的访问权限的cookie混淆在一起。
还要理解一个点：CSRF攻击，仅仅是利用了http携带cookie的特性进行攻击的，但是攻击站点还是无法得到被攻击站点的cookie。这个和XSS不同，XSS是直接通过拿到Cookie等信息进行攻击的。

### 应对CSRF攻击

#### 方案一：放弃Cookie、使用Token

由于CSRF是通过Cookie伪造请求的方式，欺骗服务器，来达到自己的目的。那么我们采取的策略就是，不使用Cookie的方式来验证用户身份，我们使用Token！
Token的策略，一般就是登陆的时候，服务端在response中，返回一个token字段，然后以后所有的通信，前端就把这个token添加到http请求的头部。
这是当前，最常用的防御CSRF攻击的策略。

#### 方案二：SameSite Cookies

前端在发展，Cookie也在进化，Cookie有一个新的属性——SateSite。能够解决CSRF攻击的问题。
它表示，只能当前域名的网站发出的http请求，携带这个Cookie。
当然，由于这是新的cookie属性，在兼容性上肯定会有问题。

#### 方案三：服务端Referer验证

我们发送的http请求中，header中会带有Referer字段，这个字段代表的是当前域的域名，服务端可以通过这个字段来判断，是不是“真正”的用户请求。
也就是说，如果b网站伪造a网站的请求，Referer字段还是表明，这个请求是b网站的。也就能辨认这个请求的真伪了。
不过，目前这种方案，使用的人比较少。可能存在的问题就是，如果连Referer字段都能伪造，怎么办？

## XSS

XSS是由于不安全的数据引起的，有可能是表单提交的数据，有可能是页面路径的参数问题。
CSRF是通过伪造http请求，来达到自己的攻击目的。但是XSS是通过盗取用户的敏感信息而达到攻击的目的。比如本地存储、用户密码、cookie等等。
比如这个不安全的数据，是一个script标签，那这个script就可以链接任意的js文件，浏览器本地就会执行这个js，那通过js我们能做的东西就太多了：
比如document.cookie，获取用户信息。
比如通过localStorage，获取本地存储的敏感信息（token）。
然后只要是这个页面展示的任何信息，我都可以获取。

### 应对XSS攻击

#### 方案一：http-only

Cookie有一个http-only属性，表示只能被http请求携带。
假如你的网站遭受到XSS攻击，攻击者就无法通过document.cookie得到你的cookie信息。

#### 方案二：正则校验

我们了解到，XSS是由于不安全的数据引起的，这些数据的来源，一个重要的渠道就是提交表单，注入到数据库。所以针对前端，我们需要把表单数据进行正则验证，通过验证之后，才能提交数据。
对于服务端，也应该对接受的数据，进行规则校验，不符合规则的数据不应该入库。从接口层面，保证数据安全。

#### 方案三：数据转义

如果无法保证数据库的数据都是安全的，前端能做的事情就是，把所有需要展示到页面的数据，进行转义，比如遇到script标签，直接replace处理。或者遇到标签标识‘<’以及‘>’这类特殊字符，添加‘\’进行处理

## Token

1、cookie可以引起csrf攻击，token在保持用户会话的时候好一点。

2、由于http请求携带cookie，当cookie过大的时候，会增大http请求的带宽。

3、cookie的特性，导致了cookie面对CSRF攻击的时候，很不安全。
