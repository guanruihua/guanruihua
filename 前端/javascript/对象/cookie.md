# Cookie

> 用户存储一些数据, 存储与电脑上的文本文件中
>
> 当 web 服务器向浏览器发送 web 页面时，在连接关闭后，服务端不会记录用户的信息。
>
> Cookie 的作用就是用于解决 "如何记录客户端的用户信息":
>
> - 当用户访问 web 页面时，他的名字可以记录在 cookie 中。
> - 在用户下一次访问该页面时，可以在 cookie 中读取用户访问记录

## 使用cookie

> - `expires`: 过期时间
> - `path`: cookie的路径

```js
document.cookie="username=John Smith; expires=Thu, 18 Dec 2043 12:00:00 GMT; path=/";
document.cookie="username2=John Smith2; expires=Thu, 18 Dec 2043 12:00:00 GMT; path=/";

getCookie("username")//John Smith
getCookie("username2")//John Smith2
document.cookie // 可以返回全部cookie
```
