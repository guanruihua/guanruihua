# <http://localhost:8080/sockjs-node/info?t=1556418283950> net:: ERR_CONNECTION_REFUSED

> sockjs-node 是一个JavaScript库，提供跨浏览器JavaScript的API，创建了一个低延迟、全双工的浏览器和web服务器之间通信通道。
>
> 服务端：sockjs-node（<https://github.com/sockjs/soc>…）
> 客户端：sockjs-client（<https://github.com/sockjs/soc>…）
>
> 开发时，如果变更网络环境（如切换wifi导致开发服务器的IP地址更换），服务器不知道如何确定访问源，就有可能造成上述情况
>

## 方法一

> 找到/node_modules/sockjs-client/dist/sockjs.js
> 找到代码的 1605行
> self.xhr.send(payload);
>
> 可在代码开发完成后关闭，会同步关闭热加载

## 方法二

> /package-lock.json
> 关闭开发环境时的 sockjs 接口
> dev 设置为false
