# node-basis

## 特点

> - 事件驱动
> - 非阻塞态IO模型(异步)
> - 轻量,高效

## Node.js Stream(流)

Stream 是一个抽象接口，Node 中有很多对象实现了这个接口。例如，对http 服务器发起请求的request 对象就是一个 Stream，还有stdout（标准输出）。

Node.js，Stream 有四种流类型：

- **Readable** - 可读操作。
- **Writable** - 可写操作。
- **Duplex** - 可读可写操作.
- **Transform** - 操作被写入数据，然后读出结果。

所有的 Stream 对象都是 EventEmitter 的实例。常用的事件有：

- **data** - 当有数据可读时触发。
- **end** - 没有更多的数据可读时触发。
- **error** - 在接收和写入过程中发生错误时触发。
- **finish** - 所有数据已被写入到底层系统时触发。

本教程会为大家介绍常用的流操作。

------

### 从流中读取数据

创建 input.txt 文件，内容如下：

```
www.sxt.com
```

创建 main.js 文件, 代码如下：

```
var fs = require("fs");
var data = '';

// 创建可读流
var readerStream = fs.createReadStream('input.txt');

// 设置编码为 utf8。
readerStream.setEncoding('UTF8');

// 处理流事件 --> data, end, and error
readerStream.on('data', function(chunk) {
   data += chunk;
});

readerStream.on('end',function(){
   console.log(data);
});

readerStream.on('error', function(err){
   console.log(err.stack);
});

console.log("程序执行完毕");
```

以上代码执行结果如下：

```
程序执行完毕地址：www.sxt.com
```

------

### 写入流

创建 main.js 文件, 代码如下：

```
var fs = require("fs");
var data = 'www.sxt.com';

// 创建一个可以写入的流，写入到文件 output.txt 中
var writerStream = fs.createWriteStream('output.txt');

// 使用 utf8 编码写入数据
writerStream.write(data,'UTF8');

// 标记文件末尾
writerStream.end();

// 处理流事件 --> data, end, and error
writerStream.on('finish', function() {
    console.log("写入完成。");
});

writerStream.on('error', function(err){
   console.log(err.stack);
});

console.log("程序执行完毕");
```

以上程序会将 data 变量的数据写入到 output.txt 文件中。代码执行结果如下：

```
$ node main.js 
程序执行完毕
写入完成。
```

查看 output.txt 文件的内容：

```
$ cat output.txt 
www.sxt.com
```

------

### 管道流

管道提供了一个输出流到输入流的机制。通常我们用于从一个流中获取数据并将数据传递到另外一个流中。

