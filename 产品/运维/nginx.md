# Nginx
>
> [windows10下nginx的安装及使用](https://blog.csdn.net/sinat_36146776/article/details/88992345)

## 安装部署

> 1、下载完成后，解压缩，运行cmd，使用命令进行操作，不要直接双击nginx.exe，不要直接双击nginx.exe，不要直接双击nginx.exe
> 一定要在dos窗口启动，不要直接双击nginx.exe，这样会导致修改配置后重启、停止nginx无效，需要手动关闭任务管理器内的所有!nginx进程，再启动才可以
> 2、使用命令到达nginx的加压缩后的目录
`cd c:\nginx-1.15.2`
> 3、启动nginx服务，启动时会一闪而过是正常的
`start nginx`
> 4、查看任务进程是否存在，dos或打开任务管理器都行
`tasklist /fi "imagename eq nginx.exe"`

### 启动nginx服务

> 版本: 1.20.2
> 双击nginx, 或者命令行输入nginx启动

![](./__assets__/nginx-2022-03-10-11-12-24.png)

> 浏览器打开

![](./__assets__/nginx-2022-03-10-11-12-59.png)

### 错误

> 如果都没有可能是启动报错了查看一下日志，在nginx目录中的logs文件夹下error.log是日志文件
> 常见错误
>
> - 端口占用
> - nginx文件夹路径含中文

### 修改配置

![](/__assets__/img/2022-01-25-15-22-37.png)

#### conf/nginx.conf

> 端口占用可修改: `8800`(或其他没有占用的)

![](/__assets__/img/2022-01-25-15-26-47.png)

重新加载修改后配置

```shell
nginx -t -c /nginx/conf/nginx.conf
```

### 重启

> `nginx -s reload`

### 关闭nginx

> 快速停止 `nginx -s stop`
> 完整有序地关闭 `nginx -s quit`

## 文件目录

```js
[wujianrong@localhost ~]# tree /usr/local/nginx
/usr/local/nginx
├── client_body_temp
├── conf                             # Nginx所有配置文件的目录
│   ├── fastcgi.conf                 # fastcgi相关参数的配置文件
│   ├── fastcgi.conf.default         # fastcgi.conf的原始备份文件
│   ├── fastcgi_params               # fastcgi的参数文件
│   ├── fastcgi_params.default       
│   ├── koi-utf
│   ├── koi-win
│   ├── mime.types                   # 媒体类型
│   ├── mime.types.default
│   ├── nginx.conf                   # Nginx主配置文件
│   ├── nginx.conf.default
│   ├── scgi_params                  # scgi相关参数文件
│   ├── scgi_params.default  
│   ├── uwsgi_params                 # uwsgi相关参数文件
│   ├── uwsgi_params.default
│   └── win-utf
├── fastcgi_temp                     # fastcgi临时数据目录
├── html                             # Nginx默认站点目录
│   ├── 50x.html                     # 错误页面优雅替代显示文件，例如当出现502错误时会调用此页面
│   └── index.html                   # 默认的首页文件
├── logs                             # Nginx日志目录
│   ├── access.log                   # 访问日志文件
│   ├── error.log                    # 错误日志文件
│   └── nginx.pid                    # pid文件，Nginx进程启动后，会把所有进程的ID号写到此文件
├── proxy_temp                       # 临时目录
├── sbin                             # Nginx命令目录
│   └── nginx                        # Nginx的启动命令
├── scgi_temp                        # 临时目录
└── uwsgi_temp                       # 临时目录
```

### 配置文件

> conf //nginx所有配置文件目录
> nginx.conf //这个是Nginx的核心配置文件，这个文件非常重要，也是我们即将要学习的重点
> nginx.conf.default //nginx.conf的备份文件  

### 日志

> logs: 记录入门的文件，当nginx服务器启动后
> 这里面会有 access.log error.log 和nginx.pid三个文件出现。

### 资源目录

> html //存放nginx自带的两个静态的html页面
> 50x.html //访问失败后的失败页面
> index.html //成功访问的默认首页

### 备份文件

> fastcgi.conf:fastcgi  //相关配置文件
> fastcgi.conf.default //fastcgi.conf的备份文件
> fastcgi_params //fastcgi的参数文件
> fastcgi_params.default //fastcgi的参数备份文件
> scgi_params //scgi的参数文件
> scgi_params.default //scgi的参数备份文件
> uwsgi_params //uwsgi的参数文件
> uwsgi_params.default //uwsgi的参数备份文件
> mime.types //记录的是HTTP协议中的Content-Type的值和文件后缀名的对应关系
> mime.types.default //mime.types的备份文件

### 编码文件

> koi-utf、koi-win、win-utf这三个文件都是与编码转换映射相关的配置文件，
> 用来将一种编码转换成另一种编码

### 执行文件

> sbin: 是存放执行程序文件nginx

### 命令

> nginx: 是用来控制Nginx的启动和停止等相关的命令。

## 配置文件

### 配置主要结构

```shell
worker_processes  1；                   # worker进程的数量
events {                                 # 事件区块开始
 worker_connections  1024；            # 每个worker进程支持的最大连接数
}                                  # 事件区块结束
http {                              # HTTP区块开始
 include       mime.types；            # Nginx支持的媒体类型库文件
 default_type  application/octet-stream；            # 默认的媒体类型
 sendfile        on；           # 开启高效传输模式
 keepalive_timeout  65；          # 连接超时
 server {                              # 第一个Server区块开始，表示一个独立的虚拟主机站点
  listen       80；                 # 提供服务的端口，默认80
  server_name  localhost；       # 提供服务的域名主机名
  location / {                      # 第一个location区块开始
   root   html；          # 站点的根目录，相当于Nginx的安装目录
   index  index.html index.htm；        # 默认的首页文件，多个用空格分开
  }                      # 第一个location区块结果
  error_page   500502503504  /50x.html；          # 出现对应的http状态码时，使用50x.html回应客户
  location = /50x.html {                   # location区块开始，访问50x.html
   root   html；                       # 指定对应的站点目录为html
  }
 }  
 ......
```

### 主配置文件

> 通过include 引入其他(子)配置文件
>
![](/__assets__/img/2022-01-25-16-40-39.png)

### 子配置文件

![](/__assets__/img/2022-01-25-16-42-26.png)

### location 匹配

```shell
location[ = | ~ | ~* | ^~] url{

}
```

> location 指令说明，该语法用来匹配 url，语法如上：
> =：用于不含正则表达式的 url 前，要求字符串与 url 严格匹配，匹配成功就停止向下搜索并处理请求。
> ~：用于表示 url 包含正则表达式，并且区分大小写。
> ~*：用于表示 url 包含正则表达式，并且不区分大小写。
> ^~：用于不含正则表达式的 url 前，要求 Nginx 服务器找到表示 url 和字符串匹配度最高的 location 后，立即使用此 location 处理请求，而不再匹配。
> 如果有 url 包含正则表达式，不需要有 ~ 开头标识。

```shell
#优先级1,精确匹配，根路径
location =/ {
  return 400;
}

#优先级2,以某个字符串开头,以av开头的，优先匹配这里，区分大小写
location ^~ /av {
  root /data/av/;
}

#优先级3，区分大小写的正则匹配，匹配/media*****路径
location ~ /media {
 alias /data/static/;
}

#优先级4 ，不区分大小写的正则匹配，所有的****.jpg|gif|png 都走这里
location ~* .*\.(jpg|gif|png|js|css)$ {
  root  /data/av/;
}

#优先7，通用匹配
location / {
  return 403;
}

```

## 常用命令

### 启动

> `nginx` //直接nginx启动，前提是配好nginx环境变量
> `systemctl start nginx.service` //使用systemctl命令启动

### 停止

> `nginx  -s stop` //立即停止服务
> `nginx -s quit` // 从容停止服务 需要进程完成当前工作后再停止
> `killall nginx` //直接杀死nginx进程
> `systemctl stop nginx.service` //systemctl停止

### 重启

> `nginx -s reload` //重启nginx
> `systemctl reload nginx.service` //systemctl重启nginx

### 验证nginx配置文件是否正确

> `nginx -t` //输出nginx.conf syntax is ok即表示nginx的配置文件正确

## 反向代理, 负载均衡

### 默认轮询

> 每个请求按时间顺序逐一分配到不同的后端服务器，如果后端某个服务器宕机，能自动剔除故障系统。

```shell
# constPolling 作为存放负载均衡的变量
upstream constPolling {
    server localhost:10001; 
    server localhost:10002;
}
server {
    listen 10000;
    server_name localhost;
    location / {
    proxy_pass http://constPolling; #在代理的时候接入constPolling
    proxy_redirect default;
    }
}

```

### 加权轮询

> 通过设置weight，值越大分配率越大 到的访问概率越高，主要用于后端每台服务器性能不均衡的情况下。其次是为在主从的情况下设置不同的权值，达到合理有效的地利用主机资源。

```shell
# constPolling 作为存放负载均衡的变量
upstream constPolling {
    server localhost:10001 weight=1; 
    server localhost:10002 weight=2;
}
server {
    listen 10000;
    server_name localhost;
    location / {
    proxy_pass http://constPolling; #在代理的时候接入constPolling
    proxy_redirect default;
    }
}

```

> 权重越大，被访问的概率越大，比如上面就是33.33%和百分66.66%的访问概率
> 访问的效果：
> localhost:10001、localhost:10002、localhost:10002、localhost:10001、localhost:10002、localhost:10002
