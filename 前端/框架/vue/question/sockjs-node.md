# vue项目报错http://localhost:8080/sockjs-node/info?t=xxxx，解决办法，亲测绝对有效！

## 方法一: 修改node_modules文件

> 找到/node_modules/sockjs-client/dist/sockjs.js 
>
> 找到代码的 1606行 ，并注释掉

   ```js
try{
  //self.xhr.send(payload); //注释这一行
}catch(e){
  self.emit('finish', 0, '');
  self._cleanup(false);
}
   ```

## 方法二

> 其实造成这个原因是：网络环境变更（如切换wifi导致开发服务器的IP地址更换），服务器不知道如何确定访问源，就有可能造成上述情况。
>
> **解决办法**：如果是开发环境，重新npm run serve,重启项目即可解决问题。如果是生产环境，很大可能是应该是域名和ip映射出问题，检查重新配置域名和ip映射就可以了。