![img](https://www.sxt.com/wp-content/uploads/2015/09/bVcla61)

如上面的图片所示，我们把文件比作装水的桶，而水就是文件里的内容，我们用一根管子(pipe)连接两个桶使得水从一个桶流入另一个桶，这样就慢慢的实现了大文件的复制过程。

以下实例我们通过读取一个文件内容并将内容写入到另外一个文件中。

设置 input.txt 文件内容如下：

```
教程官网地址：www.sxt.com
管道流操作实例
```

创建 main.js 文件, 代码如下：

```
var fs = require("fs");

// 创建一个可读流
var readerStream = fs.createReadStream('input.txt');

// 创建一个可写流
var writerStream = fs.createWriteStream('output.txt');

// 管道读写操作
// 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中
readerStream.pipe(writerStream);

console.log("程序执行完毕");
```

代码执行结果如下：

```
$ node main.js 
程序执行完毕
```

查看 output.txt 文件的内容：

```
$ cat output.txt 
教程官网地址：www.sxt.com
管道流操作实例
```

------

### 链式流

链式是通过连接输出流到另外一个流并创建多个流操作链的机制。链式流一般用于管道操作。

接下来我们就是用管道和链式来压缩和解压文件。

创建 compress.js 文件, 代码如下：

```js
var fs = require("fs");
var zlib = require('zlib');

// 压缩 input.txt 文件为 input.txt.gz
fs.createReadStream('input.txt')
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream('input.txt.gz'));
  
console.log("文件压缩完成。");
```

代码执行结果如下：

```
$ node compress.js 
文件压缩完成。
```

执行完以上操作后，我们可以看到当前目录下生成了 input.txt 的压缩文件 input.txt.gz。

接下来，让我们来解压该文件，创建 decompress.js 文件，代码如下：

```
var fs = require("fs");
var zlib = require('zlib');

// 解压 input.txt.gz 文件为 input.txt
fs.createReadStream('input.txt.gz')
  .pipe(zlib.createGunzip())
  .pipe(fs.createWriteStream('input.txt'));
  
console.log("文件解压完成。");
```

代码执行结果如下：

```js
$ node decompress.js 
文件解压完成。
```

## Node.js 事件循环

Node.js 是单进程单线程应用程序，但是因为 V8 引擎提供的异步执行回调接口，通过这些接口可以处理大量的并发，所以性能非常高。

Node.js 几乎每一个 API 都是支持回调函数的。

Node.js 基本上所有的事件机制都是用设计模式中观察者模式实现。

Node.js 单线程类似进入一个while(true)的事件循环，直到没有事件观察者退出，每个异步事件都生成一个事件观察者，如果有事件发生就调用该回调函数.

```
开启进程
开启线程
初始化数据，window/document/location...
whild(true){
    
    初始化事件列表
    根据事件修改数据
    根据数据去渲染页面
    
    
    if(count=0){
        运行js代码
        btn.onclick = function(){
            document.body.style.background = "skyblue"
            console.log(123)
        }
        console.log(456)
        count++
    }
    
    
}
```

------

### 事件驱动程序

Node.js 使用事件驱动模型，当web server接收到请求，就把它关闭然后进行处理，然后去服务下一个web请求。

当这个请求完成，它被放回处理队列，当到达队列开头，这个结果被返回给用户。

这个模型非常高效可扩展性非常强，因为 webserver 一直接受请求而不等待任何读写操作。（这也称之为非阻塞式IO或者事件驱动IO）

在事件驱动模型中，会生成一个主循环来监听事件，当检测到事件时触发回调函数。

![img](https://www.sxt.com/wp-content/uploads/2015/09/event_loop.jpg)

Node.js 有多个内置的事件，我们可以通过引入 events 模块，并通过实例化 EventEmitter 类来绑定和监听事件，如下实例：

```
// 引入 events 模块
var events = require('events');
// 创建 eventEmitter 对象
var eventEmitter = new events.EventEmitter();
```

以下程序绑定事件处理程序：

```
// 绑定事件及事件的处理程序
eventEmitter.on('eventName', eventHandler);
```

我们可以通过程序触发事件：

```
// 触发事件
eventEmitter.emit('eventName');
```

### 实例

创建 main.js 文件，代码如下所示：

### 实例

```js
// 引入 events 模块
var events = require('events');
// 创建 eventEmitter 对象
var eventEmitter = new events.EventEmitter();
 
// 创建事件处理程序
var connectHandler = function connected() {
   console.log('连接成功。');
  
   // 触发 data_received 事件 
   eventEmitter.emit('data_received');
}
 
// 绑定 connection 事件处理程序
eventEmitter.on('connection', connectHandler);
 
// 使用匿名函数绑定 data_received 事件
eventEmitter.on('data_received', function(){
   console.log('数据接收成功。');
});
 
// 触发 connection 事件 
eventEmitter.emit('connection');
 
console.log("程序执行完毕。");
```

接下来让我们执行以上代码：

```
$ node main.js
连接成功。
数据接收成功。
程序执行完毕。
```

------

### Node 应用程序是如何工作的？

在 Node 应用程序中，执行异步操作的函数将回调函数作为最后一个参数， 回调函数接收错误对象作为第一个参数。

接下来让我们来重新看下前面的实例，创建一个 input.txt ,文件内容如下：

```
官网地址：www.sxt.com
```

创建 main.js 文件，代码如下：

```js
var fs = require("fs");

fs.readFile('input.txt', function (err, data) {
   if (err){
      console.log(err.stack);
      return;
   }
   console.log(data.toString());
});
console.log("程序执行完毕");
```

以上程序中 fs.readFile() 是异步函数用于读取文件。 如果在读取文件过程中发生错误，错误 err 对象就会输出错误信息。

如果没发生错误，readFile 跳过 err 对象的输出，文件内容就通过回调函数输出。

执行以上代码，执行结果如下：

```
程序执行完毕
官网地址：www.sxt.com
```

接下来我们删除 input.txt 文件，执行结果如下所示：

```
程序执行完毕
Error: ENOENT, open 'input.txt'
```

因为文件 input.txt 不存在，所以输出了错误信息。

### Node.js EventEmitter

Node.js 所有的异步 I/O 操作在完成时都会发送一个事件到事件队列。

Node.js 里面的许多对象都会分发事件：一个 net.Server 对象会在每次有新连接时触发一个事件， 一个 fs.readStream 对象会在文件被打开的时候触发一个事件。 所有这些产生事件的对象都是 events.EventEmitter 的实例。

------

### EventEmitter 类

events 模块只提供了一个对象： events.EventEmitter。EventEmitter 的核心就是事件触发与事件监听器功能的封装。

你可以通过require("events");来访问该模块。

```js
// 引入 events 模块
var events = require('events');
// 创建 eventEmitter 对象
var eventEmitter = new events.EventEmitter();
```

EventEmitter 对象如果在实例化时发生错误，会触发 error 事件。当添加新的监听器时，newListener 事件会触发，当监听器被移除时，removeListener 事件被触发。

下面我们用一个简单的例子说明 EventEmitter 的用法：

```js
//event.js 文件
var EventEmitter = require('events').EventEmitter; 
var event = new EventEmitter(); 
event.on('some_event', function() { 
    console.log('some_event 事件触发'); 
}); 
setTimeout(function() { 
    event.emit('some_event'); 
}, 1000); 
```

执行结果如下：

运行这段代码，1 秒后控制台输出了 **'some_event 事件触发'**。其原理是 event 对象注册了事件 some_event 的一个监听器，然后我们通过 setTimeout 在 1000 毫秒以后向 event 对象发送事件 some_event，此时会调用some_event 的监听器。

```
$ node event.js 
some_event 事件触发
```

EventEmitter 的每个事件由一个事件名和若干个参数组成，事件名是一个字符串，通常表达一定的语义。对于每个事件，EventEmitter 支持 若干个事件监听器。

当事件触发时，注册到这个事件的事件监听器被依次调用，事件参数作为回调函数参数传递。

让我们以下面的例子解释这个过程：

```
//event.js 文件
var events = require('events'); 
var emitter = new events.EventEmitter(); 
emitter.on('someEvent', function(arg1, arg2) { 
    console.log('listener1', arg1, arg2); 
}); 
emitter.on('someEvent', function(arg1, arg2) { 
    console.log('listener2', arg1, arg2); 
}); 
emitter.emit('someEvent', 'arg1 参数', 'arg2 参数'); 
```

执行以上代码，运行的结果如下：

```
$ node event.js 
listener1 arg1 参数 arg2 参数
listener2 arg1 参数 arg2 参数
```

以上例子中，emitter 为事件 someEvent 注册了两个事件监听器，然后触发了 someEvent 事件。

运行结果中可以看到两个事件监听器回调函数被先后调用。 这就是EventEmitter最简单的用法。

EventEmitter 提供了多个属性，如 **on** 和 **emit**。**on** 函数用于绑定事件函数，**emit** 属性用于触发一个事件。接下来我们来具体看下 EventEmitter 的属性介绍。

## http模块

开启一个本地服务器需要Node.js中`http`核心模块

1. http--模块提供了搭建本地服务器的API,首先我们在项目中引入；

```
let http = require('http')
```

引入之后我们利用http.createServer()方法得到一个服务器实例。

```
let server = http.createServer() // createServer()方法返回一个server实例，所以我们需要一个变量来接收

```

1. 经过以上两步，我们已经搭建好了一个服务器实例，然后我们给服务器实例绑定接收`request`的事情处理函数，代码如下：

```
server.on('request', (req, res) => {
  console.log(req.url) // 获取到请求的路径（请求路径永远以“/”开头）
})

// 给服务器绑定接收请求的处理事件，当服务器接收到客户端发送的请求后，会调用后面的处理函数，处理函数接收两个参数：请求信息对象，响应信息对象。
```

1. 绑定监听端口号，开启服务器。代码如下：

```
server.listen(3000, () => {
  console.log('服务器开启成功，可以通过访问http://127.0.0.1:3000/来获取数据~~')
})

// server.listen()用来绑定监听的端口号，可以传入第二个参数，当服务器开启成功后，触发后面的回调函数

```

1. 最后看到的效果如下图所示：

![node演示](D:/note/Front-end/node学习图片资源/07.png)

我们看到请求路径被打印在了CMD窗口中。

好了，经过这简单的操作是不是已经完成了一个服务器的简单搭建，接下来我们来实现一个需求：

- 当我们访问“<http://127.0.0.1:3000/login>”, 服务器返回 “login page”
- 当我们访问“<http://127.0.0.1:3000/register>”, 服务器返回 “register page”
- 当我们访问“<http://127.0.0.1:3000/>”, 服务器返回 “index page”
- 当我们访问“<http://127.0.0.1:3000/product>”, 服务器返回 **产品信息列表**

我们实现这个需求，只需要在绑定服务器监听的事件处理函数中获取到用户的请求路径，然后根据不同路径返回不同数据即可，这个也不难。详情代码看下：

```js
let http = require('http')
let server = http.createServer()

server.on('request', (req, res) => {
  let url = req.url //得到请求的路径 （请求的路径永远以‘/’开头）
  if (url === '/') {
    res.end('index page')
  } else if (url === '/login') {
    res.end('login page')
  } else if (url === '/register') {
    res.end('register page')
  } else if (url === '/product'){
    let arr = [
      {
        name: 'iphone X',
        price: 8888
      },
      {
        name: 'iphone 7',
        price: 4320
      }
    ]
    // 响应的数据类型必须是字符串或者二进制数据
    res.end(JSON.stringify(arr))
  } else {
    res.end('404 NOT found')
  }
})

server.listen(3000, () => {
  console.log('服务器启动成功了，，可以访问http://127.0.0.1:3000/啦')
})
```

最后实现的效果图如下：

![node演示](D:/note/Front-end/node学习图片资源/08.gif)

我们看到我们请求不同的路径，服务器给我们返回了不同的内容，并且显示在了网页中。

### 设置状态码和响应头

```
response.writeHead(200, { 'Content-Type': 'text/plain' });
```

### 设置响应头

```
response.setHeader('Content-Type', 'text/html');
```

### 写入内容

```
response.write(fileData);
```

### 结束响应

```
response.end();
```

### 静态服务器定义

能够根据需要请求的文件，原封不动的将服务器磁盘中的数据直接返回给到浏览器。

1. 根据设定的目录，判断用户是否请求的文件时静态文件

```
//解析路径
let urlObj = path.parse(req.url)
//判断是否请求静态文件
urlObj.dir=='/static'
```

1. 从磁盘读取静态文件并返回

```
//根据请求的后缀名，返回文件的类型
res.setHeader("content-type",getContentType(urlObj.ext))
//从服务器磁盘中读取文件，并输出到响应对象中
let rs = fs.createReadStream('./static/'+urlObj.base)
rs.pipe(res)
```

1. 如何 根据后缀名返回文件类型

```
function getContentType(extName){
    switch(extName){
        case ".jpg":
            return "image/jpeg";
        case ".html":
            return "text/html;charset=utf-8";
        case ".js":
            return "text/javascript;charset=utf-8";
        case ".json":
            return "text/json;charset=utf-8";
        case ".gif":
            return "image/gif";
        case ".css":
            return "text/css"
    }
}
```

#### 完整案例

```
//引入http模块
let http = require('http');
//创建server对象
let server = http.createServer()
//引入path模块
let path = require('path')
//引入文件模块
let fs = require('fs')
//监听客户端发送过来的请求
//req请求对象包含了请求的相关的信息
//res对象用于响应内容，可以通过这个对象帮助我们快速实现HTTP响应
server.on('request',function(req,res){
    //解析路径
    let urlObj = path.parse(req.url)
    //识别请求的路径
    //console.log(urlObj)
    //进入首页，返回首页的内容
    if(req.url=="/"){
        res.setHeader("content-type","text/html;charset=utf-8")
        res.end(`<link rel="stylesheet" href="./static/style.css"><h1>首页</h1><img src='./static/cxk.jpg'>`)
    }else if(urlObj.dir=='/static'){
        res.setHeader("content-type",getContentType(urlObj.ext))
        let rs = fs.createReadStream('./static/'+urlObj.base)
        rs.pipe(res)
    }else{
        
        res.setHeader("content-type","text/html;charset=utf-8")
        res.end("<h1>404页面找不到</h1>")
    }
    
})


function getContentType(extName){
    switch(extName){
        case ".jpg":
            return "image/jpeg";
        case ".html":
            return "text/html;charset=utf-8";
        case ".js":
            return "text/javascript;charset=utf-8";
        case ".json":
            return "text/json;charset=utf-8";
        case ".gif":
            return "image/gif";
        case ".css":
            return "text/css"
    }
}



//启动服务器，监听服务端口
server.listen(80,function(){
    console.log("服务已启动：http:127.0.0.1")
})
```

## 模板动态生成页面

1. 根据规则去解析链接，并且获取ID或者时索引值

```js
//请求路径：http://127.0.0.1/movies/0
let index = req.pathObj.base;
```

2. 根据索引获取数据

```json
let movies = [
         {
            name:"雪暴",
            brief:"电影《雪暴》讲述了在一座极北的边陲小镇，一伙穷凶极恶、作案手法老到的悍匪为抢夺黄金，打劫运金车，并借助大雪掩盖了所有犯罪痕迹。为了探求真相，警察王康浩暗地里搜集证据，熟悉地形，终于在一场灾难级的暴雪降临时，与谋财害命的悍匪发生了惊心动魄的正面对决……",
            author:"张震"
         },{
             name:"少年的你",
             brief:"陈念（周冬雨 饰）是一名即将参加高考的高三学生，同校女生胡晓蝶（张艺凡 饰）的跳楼自杀让她的生活陷入了困顿之中。胡晓蝶死后，陈念遭到了以魏莱（周也 饰）为首的三人组的霸凌，魏莱虽然表面上看来是乖巧的优等生，实际上却心思毒辣，胡晓蝶的死和她有着千丝万缕的联系。",
             author:"周冬雨 "
         }
     ]
let pageData = movies[index]
```

1. 根据模板渲染页面

```js
res.render( movies[index],'./template/index.html')
```

1. 底层需要实现渲染函数，通过正则匹配，找到需要修改的地方进行一一的修改。

```js
function render(options,path){
    fs.readFile(path,{encoding:"utf-8",flag:"r"},(err,data)=>{
        if(err){
            console.log(err)
        }else{
            console.log(data)
            let reg = /\{\{(.*?)\}\}/igs
            let result;
            while(result = reg.exec(data)){
                //去除2边的空白
                let strKey = result[1].trim()
                let strValue = options[strKey]
                data = data.replace(result[0],strValue)
            }

            this.end(data)
        }
    })
}
```

## async_promise

### 写法不同

ES5正常写法

```
getAjax(url,(res)=>{})
```

Promise

```
get(url).then((res)=>{})
```

async_await

```
(async ()=>{ 
    let res = await get(url)
})()
```

总结：

- ES5写法和promise写法，主要区别在写法的不同，可以让回调函数，划分出去在.then的函数里去执行，使得代码更加的另外，也可以将两个不同的参数，可以划分开来写。
- async和promise的区别，不要在于async时promise的语法糖，这种形式的写法在底层编译之后会自动转化成promise的写法

### Promise实现原理

promise需要实现的功能

```
function fn(resolve,reject){
    setTimeout(()=>{
        if(true){
            resolve()
        }else{
            reject()
        }
    })
}
var p1 = new LcPromise(fn)

p1.then(function(res){
    document.body.style.background = "greenyellow"
    console.log("这是成功做的事情")
    console.log(res)
})

p1.catch(function(res){
    document.body.style.background = "pink"
    console.log("这是失败做的事情")
    console.log(res)
})
```

p1promise对象发送了异步操作，必然会有1个未来事件，在未来要执行。这个过程由传入的函数对象fn执行。函数fn里必然需要由成功执行和失败执行的函数

1创建类构造对象

```
class LcPromise{
    constructor(fn) {
        //将成功的事件函数集成在successList数组里
        this.successList  = [];
        //这里将所有的失败函数集成到failList里
        this.failList = []
        //pending,fullfilled,rejected
        this.state = "pending"
        //传入的函数对象,(异步操作的函数内容)
        fn(this.resolveFn.bind(this),this.rejectFn.bind(this))
    }
}
```

构造函数的作用：

- 声明成功函数放置的数组对象
- 声明失败函数放置的数组对象
- 定义初始化状态
- 调用传入进行执行异步内容的函数（在未来有成功的结果时调用传入进去的成功函数，在未来失败时调用传入进行的失败函数）

2.传入成功或者失败时需要调用的函数

```js
class LcPromise{
    constructor(fn) {

        //将成功的事件函数集成在successList数组里
        this.successList  = [];
        //这里将所有的失败函数集成到failList里
        this.failList = []
        //pending,fullfilled,rejected
        this.state = "pending"
        //传入的函数对象,(异步操作的函数内容)
        fn(this.resolveFn.bind(this),this.rejectFn.bind(this))
    }
    then(successFn,failFn){
        if(typeof successFn=='function'){
            this.successList.push(successFn)
        }
        if(typeof failFn=='function'){
            this.failList.push(failFn)
        }
    }
    catch(failFn){
        if(typeof failFn=='function'){
            this.failList.push(failFn)
        }
    }
}
```

作用：

- 将成功和失败的函数传入值成功和失败的数组里

定义调用成功和失败的函数

```js
//promise aysnc await proxy Iteratror
class LcPromise{
  constructor(fn) {

    //将成功的事件函数集成在successList数组里
    this.successList  = [];
    //这里将所有的失败函数集成到failList里
    this.failList = []
    //pending,fullfilled,rejected
    this.state = "pending"
    //传入的函数对象,(异步操作的函数内容)
    fn(this.resolveFn.bind(this),this.rejectFn.bind(this))
  }
  then(successFn,failFn){
    if(typeof successFn=='function'){
      this.successList.push(successFn)
    }
    if(typeof failFn=='function'){
      this.failList.push(failFn)
    }
  }
  catch(failFn){
    if(typeof failFn=='function'){
      this.failList.push(failFn)
    }
  }
  resolveFn(res){
    this.state = "fullfilled"
    this.successList.forEach(function(item,index){
      //将成功的事件循环调用
      item(res)
    })
  }
  rejectFn(res){
    this.state = 'rejected'
    //注册到的失败所有事件进行调用
    this.failList.forEach(function(item,index){
      item(res)
    })

    throw Error(res);
  }

}
```

作用：

- 成功时调用成功数组里所有的函数，失败时调用失败数组里所有的函数。

### 应用

如何将promise与async和await结合使用

典型异步读写的回调操作

```js
fs.readFile(path,{flag:'r',encoding:"utf-8"},function(err,data){
    if(err){
        //console.log(err)
        //失败执行的内容
        reject(err)

    }else{
        //console.log(data)
        //成功执行的内容
        resolve(data)
    }
    //console.log(456)
})

```

转换成promise对象

```
new Promise(function(resolve,reject){
    fs.readFile(path,{flag:'r',encoding:"utf-8"},function(err,data){
        if(err){
            reject(err)
        }else{
            resolve(data)
        }
    })
})
```

由于每次使用，都不想写这么多代码，那么就会把这样的写法直接进行函数的封装

```
function fsRead(path){
    return new Promise(function(resolve,reject){
        fs.readFile(path,{flag:'r',encoding:"utf-8"},function(err,data){
            if(err){
                reject(err)
            }else{
                resolve(data)
            }
        })
    })
}

```

使用的时候，就可以使用promise写法

```
p1 = fsRead(path) //就可以得到promise对象
p1.then(function(data){
    console.log('输出数据:',data)
})
```

asycn_await写法

```
(async ()=>{ 
    let data = await fsRead(path)
})()
```

异步async函数调用之后也是一个promise对象

```
(async ()=>{ 
    async function test(){
        let data = await fsRead(path)
        return data;
    }
    let p = test()//异步函数调用后，也是一个promise对象
    p.then(function(data){
        console.log(data)
    })
    let a = await test()//异步函数调用后，也是一个promise对象
    
    console.log(123)
})()
```

## 梳理框架流程

### 1浏览器发送请求

1. 用户输入网址地址

```
http://127.0.0.1/
```

1. 浏览器根据请求转变成HTTP的请求包

```
GET / HTTP/1.1
Host: 127.0.0.1
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/76.0.3809.132 Safari/537.36
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Sec-Fetch-Site: none
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh;q=0.9
```

### 2服务器接受到请求

​ 1. http模块里中实例化的server对象，server对象监听每一次浏览器发送过来的请求，每次的请求都会触发`request`事件

```
this.server.on('request',(req,res)=>{})
```

1. 将HTTP的请求包转化成req的请求对象，并且传入到请求事件触发的函数中。
2. 会创建生成1个res响应对象，这个对象可以帮助我们快速的实现HTTP的响应

### 3解析请求路径，调用不同的页面渲染函数

1. 正则匹配方式进行对路径的匹配
2. 以匹配的正则字符串作为KEY，找到需要调用执行的渲染函数

```
//循环匹配正则路径
for(let key in this.reqEvent){

    let regStr = key
    let reg = new RegExp(regStr,'igs');
    console.log(regStr,reg)
    if(reg.test(req.url)){
        this.reqEvent[key](req,res)
        resState = true
        break;
    }
}
```

1. 调用页面的执行函数

```
app.on('/movies/[01]',(req,res)=>{})//这里的箭头函数即为真正匹配到的页面时执行的函数
```

1. 调用模板的渲染函数

```
res.render(movies[index],'./template/index0.html')
```

1. 执行渲染函数

```
function render(options,path){
    fs.readFile(path,{encoding:"utf-8",flag:"r"},(err,data)=>{
        if(err){
            console.log(err)
        }else{
            //数组变量的替换
            data = replaceArr(data,options)
            //单个变量的替换
            data = replaceVar(data,options)
            //最终输出渲染出来的HTML
            this.end(data)
        }
    })
}
```

1. 数组变量的替换

```
function replaceArr(data,options){
    //匹配循环的变量，并且替换循环的内容
    let reg = /\{\%for \{(.*?)\} \%\}(.*?)\{\%endfor\%\}/igs
    while(result = reg.exec(data)){
        let strKey = result[1].trim();//提取变量时，去掉左右两边的空格
        //通过KEY值获取数组内容
        let strValueArr = options[strKey]
        let listStr = ""
        strValueArr.forEach((item,i)=>{
            //替换每一项内容里的变量
            listStr = listStr + replaceVar(result[2],{"item":item})
        })
        data = data.replace(result[0],listStr)
    }
    return data;
}
```

1. 单个变量的替换

```
function replaceVar(data,options){
    let reg = /\{\{(.*?)\}\}/igs
    let result;
    console.log(options)
    while(result = reg.exec(data)){
        //去除2边的空白
        let strKey = result[1].trim()
        
        console.log(strKey)// item,item.abc
        //options.item
        let strValue = eval('options.'+strKey);//执行字符串作为JS表达式，并将计算出来的结果返回
        data = data.replace(result[0],strValue)
    }
    return data
}
```

### 4如果是请求静态文件，那么就按照静态文件的形式输出

1. 首先判断是否响应过，如果没有响应过，可以判断是否为静态文件，如果是静态文件就正常的输出
2. 否则，就输出404

```
if(!resState){
    if(pathObj.dir==this.staticDir){
        res.setHeader("content-type",this.getContentType(pathObj.ext))
        let rs = fs.createReadStream('./static/'+pathObj.base)
        rs.pipe(res)
    }else{
        res.setHeader("content-type","text/html;charset=utf-8")
        res.end("<h1>404!页面找不到</h1>")
    }
}
```

### 5RES响应对象将res设置的内容最终转化成http的响应包

```
HTTP/1.1 200 OK
content-type: text/html;charset=utf-8
Date: Sat, 30 Nov 2019 07:01:36 GMT
Connection: keep-alive
Content-Length: 46

<h1>这是首页</h1><img src='./abc/cxk.jpg'>
```

### 6浏览器解析响应包，并将html渲染在页面上

## 正则路由的设定

要求：可以根据自己设定的正则匹配路径来执行相对应的函数来响应用户的内容。

### 1.设定正则的匹配路径和响应的执行函数

```
app.on('^/$',(req,res)=>{
    res.setHeader("content-type","text/html;charset=utf-8");
    res.end("<h1>这是首页</h1><img src='./abc/cxk.jpg'>")
})
```

### 2. 获取正则路径创建正则对象

```
let reg = new RegExp(regStr,'igs');
```

### 3.匹配路径，并调用相对应的函数

```
if(reg.test(req.url)){
    this.reqEvent[key](req,res)
    resState = true
    break;
}
```

### 4.判断是否正则路径响应过，如果响应过，将不再响应，不能重复响应，会报错

```javascript
if(!resState){
    if(pathObj.dir==this.staticDir){
        res.setHeader("content-type",this.getContentType(pathObj.ext))
        let rs = fs.createReadStream('./static/'+pathObj.base)
        rs.pipe(res)
    }else{
        res.setHeader("content-type","text/html;charset=utf-8")
        res.end("<h1>404!页面找不到</h1>")
    }
}
```

## Node总结

Node：一门后端语言（服务器端的程序语言），能够连接数据库存取数据，能够接受和处理网络请求（服务器的响应，发送请求去获取数据），单线程事件驱动，异步执行，不等待，提高IO（input和ouput）的处理速度和效率。

服务器：本质上是一台PC主机（linux系统，windows系统），部署了后端语言的执行环境，并且能够长时间提供网络服务。

### 事件驱动

node本身提供了事件对象，帮助我们快速订阅者模式，或者观察者模式，或者事件模式。

```js
//事件的订阅
event.on(‘林俊杰演唱会’，()=>{订阅门票})
//事件的触发
event.emit(‘林俊杰演唱会’)
```

### 读写事件

```js
fs.readfile('path',读取配置,(err,data)=>{})
fs.writeFile('path',写入数据，写入配置，()=>{})
```

### 读写的promise封装

```javascript
let fs = require('fs')
function fsRead(path){
    return new Promise(function(resolve,reject){
        fs.readFile(path,{flag:'r',encoding:"utf-8"},function(err,data){
            if(err){
                //console.log(err)
                //失败执行的内容
                reject(err)

            }else{
                //console.log(data)
                //成功执行的内容
                resolve(data)
            }
            //console.log(456)
        })
    })
}


function fsWrite(path,content){
    return new Promise(function(resolve,reject){
        fs.writeFile(path,content,{flag:"a",encoding:"utf-8"},function(err){
            if(err){
                //console.log("写入内容出错")
                reject(err)
            }else{
                resolve(err)
                //console.log("写入内容成功")
            }
        })
    })
}

function fsDir(path){
    return new Promise(function(resolve,reject){
        fs.mkdir(path,function(err){
            if(err){
                reject(err)
            }else{
                resolve("成功创建目录")
            }
        })
    })
}

module.exports = {fsRead,fsWrite,fsDir}
```

#### 使用方式

```
(async function(){
 let data = await fsRead('path')
})()
```

### 网络请求数据

request,axios:效率比较高，单局限性比较大

puppeteer:效率低，局限性比较小

重点掌握的是：页面的分析，数据存放的位置，以及响应内容。

### 网络响应数据

http.createServer：就可以创建1个服务器去监听某个端口，并且通过请求事件来处理每个发送过来的请求。

server.on('request',(req,res)=>{

​ req:请求数据都会放在请求对象里

​ res：能够做出响应对象

})

### 路由

根据不同的路径去响应不同的内容

```javascript
//循环匹配正则路径
for(let key in this.reqEvent){
    res.setHeader("content-type","text/html;charset=utf-8")
    let regStr = key
    let reg = new RegExp(regStr,'igs');
    //console.log(regStr,reg)
    if(reg.test(req.url)){
        this.reqEvent[key](req,res)
        resState = true
        break;
    }
}
```

### 模板

会有个固定样式和结构的HTML模板，根据请求的数据不同，显示页面内容。例如新闻网站

```javascript
function render(options,path){
    fs.readFile(path,{encoding:"utf-8",flag:"r"},(err,data)=>{
        if(err){
            console.log(err)
        }else{
            try {
                data = replaceArr(data,options)
                data = replaceVar(data,options)
            } catch (error) {
               console.log(error)     
            }

            this.end(data)
        }
    })
}
```

## 回调函数

> - 异步变成的直接体现就是回调
>
> - node所有api都支持回调函数

```js
function foo(value, callback1, callback) { }
```

### 阻塞代码实例

input.txt

```txt
grhgrh
```

main.js

```js
var fs = require('fs');
var data = fs.readFileSync('input.txt');
console.log(data.toString());
console.log("程序执行结束!");
```

执行代码

```cmd
$ node main.js
grhgrh

程序执行结束!
```

### 非阻塞态实例

input.txt

```txt
grhgrh
```

main.js

```js
var fs = require('fs');
fs.readFile('input.txt', function(err, data){
  if(err) return console.log(err);
  consolo.log(data.toString());
});
console.log("程序执行结束!");
```

执行代码

```cmd
$ node main.js
程序执行结束!
grhgrh
```

## 事件循环

> - node事件机制都是用观察者设计模式来实现的
> - 单线程进入while(true)的时间循环,知道没有事件观察者退出,每个异步时间都生成一个时间观察者,没有事件发生就调用该回调函数

### 事件驱动程序

> - 当web server接收到请求，就把它关闭然后进行处理，然后去服务下一个web请求。
>
> - 当这个请求完成，它被放回处理队列，当到达队列开头，这个结果被返回给用户

<img src="https://images.gitee.com/uploads/images/2020/0601/115845_6c7a6287_6545143.png" style="zoom:50%;" />

```js
// 引入 events 模块
var events = require('events');
// 创建 eventEmitter 对象
var eventEmitter = new events.EventEmitter();
 
// 创建事件处理程序
var connectHandler = function connected() {
   console.log('连接成功。');
  
   // 触发 data_received 事件 
   eventEmitter.emit('data_received');
}
 
// 绑定 connection 事件处理程序
eventEmitter.on('connection', connectHandler);
 
// 使用匿名函数绑定 data_received 事件
eventEmitter.on('data_received', function(){
   console.log('数据接收成功。');
});
 
// 触发 connection 事件 
eventEmitter.emit('connection');
 
console.log("程序执行完毕。");

执行结果:
$ node main.js
连接成功。
数据接收成功。
程序执行完毕

```

## EventEmitter

> - Node.js 所有的异步 I/O 操作在完成时都会发送一个事件到事件队列。
>
> - Node.js 里面的许多对象都会分发事件：
>   - 一个 net.Server 对象会在每次有新连接时触发一个事件，
>   - 一个 fs.readStream 对象会在文件被打开的时候触发一个事件。 所有这些产生事件的对象都是 events.EventEmitter 的实例。

----

```js
//event.js 文件
var events = require('events'); 
var emitter = new events.EventEmitter(); 
emitter.on('someEvent', function(arg1, arg2) { 
    console.log('listener1', arg1, arg2); 
}); 
emitter.on('someEvent', function(arg1, arg2) { 
    console.log('listener2', arg1, arg2); 
}); 
emitter.emit('someEvent', 'arg1 参数', 'arg2 参数'); 

//执行后
$ node event.js 
listener1 arg1 参数 arg2 参数
listener2 arg1 参数 arg2 参数
```

> emitter 为事件 someEvent 注册了两个事件监听器，然后触发了 someEvent 事件

### 方法

| 序号 | 方法 & 描述                                                                                                                                                                                                                                                                                                                     |
| :--- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1    | **addListener(event, listener)** 为指定事件添加一个监听器到监听器数组的尾部。                                                                                                                                                                                                                                                   |
| 2    | **on(event, listener)** 为指定事件注册一个监听器，接受一个字符串 event 和一个回调函数。`server.on('connection', function (stream) {  console.log('someone connected!'); });`                                                                                                                                                    |
| 3    | **once(event, listener)** 为指定事件注册一个单次监听器，即 监听器最多只会触发一次，触发后立刻解除该监听器。`server.once('connection', function (stream) {  console.log('Ah, we have our first user!'); });`                                                                                                                     |
| 4    | **removeListener(event, listener)** 移除指定事件的某个监听器，监听器必须是该事件已经注册过的监听器。它接受两个参数，第一个是事件名称，第二个是回调函数名称。`var callback = function(stream) {  console.log('someone connected!'); }; server.on('connection', callback); // ... server.removeListener('connection', callback);` |
| 5    | **removeAllListeners([event])** 移除所有事件的所有监听器， 如果指定事件，则移除指定事件的所有监听器。                                                                                                                                                                                                                           |
| 6    | **setMaxListeners(n)** 默认情况下， EventEmitters 如果你添加的监听器超过 10 个就会输出警告信息。 setMaxListeners 函数用于提高监听器的默认限制的数量。                                                                                                                                                                           |
| 7    | **listeners(event)** 返回指定事件的监听器数组。                                                                                                                                                                                                                                                                                 |
| 8    | **emit(event, [arg1], [arg2], [...])** 按监听器的顺序执行执行每个监听器，如果事件有注册监听返回 true，否则返回 false。                                                                                                                                                                                                          |

### 事件

| 序号 | 事件 & 描述                                                                                                                                                                    |
| :--- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | **newListener** **event** - 字符串，事件名称**listener** - 处理事件函数该事件在添加新监听器时被触发。                                                                          |
| 2    | **removeListener** **event** - 字符串，事件名称**listener** - 处理事件函数从指定监听器数组中删除一个监听器。需要注意的是，此操作将会改变处于被删监听器之后的那些监听器的索引。 |

### 实例

main.js

```js
var events = require('events');
var eventEmitter = new events.EventEmitter();

// 监听器 #1
var listener1 = function listener1() {
   console.log('监听器 listener1 执行。');
}

// 监听器 #2
var listener2 = function listener2() {
  console.log('监听器 listener2 执行。');
}

// 绑定 connection 事件，处理函数为 listener1 
eventEmitter.addListener('connection', listener1);

// 绑定 connection 事件，处理函数为 listener2
eventEmitter.on('connection', listener2);

var eventListeners = eventEmitter.listenerCount('connection');
console.log(eventListeners + " 个监听器监听连接事件。");

// 处理 connection 事件 
eventEmitter.emit('connection');

// 移除监绑定的 listener1 函数
eventEmitter.removeListener('connection', listener1);
console.log("listener1 不再受监听。");

// 触发连接事件
eventEmitter.emit('connection');

eventListeners = eventEmitter.listenerCount('connection');
console.log(eventListeners + " 个监听器监听连接事件。");

console.log("程序执行完毕。");

执行结果:

$ node main.js
  2 个监听器监听连接事件。
  监听器 listener1 执行。
  监听器 listener2 执行。
  listener1 不再受监听。
  监听器 listener2 执行。
  1 个监听器监听连接事件。
  程序执行完毕。

```

----

## Buffer(缓冲区)

> - JavaScript 语言自身只有字符串数据类型，没有二进制数据类型。
>
> - 但在处理像TCP流或文件流时，必须使用到二进制数据。因此在 Node.js中，定义了一个 Buffer 类，该类用来创建一个专门存放二进制数据的缓存区。
>
> - 在 Node.js 中，Buffer 类是随 Node 内核一起发布的核心库。Buffer 库为 Node.js 带来了一种存储原始数据的方法，可以让 Node.js 处理二进制数据，每当需要在 Node.js 中处理I/O操作中移动的数据时，就有可能使用 Buffer 库。原始数据存储在 Buffer 类的实例中。一个 Buffer 类似于一个整数数组，但它对应于 V8 堆内存之外的一块原始内存。

### node支持的字符编码

> - **ascii** - 仅支持 7 位 ASCII 数据。如果设置去掉高位的话，这种编码是非常快的。
> - ==**utf8** - 多字节编码的 Unicode 字符。许多网页和其他文档格式都使用 UTF-8 。==
> - **utf16le** - 2 或 4 个字节，小字节序编码的 Unicode 字符。支持代理对（U+10000 至 U+10FFFF）。
> - **ucs2** - **utf16le** 的别名。
> - **base64** - Base64 编码。
> - **latin1** - 一种把 **Buffer** 编码成一字节编码的字符串的方式。
> - **binary** - **latin1** 的别名。
> - **hex** - 将每个字节编码为两个十六进制字符。

eg:

```js
const buf = Buffer.from('runoob', 'ascii');

// 输出 72756e6f6f62
console.log(buf.toString('hex'));

// 输出 cnVub29i
console.log(buf.toString('base64'));
```

### 创建Buffer类

> Buffer 提供了以下 API 来创建 Buffer 类：
>
> - **Buffer.alloc(size[, fill[, encoding]])：** 返回一个指定大小的 Buffer 实例，如果没有设置 fill，则默认填满 0
> - **Buffer.allocUnsafe(size)：** 返回一个指定大小的 Buffer 实例，但是它不会被初始化，所以它可能包含敏感的数据
> - **Buffer.allocUnsafeSlow(size)**
> - **Buffer.from(array)：** 返回一个被 array 的值初始化的新的 Buffer 实例（传入的 array 的元素只能是数字，不然就会自动被 0 覆盖）
> - **Buffer.from(arrayBuffer[, byteOffset[, length]])：** 返回一个新建的与给定的 ArrayBuffer 共享同一内存的 Buffer。
> - **Buffer.from(buffer)：** 复制传入的 Buffer 实例的数据，并返回一个新的 Buffer 实例
> - **Buffer.from(string[, encoding])：** 返回一个被 string 的值初始化的新的 Buffer 实例

```js
// 创建一个长度为 10、且用 0 填充的 Buffer。
const buf1 = Buffer.alloc(10);

// 创建一个长度为 10、且用 0x1 填充的 Buffer。 
const buf2 = Buffer.alloc(10, 1);

// 创建一个长度为 10、且未初始化的 Buffer。
// 这个方法比调用 Buffer.alloc() 更快，
// 但返回的 Buffer 实例可能包含旧数据，
// 因此需要使用 fill() 或 write() 重写。
const buf3 = Buffer.allocUnsafe(10);

// 创建一个包含 [0x1, 0x2, 0x3] 的 Buffer。
const buf4 = Buffer.from([1, 2, 3]);

// 创建一个包含 UTF-8 字节 [0x74, 0xc3, 0xa9, 0x73, 0x74] 的 Buffer。
const buf5 = Buffer.from('tést');

// 创建一个包含 Latin-1 字节 [0x74, 0xe9, 0x73, 0x74] 的 Buffer。
const buf6 = Buffer.from('tést', 'latin1');
```

### 写入缓存区

```js
buf.write(string[, offset[, length]][, encoding])
```

> - **string** - 写入缓冲区的字符串。
> - **offset** - 缓冲区开始写入的索引值，默认为 0 。
> - **length** - 写入的字节数，默认为 buffer.length
> - **encoding** - 使用的编码。默认为 'utf8' 。

```js
buf = Buffer.alloc(256);
len = buf.write("www.runoob.com");

console.log("写入字节数 : "+  len);
```

执行以上代码，输出结果为：

```js
$node main.js
写入字节数 : 14
```

### 从缓存区读取数据

```js
buf.toString([encoding[, start[, end]]])
```

> - **encoding** - 使用的编码。默认为 'utf8' 。
> - **start** - 指定开始读取的索引位置，默认为 0。
> - **end** - 结束位置，默认为缓冲区的末尾。

```js
buf = Buffer.alloc(26);
for (var i = 0 ; i < 26 ; i++) {
  buf[i] = i + 97;
}

console.log( buf.toString('ascii'));       // 输出: abcdefghijklmnopqrstuvwxyz
console.log( buf.toString('ascii',0,5));   //使用 'ascii' 编码, 并输出: abcde
console.log( buf.toString('utf8',0,5));    // 使用 'utf8' 编码, 并输出: abcde
console.log( buf.toString(undefined,0,5)); // 使用默认的 'utf8' 编码, 并输出: abcde

$ node main.js
abcdefghijklmnopqrstuvwxyz
abcde
abcde
abcde
```

### 将Buffer转换为JSON对象

```js
buf.toJSON()
```

> 当字符串化一个 Buffer 实例时，`JSON.stringify()`会隐式地调用该 **toJSON()**。

```js
const buf = Buffer.from([0x1, 0x2, 0x3, 0x4, 0x5]);
const json = JSON.stringify(buf);

// 输出: {"type":"Buffer","data":[1,2,3,4,5]}
console.log(json);

const copy = JSON.parse(json, (key, value) => {
  return value && value.type === 'Buffer' ?
    Buffer.from(value.data) :
    value;
});

// 输出: <Buffer 01 02 03 04 05>
console.log(copy);

执行结果
{"type":"Buffer","data":[1,2,3,4,5]}
<Buffer 01 02 03 04 05>

```

### 缓存区合并

```js
Buffer.concat(list[, totalLength])
```

> - **list** - 用于合并的 Buffer 对象数组列表。
> - **totalLength** - 指定合并后Buffer对象的总长度。

eg;

```js
var buffer1 = Buffer.from(('grh'));
var buffer2 = Buffer.from(('www.runoob.com'));
var buffer3 = Buffer.concat([buffer1,buffer2]);
console.log("buffer3 内容: " + buffer3.toString());

输出:
buffer3 内容: grhwww.runoob.com
```

### 缓存区比较

```js
buf.compare(otherBuffer);
```

> **otherBuffer** - 与 **buf** 对象比较的另外一个 Buffer 对象。//返回一个数字

eg:

```js
var buffer1 = Buffer.from('ABC');
var buffer2 = Buffer.from('ABCD');
var result = buffer1.compare(buffer2);

if(result < 0) {
   console.log(buffer1 + " 在 " + buffer2 + "之前");
}else if(result == 0){
   console.log(buffer1 + " 与 " + buffer2 + "相同");
}else {
   console.log(buffer1 + " 在 " + buffer2 + "之后");
}

输出
ABC在ABCD之前

```

### 拷贝缓存区

```js
buf.copy(targetBuffer[, targetStart[, sourceStart[, sourceEnd]]])
```

> - **targetBuffer** - 要拷贝的 Buffer 对象。
> - **targetStart** - 数字, 可选, 默认: 0
> - **sourceStart** - 数字, 可选, 默认: 0
> - **sourceEnd** - 数字, 可选, 默认: buffer.length
> - 没有返回值

eg:

```js
var buf1 = Buffer.from('abcdefghijkl');
var buf2 = Buffer.from('RUNOOB');

//将 buf2 插入到 buf1 指定位置上
buf2.copy(buf1, 2);

console.log(buf1.toString());

结果:abRUNOOBijkl
```

### 缓存区裁剪

```js
buf.slice([start[, end]])
```

> - **start** - 数字, 可选, 默认: 0
> - **end** - 数字, 可选, 默认: buffer.length

```js
var buffer1 = Buffer.from('runoob');
// 剪切缓冲区
var buffer2 = buffer1.slice(0,2);
console.log("buffer2 content: " + buffer2.toString());

结果:buffer2 content: ru
```

### 缓存区长度 buf.length

### 方法参考手册

| 序号 | 方法 & 描述                                                                                                                                                                                                                                                                                                                                                                                                                          |
| :--- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | **new Buffer(size)** 分配一个新的 size 大小单位为8位字节的 buffer。 注意, size 必须小于 kMaxLength，否则，将会抛出异常 RangeError。废弃的: 使用 Buffer.alloc() 代替（或 Buffer.allocUnsafe()）。                                                                                                                                                                                                                                     |
| 2    | **new Buffer(buffer)** 拷贝参数 buffer 的数据到 Buffer 实例。废弃的: 使用 Buffer.from(buffer) 代替。                                                                                                                                                                                                                                                                                                                                 |
| 3    | **new Buffer(str[, encoding])** 分配一个新的 buffer ，其中包含着传入的 str 字符串。 encoding 编码方式默认为 'utf8'。 废弃的: 使用 Buffer.from(string[, encoding]) 代替。                                                                                                                                                                                                                                                             |
| 4    | **buf.length** 返回这个 buffer 的 bytes 数。注意这未必是 buffer 里面内容的大小。length 是 buffer 对象所分配的内存数，它不会随着这个 buffer 对象内容的改变而改变。                                                                                                                                                                                                                                                                    |
| 5    | **buf.write(string[, offset[, length]][, encoding])** 根据参数 offset 偏移量和指定的 encoding 编码方式，将参数 string 数据写入buffer。 offset 偏移量默认值是 0, encoding 编码方式默认是 utf8。 length 长度是将要写入的字符串的 bytes 大小。 返回 number 类型，表示写入了多少 8 位字节流。如果 buffer 没有足够的空间来放整个 string，它将只会只写入部分字符串。 length 默认是 buffer.length - offset。 这个方法不会出现写入部分字符。 |
| 6    | **buf.writeUIntLE(value, offset, byteLength[, noAssert])** 将 value 写入到 buffer 里， 它由 offset 和 byteLength 决定，最高支持 48 位无符号整数，小端对齐，例如： `const buf = Buffer.allocUnsafe(6); buf.writeUIntLE(0x1234567890ab, 0, 6); // 输出:  console.log(buf);`noAssert 值为 true 时，不再验证 value 和 offset 的有效性。 默认是 false。                                                                                   |
| 7    | **buf.writeUIntBE(value, offset, byteLength[, noAssert])** 将 value 写入到 buffer 里， 它由 offset 和 byteLength 决定，最高支持 48 位无符号整数，大端对齐。noAssert 值为 true 时，不再验证 value 和 offset 的有效性。 默认是 false。`const buf = Buffer.allocUnsafe(6); buf.writeUIntBE(0x1234567890ab, 0, 6); // 输出:  console.log(buf);`                                                                                          |
| 8    | **buf.writeIntLE(value, offset, byteLength[, noAssert])** 将value 写入到 buffer 里， 它由offset 和 byteLength 决定，最高支持48位有符号整数，小端对齐。noAssert 值为 true 时，不再验证 value 和 offset 的有效性。 默认是 false。                                                                                                                                                                                                      |
| 9    | **buf.writeIntBE(value, offset, byteLength[, noAssert])** 将value 写入到 buffer 里， 它由offset 和 byteLength 决定，最高支持48位有符号整数，大端对齐。noAssert 值为 true 时，不再验证 value 和 offset 的有效性。 默认是 false。                                                                                                                                                                                                      |
| 10   | **buf.readUIntLE(offset, byteLength[, noAssert])** 支持读取 48 位以下的无符号数字，小端对齐。noAssert 值为 true 时， offset 不再验证是否超过 buffer 的长度，默认为 false。                                                                                                                                                                                                                                                           |
| 11   | **buf.readUIntBE(offset, byteLength[, noAssert])** 支持读取 48 位以下的无符号数字，大端对齐。noAssert 值为 true 时， offset 不再验证是否超过 buffer 的长度，默认为 false。                                                                                                                                                                                                                                                           |
| 12   | **buf.readIntLE(offset, byteLength[, noAssert])** 支持读取 48 位以下的有符号数字，小端对齐。noAssert 值为 true 时， offset 不再验证是否超过 buffer 的长度，默认为 false。                                                                                                                                                                                                                                                            |
| 13   | **buf.readIntBE(offset, byteLength[, noAssert])** 支持读取 48 位以下的有符号数字，大端对齐。noAssert 值为 true 时， offset 不再验证是否超过 buffer 的长度，默认为 false。                                                                                                                                                                                                                                                            |
| 14   | **buf.toString([encoding[, start[, end]]])** 根据 encoding 参数（默认是 'utf8'）返回一个解码过的 string 类型。还会根据传入的参数 start (默认是 0) 和 end (默认是 buffer.length)作为取值范围。                                                                                                                                                                                                                                        |
| 15   | **buf.toJSON()** 将 Buffer 实例转换为 JSON 对象。                                                                                                                                                                                                                                                                                                                                                                                    |
| 16   | **buf[index]** 获取或设置指定的字节。返回值代表一个字节，所以返回值的合法范围是十六进制0x00到0xFF 或者十进制0至 255。                                                                                                                                                                                                                                                                                                                |
| 17   | **buf.equals(otherBuffer)** 比较两个缓冲区是否相等，如果是返回 true，否则返回 false。                                                                                                                                                                                                                                                                                                                                                |
| 18   | **buf.compare(otherBuffer)** 比较两个 Buffer 对象，返回一个数字，表示 buf 在 otherBuffer 之前，之后或相同。                                                                                                                                                                                                                                                                                                                          |
| 19   | **buf.copy(targetBuffer[, targetStart[, sourceStart[, sourceEnd]]])** buffer 拷贝，源和目标可以相同。 targetStart 目标开始偏移和 sourceStart 源开始偏移默认都是 0。 sourceEnd 源结束位置偏移默认是源的长度 buffer.length 。                                                                                                                                                                                                          |
| 20   | **buf.slice([start[, end]])** 剪切 Buffer 对象，根据 start(默认是 0 ) 和 end (默认是 buffer.length ) 偏移和裁剪了索引。 负的索引是从 buffer 尾部开始计算的。                                                                                                                                                                                                                                                                         |
| 21   | **buf.readUInt8(offset[, noAssert])** 根据指定的偏移量，读取一个无符号 8 位整数。若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 如果这样 offset 可能会超出buffer 的末尾。默认是 false。                                                                                                                                                                                                                                      |
| 22   | **buf.readUInt16LE(offset[, noAssert])** 根据指定的偏移量，使用特殊的 endian 字节序格式读取一个无符号 16 位整数。若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 offset 可能会超出 buffer 的末尾。默认是 false。                                                                                                                                                                                                     |
| 23   | **buf.readUInt16BE(offset[, noAssert])** 根据指定的偏移量，使用特殊的 endian 字节序格式读取一个无符号 16 位整数，大端对齐。若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 offset 可能会超出 buffer 的末尾。默认是 false。                                                                                                                                                                                           |
| 24   | **buf.readUInt32LE(offset[, noAssert])** 根据指定的偏移量，使用指定的 endian 字节序格式读取一个无符号 32 位整数，小端对齐。 若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 offset 可能会超出buffer 的末尾。默认是 false。                                                                                                                                                                                           |
| 25   | **buf.readUInt32BE(offset[, noAssert])** 根据指定的偏移量，使用指定的 endian 字节序格式读取一个无符号 32 位整数，大端对齐。 若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 offset 可能会超出buffer 的末尾。默认是 false。                                                                                                                                                                                           |
| 26   | **buf.readInt8(offset[, noAssert])** 根据指定的偏移量，读取一个有符号 8 位整数。 若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 offset 可能会超出 buffer 的末尾。默认是 false。                                                                                                                                                                                                                                     |
| 27   | **buf.readInt16LE(offset[, noAssert])** 根据指定的偏移量，使用特殊的 endian 格式读取一个 有符号 16 位整数，小端对齐。 若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 offset 可能会超出 buffer 的末尾。默认是 false。                                                                                                                                                                                                |
| 28   | **buf.readInt16BE(offset[, noAssert])** 根据指定的偏移量，使用特殊的 endian 格式读取一个 有符号 16 位整数，大端对齐。 若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 offset 可能会超出 buffer 的末尾。默认是 false。                                                                                                                                                                                                |
| 29   | **buf.readInt32LE(offset[, noAssert])** 根据指定的偏移量，使用指定的 endian 字节序格式读取一个有符号 32 位整数，小端对齐。 若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 offset 可能会超出buffer 的末尾。默认是 false。                                                                                                                                                                                            |
| 30   | **buf.readInt32BE(offset[, noAssert])** 根据指定的偏移量，使用指定的 endian 字节序格式读取一个有符号 32 位整数，大端对齐。 若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 offset 可能会超出buffer 的末尾。默认是 false。                                                                                                                                                                                            |
| 31   | **buf.readFloatLE(offset[, noAssert])** 根据指定的偏移量，使用指定的 endian 字节序格式读取一个 32 位双浮点数，小端对齐。 若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 offset 可能会超出buffer的末尾。默认是 false。                                                                                                                                                                                               |
| 32   | **buf.readFloatBE(offset[, noAssert])** 根据指定的偏移量，使用指定的 endian 字节序格式读取一个 32 位双浮点数，大端对齐。 若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 offset 可能会超出buffer的末尾。默认是 false。                                                                                                                                                                                               |
| 33   | **buf.readDoubleLE(offset[, noAssert])** 根据指定的偏移量，使用指定的 endian字节序格式读取一个 64 位双精度数，小端对齐。 若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 offset 可能会超出buffer 的末尾。默认是 false。                                                                                                                                                                                              |
| 34   | **buf.readDoubleBE(offset[, noAssert])** 根据指定的偏移量，使用指定的 endian字节序格式读取一个 64 位双精度数，大端对齐。 若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 offset 可能会超出buffer 的末尾。默认是 false。                                                                                                                                                                                              |
| 35   | **buf.writeUInt8(value, offset[, noAssert])** 根据传入的 offset 偏移量将 value 写入 buffer。注意：value 必须是一个合法的无符号 8 位整数。 若参数 noAssert 为 true 将不会验证 offset 偏移量参数。 这意味着 value 可能过大，或者 offset 可能会超出 buffer 的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则不要使用。默认是 false。                                                                                       |
| 36   | **buf.writeUInt16LE(value, offset[, noAssert])** 根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个合法的无符号 16 位整数，小端对齐。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着 value 可能过大，或者 offset 可能会超出buffer的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false。                                          |
| 37   | **buf.writeUInt16BE(value, offset[, noAssert])** 根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个合法的无符号 16 位整数，大端对齐。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着 value 可能过大，或者 offset 可能会超出buffer的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false。                                          |
| 38   | **buf.writeUInt32LE(value, offset[, noAssert])** 根据传入的 offset 偏移量和指定的 endian 格式(LITTLE-ENDIAN:小字节序)将 value 写入buffer。注意：value 必须是一个合法的无符号 32 位整数，小端对齐。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着value 可能过大，或者offset可能会超出buffer的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false。                      |
| 39   | **buf.writeUInt32BE(value, offset[, noAssert])** 根据传入的 offset 偏移量和指定的 endian 格式(Big-Endian:大字节序)将 value 写入buffer。注意：value 必须是一个合法的有符号 32 位整数。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着 value 可能过大，或者offset可能会超出buffer的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false。                                  |
| 40   | **buf.writeInt8(value, offset[, noAssert])**                                                                                                                                                                                                                                                                                                                                                                                         |
| 41   | **buf.writeInt16LE(value, offset[, noAssert])** 根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个合法的 signed 16 位整数。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着 value 可能过大，或者 offset 可能会超出 buffer 的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false 。                                                 |
| 42   | **buf.writeInt16BE(value, offset[, noAssert])** 根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个合法的 signed 16 位整数。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着 value 可能过大，或者 offset 可能会超出 buffer 的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false 。                                                 |
| 43   | **buf.writeInt32LE(value, offset[, noAssert])** 根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个合法的 signed 32 位整数。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着 value 可能过大，或者 offset 可能会超出 buffer 的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false。                                                  |
| 44   | **buf.writeInt32BE(value, offset[, noAssert])** 根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个合法的 signed 32 位整数。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着 value 可能过大，或者 offset 可能会超出 buffer 的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false。                                                  |
| 45   | **buf.writeFloatLE(value, offset[, noAssert])** 根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer 。注意：当 value 不是一个 32 位浮点数类型的值时，结果将是不确定的。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着 value可能过大，或者 offset 可能会超出 buffer 的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false。                                |
| 46   | **buf.writeFloatBE(value, offset[, noAssert])** 根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer 。注意：当 value 不是一个 32 位浮点数类型的值时，结果将是不确定的。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着 value可能过大，或者 offset 可能会超出 buffer 的末尾从而造成 value 被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false。                                |
| 47   | **buf.writeDoubleLE(value, offset[, noAssert])** 根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个有效的 64 位double 类型的值。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着 value 可能过大，或者 offset 可能会超出 buffer 的末尾从而造成value被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false。                                               |
| 48   | **buf.writeDoubleBE(value, offset[, noAssert])** 根据传入的 offset 偏移量和指定的 endian 格式将 value 写入 buffer。注意：value 必须是一个有效的 64 位double 类型的值。 若参数 noAssert 为 true 将不会验证 value 和 offset 偏移量参数。 这意味着 value 可能过大，或者 offset 可能会超出 buffer 的末尾从而造成value被丢弃。 除非你对这个参数非常有把握，否则尽量不要使用。默认是 false。                                               |
| 49   | **buf.fill(value[, offset][, end])** 使用指定的 value 来填充这个 buffer。如果没有指定 offset (默认是 0) 并且 end (默认是 buffer.length) ，将会填充整个buffer。                                                                                                                                                                                                                                                                       |

## Stream(流)

> **Stream四种类型**
>
> - Readable** - 可读操作。
> - **Writable** - 可写操作。
> - **Duplex** - 可读可写操作.
> - **Transform** - 操作被写入数据，然后读出结果。
>
>
>
> 所有的 Stream 对象都是 EventEmitter 的实例。
>
> 常用的事件有：
>
> - **data** - 当有数据可读时触发。
> - **end** - 没有更多的数据可读时触发。
> - **error** - 在接收和写入过程中发生错误时触发。
> - **finish** - 所有数据已被写入到底层系统时触发。

### 从流中读取数据

input.js

```txt
grh
```

main.js

```js
var fs = require("fs");
var data = '';

// 创建可读流
var readerStream = fs.createReadStream('input.txt');

// 设置编码为 utf8。
readerStream.setEncoding('UTF8');

// 处理流事件 --> data, end, and error
readerStream.on('data', function(chunk) {
   data += chunk;
});

readerStream.on('end',function(){
   console.log(data);
});

readerStream.on('error', function(err){
   console.log(err.stack);
});

console.log("程序执行完毕");

结果:

程序执行完毕
grh官网地址：www.runoob.com

```

### 写入流

main.js

```js
var fs = require("fs");
var data = 'grh官网地址：www.runoob.com';

// 创建一个可以写入的流，写入到文件 output.txt 中
var writerStream = fs.createWriteStream('output.txt');

// 使用 utf8 编码写入数据
writerStream.write(data,'UTF8');

// 标记文件末尾
writerStream.end();

// 处理流事件 --> data, end, and error
writerStream.on('finish', function() {
    console.log("写入完成。");
});

writerStream.on('error', function(err){
   console.log(err.stack);
});

console.log("程序执行完毕");

结果

程序执行完毕
写入完成

```

---

### 管道流

> - 一个输出流流到流入的机制
> - 我们用于从一个流中获取数据并将数据传递到另一个流中
>   - 可以慢慢的实现大文件的复制过程

```js
var fs = require("fs");

// 创建一个可读流
var readerStream = fs.createReadStream('input.txt');

// 创建一个可写流
var writerStream = fs.createWriteStream('output.txt');

// 管道读写操作
// 读取 input.txt 文件内容，并将内容写入到 output.txt 文件中
readerStream.pipe(writerStream);

console.log("程序执行完毕");
```

---

### 链式流

> - 一般用于管道操作
> - 通过连接输出流到另外一个流并创建多个流操作链的机制

```js
//compress.js 压缩文件

var fs = require("fs");
var zlib = require('zlib');

// 压缩 input.txt 文件为 input.txt.gz
fs.createReadStream('input.txt')
  .pipe(zlib.createGzip())
  .pipe(fs.createWriteStream('input.txt.gz'));
  
console.log("文件压缩完成。");
```

```js
//decompress.js 解压文件

var fs = require("fs");
var zlib = require('zlib');

// 解压 input.txt.gz 文件为 input.txt
fs.createReadStream('input.txt.gz')
  .pipe(zlib.createGunzip())
  .pipe(fs.createWriteStream('input.txt'));
  
console.log("文件解压完成。");
```

----

## 模块系统

> **exports 和 module.exports 的使用**
>
> - 如果要对外暴露属性或方法，就用 **exports** 就行，要暴露对象(类似class，包含了很多属性和方法)，就用 **module.exports**。
> - 不用同时使用两个

### 创建模块

自封装一个对象

```js
module.exports = function() {
  // ...
}

//hello.js 
function Hello() { 
    var name; 
    this.setName = function(thyName) { 
        name = thyName; 
    }; 
    this.sayHello = function() { 
        console.log('Hello ' + name); 
    }; 
}; 
module.exports = Hello;

//main.js 
var Hello = require('./hello'); 
hello = new Hello(); 
hello.setName('BYVoid'); 
hello.sayHello(); 
```

hello.js

```js
exports.world = function() {
  console.log('Hello World');
}
```

main.js

```js
var hello = require('./hello');
hello.world();
```

### 服务器的模块

```js
var http = require("http");

...

http.createServer(...);
```

#### require方法执行过程

<img src="https://images.gitee.com/uploads/images/2020/0601/215608_19475bd4_6545143.png" style="zoom:50%;" />

---

## 函数

> 函数中传输参数的地方可以直接定义函数
>
> ```js
> function say(word) {
> console.log(word);
> }
> 
> function execute(someFunction, value) {
> someFunction(value);
> }
> 
> execute(say, "Hello");
> ```
>
>

### 匿名函数

> 我们可以把一个函数作为变量传递。
>
> 但是我们不一定要绕这个"==先定义，再传递=="的圈子，
>
> 我们可以直接在另一个函数的括号中定义和传递这个函数：

```js
function execute(someFunction, value) {
  someFunction(value);
}

execute(function(word){ console.log(word) }, "Hello");
```

### 函数传递是如何让HTTP服务器工作的

```js
var http = require("http");

http.createServer(function(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}).listen(8888);
```

```js
var http = require("http");

function onRequest(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}

http.createServer(onRequest).listen(8888);
```

----

### 路由

<img src="https://images.gitee.com/uploads/images/2020/0601/223037_4fbde394_6545143.png" style="zoom:50%;" />

server.js

```js
var http = require("http");
var url = require("url");
 
function start() {
  function onRequest(request, response) {
    var pathname = url.parse(request.url).pathname;
    console.log("Request for " + pathname + " received.");
    response.writeHead(200, {"Content-Type": "text/plain"});
    response.write("Hello World");
    response.end();
  }
 
  http.createServer(onRequest).listen(8888);
  console.log("Server has started.");
}
 
exports.start = start;
```

router.js

```js
function route(pathname) {
  console.log("About to route a request for " + pathname);
}
 
exports.route = route;
```

index.js

```js
var server = require("./server");
var router = require("./router");
 
server.start(router.route);
```

启动

```shell
$ node index.js
Server has started.
```

浏览器访问 **<http://127.0.0.1:8888/>**

----

## 全局对象

> - 在程序的任何地方都可以访问,即全局变量
>
> - 通常window是全局对象,node中的全局对象是global
> - 按照ECMAScript定义,全局变量的条件:
>   - 在最外层定义的变量
>   - 全局对象的属性
>   - 隐式定义的变量(未定义直接赋值的变量)
>   - 不要使用var定义变量,全局变量会污染命名空间

### __filename

> - 表示当前正在执行的脚本的文件名
> - 输出文件说在位置的绝对路径
> - 如果是在模块中,返回的值是模块文件的路径

main.js

```js
// 输出全局变量 __filename 的值
console.log( __filename );

执行:
$ node main.js
/web/com/runoob/nodejs/main.js
```

----

### __dirname

> 表示当前执行脚本说在的目录

main.js

```js
// 输出全局变量 __dirname 的值
console.log( __dirname );
执行 main.js 文件，代码如下所示:


输出:
$ node main.js
/web/com/runoob/nodejs
```

----

### clearTimeout(t)

> - 全局函数用于停止一个之前通过 setTimeout() 创建的定时器。
>
> - 参数 **t** 是通过 setTimeout() 函数创建的定时器。

```js
function printHello(){
   console.log( "Hello, World!");
}
// 两秒后执行以上函数
var t = setTimeout(printHello, 2000);

// 清除定时器
clearTimeout(t);
```

### setInterval(cb, ms)

> - 指定在每ms数执行
> - 直到clearInterval()被调用或窗口关闭

```js
function printHello(){
   console.log( "Hello, World!");
}
// 两秒后执行以上函数
setInterval(printHello, 2000);
```

### console

> console 用于提供控制台标准输出，它是由 Internet Explorer 的 JScript 引擎提供的调试工具，后来逐渐成为浏览器的实施标准。
>
> Node.js 沿用了这个标准，提供与习惯行为一致的 console 对象，用于向标准输出流（stdout）或标准错误流（stderr）输出字符。

| 序号 | 方法 & 描述                                                                                                                                                                                           |
| :--- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | **console.log([data][, ...])** 向标准输出流打印字符并以换行符结束。该方法接收若干 个参数，如果只有一个参数，则输出这个参数的字符串形式。如果有多个参数，则 以类似于C 语言 printf() 命令的格式输出。   |
| 2    | **console.info([data][, ...])** 该命令的作用是返回信息性消息，这个命令与console.log差别并不大，除了在chrome中只会输出文字外，其余的会显示一个蓝色的惊叹号。                                           |
| 3    | **console.error([data][, ...])** 输出错误消息的。控制台在出现错误时会显示是红色的叉子。                                                                                                               |
| 4    | **console.warn([data][, ...])** 输出警告消息。控制台出现有黄色的惊叹号。                                                                                                                              |
| 5    | **console.dir(obj[, options])** 用来对一个对象进行检查（inspect），并以易于阅读和打印的格式显示。                                                                                                     |
| 6    | **console.time(label)** 输出时间，表示计时开始。                                                                                                                                                      |
| 7    | **console.timeEnd(label)** 结束时间，表示计时结束。                                                                                                                                                   |
| 8    | **console.trace(message[, ...])** 当前执行的代码在堆栈中的调用路径，这个测试函数运行很有帮助，只要给想测试的函数里面加入 console.trace 就行了。                                                       |
| 9    | **console.assert(value[, message][, ...])** 用于判断某个表达式或变量是否为真，接收两个参数，第一个参数是表达式，第二个参数是字符串。只有当第一个参数为false，才会输出第二个参数，否则不会有任何结果。 |

#### console.log()

```js
console.log('Hello world'); 
console.log('byvoid%diovyb'); 
console.log('byvoid%diovyb', 1991); 
运行结果:
Hello world 
byvoid%diovyb 
byvoid1991iovyb
```

#### console.error()

> 与console.log() 用法相同，只是向标准错误流输出。

#### console.trace()

> 向标准流输出当前的调用栈

```js
console.trace()

Trace:
at Object.<anonymous> (/home/byvoid/consoletrace.js:1:71) 
at Module._compile (module.js:441:26) 
at Object..js (module.js:459:10) 
at Module.load (module.js:348:31) 
at Function._load (module.js:308:12) 
at Array.0 (module.js:479:10) 
at EventEmitter._tickCallback (node.js:192:40)
```

eg:

```js
console.info("程序开始执行：");
var counter = 10;
console.log("计数: %d", counter);
console.time("获取数据");
//
// 执行一些代码
// 
console.timeEnd('获取数据');
console.info("程序执行完毕。")


$ node main.js
程序开始执行：
计数: 10
获取数据: 0ms
程序执行完毕
```

### process

> - 全局变量,即global对象属性

| 序号 | 事件 & 描述                                                                                                                                                                 |
| :--- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | **exit** 当进程准备退出时触发。                                                                                                                                             |
| 2    | **beforeExit** 当 node 清空事件循环，并且没有其他安排时触发这个事件。通常来说，当没有进程安排时 node 退出，但是 'beforeExit' 的监听器可以异步调用，这样 node 就会继续执行。 |
| 3    | **uncaughtException** 当一个异常冒泡回到事件循环，触发这个事件。如果给异常添加了监视器，默认的操作（打印堆栈跟踪信息并退出）就不会发生。                                    |
| 4    | **Signal 事件** 当进程接收到信号时就触发。信号列表详见标准的 POSIX 信号名，如 SIGINT、SIGUSR1 等。                                                                          |

```js
process.on('exit', function(code) {

  // 以下代码永远不会执行
  setTimeout(function() {
    console.log("该代码不会执行");
  }, 0);
  
  console.log('退出码为:', code);
});
console.log("程序执行结束")

$ node main.js
程序执行结束
退出码为: 0

```

#### 退出状态码

| 状态码 | 名称 & 描述                                                                                                                                                                        |
| :----- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | **Uncaught Fatal Exception** 有未捕获异常，并且没有被域或 uncaughtException 处理函数处理。                                                                                         |
| 2      | **Unused** 保留                                                                                                                                                                    |
| 3      | **Internal JavaScript Parse Error** JavaScript的源码启动 Node 进程时引起解析错误。非常罕见，仅会在开发 Node 时才会有。                                                             |
| 4      | **Internal JavaScript Evaluation Failure** JavaScript 的源码启动 Node 进程，评估时返回函数失败。非常罕见，仅会在开发 Node 时才会有。                                               |
| 5      | **Fatal Error** V8 里致命的不可恢复的错误。通常会打印到 stderr ，内容为： FATAL ERROR                                                                                              |
| 6      | **Non-function Internal Exception Handler** 未捕获异常，内部异常处理函数不知为何设置为on-function，并且不能被调用。                                                                |
| 7      | **Internal Exception Handler Run-Time Failure** 未捕获的异常， 并且异常处理函数处理时自己抛出了异常。例如，如果 process.on('uncaughtException') 或 domain.on('error') 抛出了异常。 |
| 8      | **Unused** 保留                                                                                                                                                                    |
| 9      | **Invalid Argument** 可能是给了未知的参数，或者给的参数没有值。                                                                                                                    |
| 10     | **Internal JavaScript Run-Time Failure** JavaScript的源码启动 Node 进程时抛出错误，非常罕见，仅会在开发 Node 时才会有。                                                            |
| 12     | **Invalid Debug Argument** 设置了参数--debug 和/或 --debug-brk，但是选择了错误端口。                                                                                               |
| 128    | **Signal Exits** 如果 Node 接收到致命信号，比如SIGKILL 或 SIGHUP，那么退出代码就是128 加信号代码。这是标准的 Unix 做法，退出信号代码放在高位。                                     |

#### Process属性

| 序号. | 属性 & 描述                                                                                                                                    |
| :---- | :--------------------------------------------------------------------------------------------------------------------------------------------- |
| 1     | **stdout** 标准输出流。                                                                                                                        |
| 2     | **stderr** 标准错误流。                                                                                                                        |
| 3     | **stdin** 标准输入流。                                                                                                                         |
| 4     | **argv** argv 属性返回一个数组，由命令行执行脚本时的各个参数组成。它的第一个成员总是node，第二个成员是脚本文件名，其余成员是脚本文件的参数。   |
| 5     | **execPath** 返回执行当前脚本的 Node 二进制文件的绝对路径。                                                                                    |
| 6     | **execArgv** 返回一个数组，成员是命令行下执行脚本时，在Node可执行文件与脚本文件之间的命令行参数。                                              |
| 7     | **env** 返回一个对象，成员为当前 shell 的环境变量                                                                                              |
| 8     | **exitCode** 进程退出时的代码，如果进程优通过 process.exit() 退出，不需要指定退出码。                                                          |
| 9     | **version** Node 的版本，比如v0.10.18。                                                                                                        |
| 10    | **versions** 一个属性，包含了 node 的版本和依赖.                                                                                               |
| 11    | **config** 一个包含用来编译当前 node 执行文件的 javascript 配置选项的对象。它与运行 ./configure 脚本生成的 "config.gypi" 文件相同。            |
| 12    | **pid** 当前进程的进程号。                                                                                                                     |
| 13    | **title** 进程名，默认值为"node"，可以自定义该值。                                                                                             |
| 14    | **arch** 当前 CPU 的架构：'arm'、'ia32' 或者 'x64'。                                                                                           |
| 15    | **platform** 运行程序所在的平台系统 'darwin', 'freebsd', 'linux', 'sunos' 或 'win32'                                                           |
| 16    | **mainModule** require.main 的备选方法。不同点，如果主模块在运行时改变，require.main可能会继续返回老的模块。可以认为，这两者引用了同一个模块。 |

```js
// 输出到终端
process.stdout.write("Hello World!" + "\n");
// 通过参数读取
process.argv.forEach(function(val, index, array) {
   console.log(index + ': ' + val);
});
// 获取执行路径
console.log(process.execPath);
// 平台信息
console.log(process.platform);


$ node main.js
Hello World!
0: node
1: /web/www/node/main.js
/usr/local/node/0.10.36/bin/node
darwin
```

#### 方法参考手册

| 序号 | 方法 & 描述                                                                                                                                                                                                                                                                                       |
| :--- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1    | **abort()** 这将导致 node 触发 abort 事件。会让 node 退出并生成一个核心文件。                                                                                                                                                                                                                     |
| 2    | **chdir(directory)** 改变当前工作进程的目录，如果操作失败抛出异常。                                                                                                                                                                                                                               |
| 3    | **cwd()** 返回当前进程的工作目录                                                                                                                                                                                                                                                                  |
| 4    | **exit([code])** 使用指定的 code 结束进程。如果忽略，将会使用 code 0。                                                                                                                                                                                                                            |
| 5    | **getgid()** 获取进程的群组标识（参见 getgid(2)）。获取到得时群组的数字 id，而不是名字。 注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。                                                                                                                                        |
| 6    | **setgid(id)** 设置进程的群组标识（参见 setgid(2)）。可以接收数字 ID 或者群组名。如果指定了群组名，会阻塞等待解析为数字 ID 。 注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。                                                                                                   |
| 7    | **getuid()** 获取进程的用户标识(参见 getuid(2))。这是数字的用户 id，不是用户名。 注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。                                                                                                                                                |
| 8    | **setuid(id)** 设置进程的用户标识（参见setuid(2)）。接收数字 ID或字符串名字。果指定了群组名，会阻塞等待解析为数字 ID 。 注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。                                                                                                         |
| 9    | **getgroups()** 返回进程的群组 iD 数组。POSIX 系统没有保证一定有，但是 node.js 保证有。 注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。                                                                                                                                         |
| 10   | **setgroups(groups)** 设置进程的群组 ID。这是授权操作，所以你需要有 root 权限，或者有 CAP_SETGID 能力。 注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。                                                                                                                         |
| 11   | **initgroups(user, extra_group)** 读取 /etc/group ，并初始化群组访问列表，使用成员所在的所有群组。这是授权操作，所以你需要有 root 权限，或者有 CAP_SETGID 能力。 注意：这个函数仅在 POSIX 平台上可用(例如，非Windows 和 Android)。                                                                |
| 12   | **kill(pid[, signal])** 发送信号给进程. pid 是进程id，并且 signal 是发送的信号的字符串描述。信号名是字符串，比如 'SIGINT' 或 'SIGHUP'。如果忽略，信号会是 'SIGTERM'。                                                                                                                             |
| 13   | **memoryUsage()** 返回一个对象，描述了 Node 进程所用的内存状况，单位为字节。                                                                                                                                                                                                                      |
| 14   | **nextTick(callback)** 一旦当前事件循环结束，调用回调函数。                                                                                                                                                                                                                                       |
| 15   | **umask([mask])** 设置或读取进程文件的掩码。子进程从父进程继承掩码。如果mask 参数有效，返回旧的掩码。否则，返回当前掩码。                                                                                                                                                                         |
| 16   | **uptime()** 返回 Node 已经运行的秒数。                                                                                                                                                                                                                                                           |
| 17   | **hrtime()** 返回当前进程的高分辨时间，形式为 [seconds, nanoseconds]数组。它是相对于过去的任意事件。该值与日期无关，因此不受时钟漂移的影响。主要用途是可以通过精确的时间间隔，来衡量程序的性能。 你可以将之前的结果传递给当前的 process.hrtime() ，会返回两者间的时间差，用来基准和测量时间间隔。 |

```js
// 输出当前目录
console.log('当前目录: ' + process.cwd());

// 输出当前版本
console.log('当前版本: ' + process.version);

// 输出内存使用情况
console.log(process.memoryUsage());

$ node main.js
当前目录: /web/com/runoob/nodejs
当前版本: v0.10.36
{ rss: 12541952, heapTotal: 4083456, heapUsed: 2157056 }
```

## 常用工具

### util

`const uril = require('util')`

#### util.callbackify

> util.callbackify(orginal)将async异步函数(或者一个返回值为Promise的函数)转换为遵循`异常优先回调风格`的函数,例如将(err,value)=>...回调作为最后一个参数. 在回答函数中,第一个参数为拒绝的原因(如果Promise解决,则为null), 第二个参数则是解决的值.

```js
const util =require('util');
 async function fn(){
   return 'hello world';
 }

const callbackFunction = util.callbackify(fn);

callbackFunction((err,ret)=>{
  if(err) throw err;
  console.log(ret);
})

执行结果:
hello world
```

> - 回调函数是异步执行的，并且有异常堆栈错误追踪。 如果回调函数抛出一个异常，进程会触发一个 'uncaughtException' 异常，如果没有被捕获，进程将会退出。
>
> - null 在回调函数中作为一个参数有其特殊的意义，如果回调函数的首个参数为 Promise 拒绝的原因且带有返回值，且值可以转换成布尔值 false，这个值会被封装在 Error 对象里，可以通过属性 reason 获取

```js
function fn() {
  return Promise.reject(null);
}
const callbackFunction = util.callbackify(fn);

callbackFunction((err, ret) => {
  // 当 Promise 被以 `null` 拒绝时，它被包装为 Error 并且原始值存储在 `reason` 中。
  err && err.hasOwnProperty('reason') && err.reason === null;  // true
});
```

> original 为async异步函数,改函数返回传统回调函数

#### util.inherits

> - util.inherits(constructor, superConstructor) 是一个实现对象间原型继承的函数
>
> - javaScript的面向对象特性是基于原型的,与常见的基于类的不同.
> - JavaScript没有提供对象继承的语言级别特性,而是通过原型复制来实现的.

```js
var util = require('util');
funciton Base(){
 this.name = 'base';
  this.base = '1999';
  this.sayHello = function(){
    console.log('Hello'+ this.name);
  };
}

Base.prototype.showName = function(){
  console.log(this.name);
}

function Sub(){
  this.name = 'sub';
}

util.inherits(Sub, Base);//这里实现了sub继承了Base

var objBase = new Base();
objBase.showname();
objBase.sayHello();
console.log(objBase);

var objSub = new Sub();
objSub.showName();
//objSub.sayHello();
console.log(objSub);
```

> - 定义一个基础对象Base和一个继承自Base的Sub,Base有三个构造函数类定义的属性和一个原型中定义的函数,
> - 通过util.inherits实现继承

```js
执行结果
base
Hello base
{ name: 'base', base :1999, sayHello:[Funciton] }
sub
{ name: 'sub' }
```

> - sub仅仅继承了Base在原型中定义的函数
> - 而构造函数内部创造函数内部创造的base属性和sayHello函数都没有被Sub继承
> - ==在原型定义的属性不会被console.log作为对象的属性输出==
> - 可以继承原型方法

----

#### util.inspect

> - `util.inspect(object, [showHidden], [depth], [colors])`
> - 将任意对象转换为字符串的方法, 通常用于挑食和错误输出
> - 至少接受一个参数object,即要转换的对象
> - showHidden:可选,如果为true,会输出更多隐藏信息
> - depth表示最大的层次,
>   - 如果对象很复杂,你可以指定成熟以控制输出信息的多少.
>   - 如果不指定depth,默认会递归2层,
>   - 指定为null表示不递归成熟完成遍历对象
> - colors值true,输出格式将会ANSI编码,通常用于在终端显示更加漂亮的效果

```js
var util = require('util'); 
function Person() { 
    this.name = 'byvoid'; 
    this.toString = function() { 
    return this.name; 
    }; 
} 
var obj = new Person(); 
console.log(util.inspect(obj)); 
console.log(util.inspect(obj, true)); 

运行结果:
Person { name: 'byvoid', toString: [Function] }
Person {
  name: 'byvoid',
  toString: 
   { [Function]
     [length]: 0,
     [name]: '',
     [arguments]: null,
     [caller]: null,
     [prototype]: { [constructor]: [Circular] } } }
```

| 方法                  | 描述           |
| --------------------- | -------------- |
| util.isArray(object)  | 是数组返回true |
| util.isRegExp(object) | 是正则返回true |
| util.isDate(object)   | 是日期返回true |

## 文件系统

> - `var fs  = require("fs")`
> - node有异步[fs.readFile()]和同步[fs.readFileSync()]两个版本
> - 异步方法函数最后一个参数为回调函数,会带函数的第一个参数包含错误信息(error)
> - 建议使用异步

file.js

```js
var fs = require("fs")
//异步读取
fs.readFIle('input.txt', function(err, data){
  if(err){
    return console.error(err);
  }
  console.log("异步读取:"+ data.toString());
});

//同步读取
var data = fs.readFileSync('input.txt');
console.log("同步读取:" + data.toString());
console.log("程序执行完毕");
```

### 打开文件

> 异步模式打开文件的语法:`fs.open(path, flags[, model], callback)`
>
> - flags:文件的打开行为
> - mode: 设置文件模式(权限), 文件创建默认权限为0666(可读可写)
> - callback: 回调函数, 带有两个参数:callback(err,fd)

flags参数

| Flag | 描述                                                 |
| :--- | :--------------------------------------------------- |
| r    | 以读取模式打开文件。如果文件不存在抛出异常。         |
| r+   | 以读写模式打开文件。如果文件不存在抛出异常。         |
| rs   | 以同步的方式读取文件。                               |
| rs+  | 以同步的方式读取和写入文件。                         |
| w    | 以写入模式打开文件，如果文件不存在则创建。           |
| wx   | 类似 'w'，但是如果文件路径存在，则文件写入失败。     |
| w+   | 以读写模式打开文件，如果文件不存在则创建。           |
| wx+  | 类似 'w+'， 但是如果文件路径存在，则文件读写失败。   |
| a    | 以追加模式打开文件，如果文件不存在则创建。           |
| ax   | 类似 'a'， 但是如果文件路径存在，则文件追加失败。    |
| a+   | 以读取追加模式打开文件，如果文件不存在则创建。       |
| ax+  | 类似 'a+'， 但是如果文件路径存在，则文件读取追加失败 |

```js
var fs = require("fs");

// 异步打开文件
console.log("准备打开文件！");
fs.open('input.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
  console.log("文件打开成功！");     
});
```

### 获取文件信息

> - `fs.stat(path,callback)`
> - callback: 回调函数,带有两个参数(err,stats), stats是fs.Stats对象

```js
//fs.stat(path)执行后，会将stats类的实例返回给其回调函数。可以通过stats类中的提供方法判断文件的相关属性。例如判断是否为文件：
var fs = require('fs');

fs.stat('/Users/liuht/code/itbilu/demo/fs.js', function (err, stats) {
    console.log(stats.isFile());         //true
})
```

stats类中的方法

| 方法                      | 描述                                                                         |
| :------------------------ | :--------------------------------------------------------------------------- |
| stats.isFile()            | 如果是文件返回 true，否则返回 false。                                        |
| stats.isDirectory()       | 如果是目录返回 true，否则返回 false。                                        |
| stats.isBlockDevice()     | 如果是块设备返回 true，否则返回 false。                                      |
| stats.isCharacterDevice() | 如果是字符设备返回 true，否则返回 false。                                    |
| stats.isSymbolicLink()    | 如果是软链接返回 true，否则返回 false。                                      |
| stats.isFIFO()            | 如果是FIFO，返回true，否则返回 false。FIFO是UNIX中的一种特殊类型的命令管道。 |
| stats.isSocket()          | 如果是 Socket 返回 true，否则返回 false。                                    |

```js
var fs = require("fs");

console.log("准备打开文件！");
fs.stat('input.txt', function (err, stats) {
   if (err) {
       return console.error(err);
   }
   console.log(stats);
   console.log("读取文件信息成功！");
   
   // 检测文件类型
   console.log("是否为文件(isFile) ? " + stats.isFile());
   console.log("是否为目录(isDirectory) ? " + stats.isDirectory());    
});

运行结果:

$ node file.js 
准备打开文件！
{ dev: 16777220,
  mode: 33188,
  nlink: 1,
  uid: 501,
  gid: 20,
  rdev: 0,
  blksize: 4096,
  ino: 40333161,
  size: 61,
  blocks: 8,
  atime: Mon Sep 07 2015 17:43:55 GMT+0800 (CST),
  mtime: Mon Sep 07 2015 17:22:35 GMT+0800 (CST),
  ctime: Mon Sep 07 2015 17:22:35 GMT+0800 (CST) }
读取文件信息成功！
是否为文件(isFile) ? true
是否为目录(isDirectory) ? false
```

### 写入文件

> - `fs.writeFile(file, data[, options], callback)`
> - 打开方式默认w模式(文件存在写入内容会覆盖)
> - **file** - 文件名或文件描述符。
> - **data** - 要写入文件的数据，可以是 String(字符串) 或 Buffer(缓冲) 对象。
> - **options** - 该参数是一个对象，包含 {encoding, mode, flag}。默认编码为 utf8, 模式为 0666 ， flag 为 'w'
> - **callback** - 回调函数，回调函数只包含错误信息参数(err)，在写入失败时返回。

```js
var fs = require("fs");

console.log("准备写入文件");
fs.writeFile('input.txt', '我是通 过fs.writeFile 写入文件的内容',  function(err) {
   if (err) {
       return console.error(err);
   }
   console.log("数据写入成功！");
   console.log("--------我是分割线-------------")
   console.log("读取写入的数据！");
   fs.readFile('input.txt', function (err, data) {
      if (err) {
         return console.error(err);
      }
      console.log("异步读取文件数据: " + data.toString());
   });
});

执行结果如下：

$ node file.js 
准备写入文件
数据写入成功！
--------我是分割线-------------
读取写入的数据！
异步读取文件数据: 我是通 过fs.writeFile 写入文件的内容
```

### 读取文件

> - ```js
>   fs.read(fd, buffer, offset, length, position, callback)
>   ```
>
> - **fd** - 通过 fs.open() 方法返回的文件描述符。
>
> - **buffer** - 数据写入的缓冲区。
>
> - **offset** - 缓冲区写入的写入偏移量。
>
> - **length** - 要从文件中读取的字节数。
>
> - **position** - 文件读取的起始位置，如果 position 的值为 null，则会从当前文件指针的位置读取。
>
> - **callback** - 回调函数，有三个参数err, bytesRead, buffer，err 为错误信息， bytesRead 表示读取的字节数，buffer 为缓冲区对象。

```js
var fs = require("fs");
var buf = new Buffer.alloc(1024);

console.log("准备打开已存在的文件！");
fs.open('input.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("准备读取文件：");
   fs.read(fd, buf, 0, buf.length, 0, function(err, bytes){
      if (err){
         console.log(err);
      }
      console.log(bytes + "  字节被读取");
      
      // 仅输出读取的字节
      if(bytes > 0){
         console.log(buf.slice(0, bytes).toString());
      }
   });
});

以上代码执行结果如下：

$ node file.js 
准备打开已存在的文件！
文件打开成功！
准备读取文件：
42  字节被读取
grh官网地址：www.runoob.com
```

### 关闭文件

> ```
> fs.close(fd, callback)
> ```
>
> - **fd** - 通过 fs.open() 方法返回的文件描述符。
> - **callback** - 回调函数，没有参数。

```js
var fs = require("fs");
var buf = new Buffer.alloc(1024);

console.log("准备打开文件！");
fs.open('input.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("准备读取文件！");
   fs.read(fd, buf, 0, buf.length, 0, function(err, bytes){
      if (err){
         console.log(err);
      }

      // 仅输出读取的字节
      if(bytes > 0){
         console.log(buf.slice(0, bytes).toString());
      }

      // 关闭文件
      fs.close(fd, function(err){
         if (err){
            console.log(err);
         } 
         console.log("文件关闭成功");
      });
   });
});

以上代码执行结果如下：

$ node file.js 
准备打开文件！
文件打开成功！
准备读取文件！
grh官网地址：www.runoob.com
文件关闭成功
```

### 截取文件

```
fs.ftruncate(fd, len, callback)
```

参数使用说明如下：

- **fd** - 通过 fs.open() 方法返回的文件描述符。
- **len** - 文件内容截取的长度。
- **callback** - 回调函数，没有参数。

input.txt 文件内容为：

```
site:www.runoob.com
```

接下来我们创建 file.js 文件，代码如下所示：

```js
var fs = require("fs");
var buf = new Buffer.alloc(1024);

console.log("准备打开文件！");
fs.open('input.txt', 'r+', function(err, fd) {
   if (err) {
       return console.error(err);
   }
   console.log("文件打开成功！");
   console.log("截取10字节内的文件内容，超出部分将被去除。");
   
   // 截取文件
   fs.ftruncate(fd, 10, function(err){
      if (err){
         console.log(err);
      } 
      console.log("文件截取成功。");
      console.log("读取相同的文件"); 
      fs.read(fd, buf, 0, buf.length, 0, function(err, bytes){
         if (err){
            console.log(err);
         }

         // 仅输出读取的字节
         if(bytes > 0){
            console.log(buf.slice(0, bytes).toString());
         }

         // 关闭文件
         fs.close(fd, function(err){
            if (err){
               console.log(err);
            } 
            console.log("文件关闭成功！");
         });
      });
   });
});
```

以上代码执行结果如下：

```js
$ node file.js 
准备打开文件！
文件打开成功！
截取10字节内的文件内容，超出部分将被去除。
文件截取成功。
读取相同的文件
site:www.r
文件关闭成功
```

### 删除文件

```
fs.unlink(path, callback)
```

参数使用说明如下：

- **path** - 文件路径。
- **callback** - 回调函数，没有参数。

input.txt 文件内容为：

```
site:www.runoob.com
```

接下来我们创建 file.js 文件，代码如下所示：

```js
var fs = require("fs");

console.log("准备删除文件！");
fs.unlink('input.txt', function(err) {
   if (err) {
       return console.error(err);
   }
   console.log("文件删除成功！");
});
```

以上代码执行结果如下：

```
$ node file.js 
准备删除文件！
文件删除成功！
```

### 创建目录

```
fs.mkdir(path[, options], callback)
```

参数使用说明如下：

- **path** - 文件路径。
- options 参数可以是：
  - **recursive** - 是否以递归的方式创建目录，默认为 false。
  - **mode** - 设置目录权限，默认为 0777。
- **callback** - 回调函数，没有参数。

接下来我们创建 file.js 文件，代码如下所示：

```
var fs = require("fs");
// tmp 目录必须存在
console.log("创建目录 /tmp/test/");
fs.mkdir("/tmp/test/",function(err){
   if (err) {
       return console.error(err);
   }
   console.log("目录创建成功。");
});
```

以上代码执行结果如下：

```
$ node file.js 
创建目录 /tmp/test/
目录创建成功。
```

可以添加 recursive: true 参数，不管创建的目录 /tmp 和 /tmp/a 是否存在：

```
fs.mkdir('/tmp/a/apple', { recursive: true }, (err) => {
  if (err) throw err;
});
```

------

### 读取目录

```
fs.readdir(path, callback)
```

参数使用说明如下：

- **path** - 文件路径。
- **callback** - 回调函数，回调函数带有两个参数err, files，err 为错误信息，files 为 目录下的文件数组列表。

接下来我们创建 file.js 文件，代码如下所示：

```js
var fs = require("fs");

console.log("查看 /tmp 目录");
fs.readdir("/tmp/",function(err, files){
   if (err) {
       return console.error(err);
   }
   files.forEach( function (file){
       console.log( file );
   });
});
```

以上代码执行结果如下：

```js
$ node file.js 
查看 /tmp 目录
input.out
output.out
test
test.txt
```

### 读取目录

```
fs.readdir(path, callback)
```

- **path** - 文件路径。
- **callback** - 回调函数，回调函数带有两个参数err, files，err 为错误信息，files 为 目录下的文件数组列表。

接下来我们创建 file.js 文件，代码如下所示：

```
var fs = require("fs");

console.log("查看 /tmp 目录");
fs.readdir("/tmp/",function(err, files){
   if (err) {
       return console.error(err);
   }
   files.forEach( function (file){
       console.log( file );
   });
});
```

以上代码执行结果如下：

```
$ node file.js 
查看 /tmp 目录
input.out
output.out
test
test.txt
```

### 删除目录

> - fs.rmdir(path, callback)
>
> - path - 文件路径。
>
> - callback - 回调函数，没有参数。

```js
var fs = require("fs");
// 执行前创建一个空的 /tmp/test 目录
console.log("准备删除目录 /tmp/test");
fs.rmdir("/tmp/test",function(err){
   if (err) {
       return console.error(err);
   }
   console.log("读取 /tmp 目录");
   fs.readdir("/tmp/",function(err, files){
      if (err) {
          return console.error(err);
      }
      files.forEach( function (file){
          console.log( file );
      });
   });
});
以上代码执行结果如下：

$ node file.js 
准备删除目录 /tmp/test
读取 /tmp 目录
……
```

### 文件模块方法参考手册

| 序号 | 方法 & 描述                                                                                                                                                              |
| :--- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | **fs.rename(oldPath, newPath, callback)** 异步 rename().回调函数没有参数，但可能抛出异常。                                                                               |
| 2    | **fs.ftruncate(fd, len, callback)** 异步 ftruncate().回调函数没有参数，但可能抛出异常。                                                                                  |
| 3    | **fs.ftruncateSync(fd, len)** 同步 ftruncate()                                                                                                                           |
| 4    | **fs.truncate(path, len, callback)** 异步 truncate().回调函数没有参数，但可能抛出异常。                                                                                  |
| 5    | **fs.truncateSync(path, len)** 同步 truncate()                                                                                                                           |
| 6    | **fs.chown(path, uid, gid, callback)** 异步 chown().回调函数没有参数，但可能抛出异常。                                                                                   |
| 7    | **fs.chownSync(path, uid, gid)** 同步 chown()                                                                                                                            |
| 8    | **fs.fchown(fd, uid, gid, callback)** 异步 fchown().回调函数没有参数，但可能抛出异常。                                                                                   |
| 9    | **fs.fchownSync(fd, uid, gid)** 同步 fchown()                                                                                                                            |
| 10   | **fs.lchown(path, uid, gid, callback)** 异步 lchown().回调函数没有参数，但可能抛出异常。                                                                                 |
| 11   | **fs.lchownSync(path, uid, gid)** 同步 lchown()                                                                                                                          |
| 12   | **fs.chmod(path, mode, callback)** 异步 chmod().回调函数没有参数，但可能抛出异常。                                                                                       |
| 13   | **fs.chmodSync(path, mode)** 同步 chmod().                                                                                                                               |
| 14   | **fs.fchmod(fd, mode, callback)** 异步 fchmod().回调函数没有参数，但可能抛出异常。                                                                                       |
| 15   | **fs.fchmodSync(fd, mode)** 同步 fchmod().                                                                                                                               |
| 16   | **fs.lchmod(path, mode, callback)** 异步 lchmod().回调函数没有参数，但可能抛出异常。Only available on Mac OS X.                                                          |
| 17   | **fs.lchmodSync(path, mode)** 同步 lchmod().                                                                                                                             |
| 18   | **fs.stat(path, callback)** 异步 stat(). 回调函数有两个参数 err, stats，stats 是 fs.Stats 对象。                                                                         |
| 19   | **fs.lstat(path, callback)** 异步 lstat(). 回调函数有两个参数 err, stats，stats 是 fs.Stats 对象。                                                                       |
| 20   | **fs.fstat(fd, callback)** 异步 fstat(). 回调函数有两个参数 err, stats，stats 是 fs.Stats 对象。                                                                         |
| 21   | **fs.statSync(path)** 同步 stat(). 返回 fs.Stats 的实例。                                                                                                                |
| 22   | **fs.lstatSync(path)** 同步 lstat(). 返回 fs.Stats 的实例。                                                                                                              |
| 23   | **fs.fstatSync(fd)** 同步 fstat(). 返回 fs.Stats 的实例。                                                                                                                |
| 24   | **fs.link(srcpath, dstpath, callback)** 异步 link().回调函数没有参数，但可能抛出异常。                                                                                   |
| 25   | **fs.linkSync(srcpath, dstpath)** 同步 link().                                                                                                                           |
| 26   | **fs.symlink(srcpath, dstpath[, type], callback)** 异步 symlink().回调函数没有参数，但可能抛出异常。 type 参数可以设置为 'dir', 'file', 或 'junction' (默认为 'file') 。 |
| 27   | **fs.symlinkSync(srcpath, dstpath[, type])** 同步 symlink().                                                                                                             |
| 28   | **fs.readlink(path, callback)** 异步 readlink(). 回调函数有两个参数 err, linkString。                                                                                    |
| 29   | **fs.realpath(path[, cache], callback)** 异步 realpath(). 回调函数有两个参数 err, resolvedPath。                                                                         |
| 30   | **fs.realpathSync(path[, cache])** 同步 realpath()。返回绝对路径。                                                                                                       |
| 31   | **fs.unlink(path, callback)** 异步 unlink().回调函数没有参数，但可能抛出异常。                                                                                           |
| 32   | **fs.unlinkSync(path)** 同步 unlink().                                                                                                                                   |
| 33   | **fs.rmdir(path, callback)** 异步 rmdir().回调函数没有参数，但可能抛出异常。                                                                                             |
| 34   | **fs.rmdirSync(path)** 同步 rmdir().                                                                                                                                     |
| 35   | **fs.mkdir(path[, mode], callback)** S异步 mkdir(2).回调函数没有参数，但可能抛出异常。 访问权限默认为 0777。                                                             |
| 36   | **fs.mkdirSync(path[, mode])** 同步 mkdir().                                                                                                                             |
| 37   | **fs.readdir(path, callback)** 异步 readdir(3). 读取目录的内容。                                                                                                         |
| 38   | **fs.readdirSync(path)** 同步 readdir().返回文件数组列表。                                                                                                               |
| 39   | **fs.close(fd, callback)** 异步 close().回调函数没有参数，但可能抛出异常。                                                                                               |
| 40   | **fs.closeSync(fd)** 同步 close().                                                                                                                                       |
| 41   | **fs.open(path, flags[, mode], callback)** 异步打开文件。                                                                                                                |
| 42   | **fs.openSync(path, flags[, mode])** 同步 version of fs.open().                                                                                                          |
| 43   | **fs.utimes(path, atime, mtime, callback)**                                                                                                                              |
| 44   | **fs.utimesSync(path, atime, mtime)** 修改文件时间戳，文件通过指定的文件路径。                                                                                           |
| 45   | **fs.futimes(fd, atime, mtime, callback)**                                                                                                                               |
| 46   | **fs.futimesSync(fd, atime, mtime)** 修改文件时间戳，通过文件描述符指定。                                                                                                |
| 47   | **fs.fsync(fd, callback)** 异步 fsync.回调函数没有参数，但可能抛出异常。                                                                                                 |
| 48   | **fs.fsyncSync(fd)** 同步 fsync.                                                                                                                                         |
| 49   | **fs.write(fd, buffer, offset, length[, position], callback)** 将缓冲区内容写入到通过文件描述符指定的文件。                                                              |
| 50   | **fs.write(fd, data[, position[, encoding]], callback)** 通过文件描述符 fd 写入文件内容。                                                                                |
| 51   | **fs.writeSync(fd, buffer, offset, length[, position])** 同步版的 fs.write()。                                                                                           |
| 52   | **fs.writeSync(fd, data[, position[, encoding]])** 同步版的 fs.write().                                                                                                  |
| 53   | **fs.read(fd, buffer, offset, length, position, callback)** 通过文件描述符 fd 读取文件内容。                                                                             |
| 54   | **fs.readSync(fd, buffer, offset, length, position)** 同步版的 fs.read.                                                                                                  |
| 55   | **fs.readFile(filename[, options], callback)** 异步读取文件内容。                                                                                                        |
| 56   | **fs.readFileSync(filename[, options])**                                                                                                                                 |
| 57   | **fs.writeFile(filename, data[, options], callback)** 异步写入文件内容。                                                                                                 |
| 58   | **fs.writeFileSync(filename, data[, options])** 同步版的 fs.writeFile。                                                                                                  |
| 59   | **fs.appendFile(filename, data[, options], callback)** 异步追加文件内容。                                                                                                |
| 60   | **fs.appendFileSync(filename, data[, options])** The 同步 version of fs.appendFile.                                                                                      |
| 61   | **fs.watchFile(filename[, options], listener)** 查看文件的修改。                                                                                                         |
| 62   | **fs.unwatchFile(filename[, listener])** 停止查看 filename 的修改。                                                                                                      |
| 63   | **fs.watch(filename[, options][, listener])** 查看 filename 的修改，filename 可以是文件或目录。返回 fs.FSWatcher 对象。                                                  |
| 64   | **fs.exists(path, callback)** 检测给定的路径是否存在。                                                                                                                   |
| 65   | **fs.existsSync(path)** 同步版的 fs.exists.                                                                                                                              |
| 66   | **fs.access(path[, mode], callback)** 测试指定路径用户权限。                                                                                                             |
| 67   | **fs.accessSync(path[, mode])** 同步版的 fs.access。                                                                                                                     |
| 68   | **fs.createReadStream(path[, options])** 返回ReadStream 对象。                                                                                                           |
| 69   | **fs.createWriteStream(path[, options])** 返回 WriteStream 对象。                                                                                                        |
| 70   | **fs.symlink(srcpath, dstpath[, type], callback)** 异步 symlink().回调函数没有参数，但可能抛出异常。                                                                     |

## GET/POST请求

### 获取GET请求内容

```js
var http = require('http');
var url = require('url');
var util = require('util');
 
http.createServer(function(req, res){
    res.writeHead(200, {'Content-Type': 'text/plain; charset=utf-8'});
    res.end(util.inspect(url.parse(req.url, true)));
}).listen(3000);

//获取url参数
http.createServer(function(req, res){
    res.writeHead(200, {'Content-Type': 'text/plain'});
 
    // 解析 url 参数
    var params = url.parse(req.url, true).query;
    res.write("网站名：" + params.name);
    res.write("\n");
    res.write("网站 URL：" + params.url);
    res.end();

}).listen(3000);
```

<img src="https://images.gitee.com/uploads/images/2020/0602/182627_1b8e7ecd_6545143.png" style="zoom:67%;" />

### 获取post请求内容

```js
var http = require('http');
var querystring = require('querystring');
var util = require('util');
 
http.createServer(function(req, res){
    // 定义了一个post变量，用于暂存请求体的信息
    var post = '';     
 
    // 通过req的data事件监听函数，每当接受到请求体的数据，就累加到post变量中
    req.on('data', function(chunk){    
        post += chunk;
    });
 
    // 在end事件触发后，通过querystring.parse将post解析为真正的POST请求格式，然后向客户端返回。
    req.on('end', function(){    
        post = querystring.parse(post);
        res.end(util.inspect(post));
    });
```

```js
var http = require('http');
var querystring = require('querystring');
 
var postHTML = 
  '<html><head><meta charset="utf-8"><title>grh Node.js 实例</title></head>' +
  '<body>' +
  '<form method="post">' +
  '网站名： <input name="name"><br>' +
  '网站 URL： <input name="url"><br>' +
  '<input type="submit">' +
  '</form>' +
  '</body></html>';
 
http.createServer(function (req, res) {
  var body = "";
  req.on('data', function (chunk) {
    body += chunk;
  });
  req.on('end', function () {
    // 解析参数
    body = querystring.parse(body);
    // 设置响应头部信息及编码
    res.writeHead(200, {'Content-Type': 'text/html; charset=utf8'});
 
    if(body.name && body.url) { // 输出提交的数据
        res.write("网站名：" + body.name);
        res.write("<br>");
        res.write("网站 URL：" + body.url);
    } else {  // 输出表单
        res.write(postHTML);
    }
    res.end();
  });
}).listen(3000);
```
