# node-expand

## nodejs使用fetch

node 中没有实现 fetch，你可以使用 [node-fetch](https://github.com/bitinn/node-fetch)，使得在 node 中也可以使用 fetch.

安装 node-fetch:

```
npm install node-fetch
```

使用 fetch 之前先加载：

```
const fetch = require('node-fetch')
```

简单使用方法：

```
fetch('https://api.github.com/users/github')
    .then(res => res.json())
    .then(json => console.log(json));
```

关于 fetch 的使用介绍 <https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch>

## Nodemon自动重启项目

1. 安装：`npm install -g nodemon`
2. 使用`nodemon`运行项目，取代之前的`node app.js`。

```
nodemon  [your app.js]
```

项目运行之后，`nodemon`会自动监听代码的改动，并且重新启动服务，大大增加我们开发效率。

1. `nodemon`常见配置

- 在命令行指定应用的端口号：`nodemon ./server.js localhost 8080`
- 查看帮助，帮助里面有很多选项都是一目了然：`nodemon -h 或者 nodemon --help`
- 运行 debug 模式：`nodemon --debug ./server.js 80`
- 手动重启项目： `Nodemon` 命令运行的终端 窗口中输入 `rs` 两个字符，然后再按下回车键，就能手动重启 `Nodemon`了。

## 爬虫

### 1.爬虫介绍

通过模拟浏览器的请求，服务器就会根据我们的请求返回我们想要的数据，将数据解析出来，并且进行保存。

### 2.爬虫流程

#### 1-目标：确定你想要获取的数据

1. 确定想要的数据在什么页面上（一般详细的数据会在详情页）
2. 确定在哪些页面可以链接到这些页面（一般分类列表页面会有详情页的链接数据）
3. 寻找页面之间和数据之间的规律

#### 2-分析页面

1. 获取数据的方式（正则，cherrio）
2. 分析数据是通过ajax请求的数据，还是html里自带的数据
3. 如果是通过AJAX请求的数据，那么需要获取ajax请求的链接，一般请求到的数据都为JSON格式数据，那么就会比较容易解析。
4. 如何数据在HTML里面，那么就用cherrio通过选择器将内容选中

#### 3-编写单个数据获取的案例

1. 解析出分类页的链接地址
2. 解析出列表页的链接地址
3. 解析出详情页的链接地址
4. 解析详情页里面想要获取的数据
5. 将数据进行保存到本地或者是数据库

#### 4-如果遇到阻碍进行反爬虫对抗

1. User-Agent是否是正常浏览器的信息
2. 将请求头设置成跟浏览器一样的内容
3. 因为爬虫的爬取速度过快，会导致封号。1那么可以降低速度进行解决，2可以使用代理进行解决
4. 如果设置需要凭证，那么可以采用无界浏览器真实模拟。

### 2.请求数据的库

request，axios：通过库，帮助我们快速实现HTTP请求包的打包

```
request.get('请求地址', {
  '请求头字段': '请求头的value值'
},(res)=>{处理返回的内容});
```

axios优势会更明显，前后端通杀，前后端调用的方式一致。

```js
axios.get('请求地址',参数对象).then(function (response) {
    console.log(response);
})
```

axios获取图片

```js
axios({
  method:'get',
  url:'http://bit.ly/2mTM3nY',
  responseType:'stream'
})
.then(function(response) {
  response.data.pipe(fs.createWriteStream('ada_lovelace.jpg'))
});
```

puppeteer:完全模拟浏览器

打开浏览器

```js
let options = {
    headless:true,//是否是无界面浏览器
    slowMo:250,//调试时可以减慢操作速度
    defaultViewport:{
        width:1200,//设置视窗的宽高
        height:800
    },
    timeout:3000，//默认超时3秒
}
let browser =await puppeteer.launch(options);
```

打开新标签页

```js
let page = await browser.newPage()
```

获取所有浏览器中的页面

```js
let pages = await browser.pages()
```

关闭浏览器

```js
browser.close()
```

将页面跳转至

```
await page.goto(url)
```

获取页面的对象,并进行操作

```
let btn = await page.$(selector)
let input = await page.$(selector)
//点击按钮
btn.click()
//聚焦到输入框
input.forcus()
```

在页面上写入内容或者键盘按键

```
await page.keyboard.type('Hello World!');
await page.keyboard.press('ArrowLeft');
await page.keyboard.down('Shift');
```

设置鼠标的移动

```
await page.mouse.move(0, 0);
await page.mouse.down();
await page.mouse.move(0, 100);
await page.mouse.move(100, 100);
await page.mouse.move(100, 0);
await page.mouse.move(0, 0);
await page.mouse.up();
```

截获页面请求

```
await page.setRequestInterception(true);
page.on('request', request => {
    request.url()//可以获取请求的网址，request，包含了所有的请求信息
    if(你想要的条件){
        request.continue()
    }else{
        request.abort([errorCode])
    }
});
```

获取浏览器的信息和内容

```
page.$eval(selector,(item)=>{return item})
page.$$eval(selectors,(items)=>{return items})
```

## 静态服务器

## 网络通信原理

**1、协议及协议栈的基本概念**

**1.1、什么是协议**

协议是网络中计算机或设备之间进行通信的一系列规则的集合。常用协议有IP、TCP、HTTP、POP3、SMTP等。

**1.2、什么是协议栈**

在网络中，为了完成通信，必须使用多层上的多种协议。这些协议按照层次顺序组合在一起，构成了协议栈(Protocol Stack)，也称为协议族(Protocol Suite)。

**1.3、协议的作用**

一个网络协议的作用主要有两个：一是建立对等层之间的虚拟通信，二是实现层次之间的无关性。

**1.4、层次间的无关性**

所谓层次间无关性，就是指较高层次和相邻的相低层次进行通信时，只是利用较低层次提供的接口和服务，而不需了解底层实现该功能所采用的算法和协议的细节；较低层次也仅是使用从高层系统传送来的参数和控制信息，这就是层次间的无关性。

**2、网络协议族/栈组成**

网络通信协议的作用是负责在网络上建立通信通道和控制通过通道的信息流的规则。为了进行网络通信，通信双方必须遵守通信协议

<img src="https://ss1.baidu.com/6ONXsjip0QIZ8tyhnq/it/u=482594737,1881689515&amp;fm=173&amp;app=49&amp;f=JPEG?w=640&amp;h=506&amp;s=5986BE1A490E44CA5A4104EB03005032" alt="img" style="zoom:50%;" />

说明：网络协议与标准是基于OSI七层模型，每一层都有其对应的协议。

**3、常见的网络协议**

TCP/IP：工业标准、开放式协议，Internet网络的标准

IPX/SPX：Novell开发的Netware操作系统使用的协议，IPX为网际数据包交换协议，工作在网络层，SPX为序列数据包交换协议，工作在传输层。

NetBIOS/NetBEUI：较小的协议栈，应用于IBM和早期的Windows系统，现在Windows仍然支持。

AppleTalk：Apple公司的Mac OS中所采用的网络协议。

**（1）TCP/IP协议**

TCP/IP是分层协议，如层次图所示：从底层到应用层，分别是物理层，链路层，网络层，传输层，应用层。数据是层层封装，封装的方式一般都是在原有数据的前面加一个数据控制头。

![img](https://ss2.baidu.com/6ONYsjip0QIZ8tyhnq/it/u=2298966842,1075106660&fm=173&app=49&f=JPEG?w=640&h=388&s=C018C53889DF40C85EED91460300A0F1)

**（2）Telnet协议**

Telnet是TCP/IP中的一种应用协议，可以为终端仿真提供支持。可使用户连接到主机上，使主机响应起来就像它直接连接在终端上一样。Telnet在发送端和接收端使用TCP的23号端口以进行专用的通信。

IPV4

IP:1.1.1.1   ---   255.255.255.255  

IPV6

10亿  10亿 10亿 10亿

**（3）FTP协议**

FTP协议使用TCP20号和21号端口，20号端口用于数据交换，21号端口用于建立连接，允许目录和文件访问，上传下载，不能远程执行文件。

TFTP是简单文件传输协议（Trivial File Transfer Protocol，TFTP），TFTP是无连接的，使用UDP的69号端口，用于当数据传输错误无关紧要而且无须安全性时的小型文件的传输。

**（4）SMTP协议**

SMTP是简单邮件传输协议（Simple Mail Transfer Protocol，SMTP）是为网络系统间的电子邮件交换而设计的。使用 25 端口。SMTP只需要在接收端的一个电子邮件地址即可发送邮件。POP3 协议用来接收邮件.使用110端口

**（5）DNS服务**

DNS是域名解析服务（Domain Name Service, DNS），作用是将域名转换为IP地址，或将IP地址转换为域名，用于解析完全合格域名（FQDN）。使用53号端口。

**（6）DHCP服务**

DHCP是动态主机配置协议(DHCP)，服务器可以提供的信息有:

1、IP地址

2、子网掩码(subnet mask)

3、域名(domain name)

4、默认网关(default gateway)

5、DNS

## HTTP协议

HTTP协议是Hyper Text Transfer Protocol（超文本传输协议）的缩写,是用于从万维网（WWW:World Wide Web ）服务器传输超文本到本地浏览器的传送协议。

HTTP是一个基于TCP/IP通信协议来传递数据（HTML 文件, 图片文件, 查询结果等）。

HTTP是一个属于应用层的面向对象的协议，由于其简捷、快速的方式，适用于分布式超媒体信息系统。它于1990年提出，经过几年的使用与发展，得到不断地完善和扩展。目前在WWW中使用的是HTTP/1.0的第六版，HTTP/1.1的规范化工作正在进行之中，而且HTTP-NG(Next Generation of HTTP)的建议已经提出。

HTTP协议工作于客户端-服务端架构为上。浏览器作为HTTP客户端通过URL向HTTP服务端即WEB服务器发送所有请求。Web服务器根据接收到的请求后，向客户端发送响应信息。

### HTTP协议的主要特点

1、简单快速：客户向服务器请求服务时，只需传送请求方法和路径。请求方法常用的有GET、HEAD、POST。每种方法规定了客户与服务器联系的类型不同。由于HTTP协议简单，使得HTTP服务器的程序规模小，因而通信速度很快。

2、灵活：HTTP允许传输任意类型的数据对象。正在传输的类型由Content-Type加以标记。

3.无连接：无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。

4.无状态：HTTP协议是无状态协议。无状态是指协议对于事务处理没有记忆能力。缺少状态意味着如果后续处理需要前面的信息，则它必须重传，这样可能导致每次连接传送的数据量增大。另一方面，在服务器不需要先前信息时它的应答就较快。 5、支持B/S及C/S模式

### HTTP协议与URL的关系

HTTP使用统一资源标识符（Uniform Resource Identifiers, URI）来传输数据和建立连接。URL是一种特殊类型的URI，包含了用于查找某个资源的足够的信息。

URL,全称是UniformResourceLocator, 中文叫统一资源定位符,是互联网上用来标识某一处资源的地址。以下面这个URL为例，介绍下普通URL的各部分组成：

```
http://www.aspxfans.com:8080/news/index.asp?boardID=5&ID=24618&page=1#name
```

##### 从上面的URL可以看出，一个完整的URL包括以下几部分

1.协议部分：该URL的协议部分为“http”，这代表网页使用的是HTTP协议。在Internet中可以使用多种协议，如HTTP，FTP等等本例中使用的是HTTP协议。在"HTTP"后面的“//”为分隔符。

2.域名部分：该URL的域名部分为“[www.aspxfans.com](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.aspxfans.com)”。一个URL中，也可以使用IP地址作为域名使用。

3.端口部分：跟在域名后面的是端口，域名和端口之间使用“:”作为分隔符。端口不是一个URL必须的部分，如果省略端口部分，将采用默认端口。

4.虚拟目录部分：从域名后的第一个“/”开始到最后一个“/”为止，是虚拟目录部分。虚拟目录也不是一个URL必须的部分。本例中的虚拟目录是“/news/”。

5.文件名部分：从域名后的最后一个“/”开始到“？”为止，是文件名部分，如果没有“?”,则是从域名后的最后一个“/”开始到“#”为止，是文件部分，如果没有“？”和“#”，那么从域名后的最后一个“/”开始到结束，都是文件名部分。本例中的文件名是“index.asp”。文件名部分也不是一个URL必须的部分，如果省略该部分，则使用默认的文件名。

6.锚部分：从“#”开始到最后，都是锚部分。本例中的锚部分是“name”。锚部分也不是一个URL必须的部分。

7.参数部分：从“？”开始到“#”为止之间的部分为参数部分，又称搜索部分、查询部分。本例中的参数部分为“boardID=5&ID=24618&page=1”。参数可以允许有多个参数，参数与参数之间用“&”作为分隔符。

\#

### URI和URL的区别

##### URI，是uniform resource identifier，统一资源标识符，用来唯一的标识一个资源

Web上可用的每种资源如HTML文档、图像、视频片段、程序等都是一个来URI来定位的 URI一般由三部组成： ①访问资源的命名机制 ②存放资源的主机名 ③资源自身的名称，由路径表示，着重强调于资源。

##### URL是uniform resource locator，统一资源定位器，它是一种具体的URI，即URL可以用来标识一个资源，而且还指明了如何locate这个资源

URL是Internet上用来描述信息资源的字符串，主要用在各种WWW客户程序和服务器程序上，特别是著名的Mosaic。 采用URL可以用一种统一的格式来描述各种信息资源，包括文件、服务器的地址和目录等。URL一般由三部组成： ①协议(或称为服务方式) ②存有该资源的主机IP地址(有时也包括端口号) ③主机资源的具体地址。如目录和文件名等

##### URN，uniform resource name，统一资源命名，是通过名字来标识资源，比如[mailto:java-net@java.sun.com](https://links.jianshu.com/go?to=mailto%3Ajava-net%40java.sun.com)

URI是以一种抽象的，高层次概念定义统一资源标识，而URL和URN则是具体的资源标识的方式。URL和URN都是一种URI。笼统地说，每个 URL 都是 URI，但不一定每个 URI 都是 URL。这是因为 URI 还包括一个子类，即统一资源名称 (URN)，它命名资源但不指定如何定位资源。上面的 mailto、news 和 isbn URI 都是 URN 的示例。

在Java的URI中，一个URI实例可以代表绝对的，也可以是相对的，只要它符合URI的语法规则。而URL类则不仅符合语义，还包含了定位该资源的信息，因此它不能是相对的。 在Java类库中，URI类不包含任何访问资源的方法，它唯一的作用就是解析。 相反的是，URL类可以打开一个到达资源的流。

> HTTP之请求消息Request

客户端发送一个HTTP请求到服务器的请求消息包括以下格式：

##### 请求行（request line）、请求头部（header）、空行和请求数据四个部分组成

![img](https:////upload-images.jianshu.io/upload_images/2964446-fdfb1a8fce8de946.png?imageMogr2/auto-orient/strip|imageView2/2/w/466/format/webp)

image

- 请求行以一个方法符号开头，以空格分开，后面跟着请求的URI和协议的版本。

##### Get请求例子，使用Charles抓取的request

\#

```
GET /562f25980001b1b106000338.jpg HTTP/1.1
Host    img.mukewang.com
User-Agent    Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.106 Safari/537.36
Accept    image/webp,image/*,*/*;q=0.8
Referer    http://www.imooc.com/
Accept-Encoding    gzip, deflate, sdch
Accept-Language    zh-CN,zh;q=0.8
```

##### 第一部分：请求行，用来说明请求类型,要访问的资源以及所使用的HTTP版本

GET说明请求类型为GET,[/562f25980001b1b106000338.jpg]为要访问的资源，该行的最后一部分说明使用的是HTTP1.1版本。

##### 第二部分：请求头部，紧接着请求行（即第一行）之后的部分，用来说明服务器要使用的附加信息

从第二行起为请求头部，HOST将指出请求的目的地.User-Agent,服务器端和客户端脚本都能访问它,它是浏览器类型检测逻辑的重要基础.该信息由你的浏览器来定义,并且在每个请求中自动发送等等

##### 第三部分：空行，请求头部后面的空行是必须的

即使第四部分的请求数据为空，也必须有空行。

##### 第四部分：请求数据也叫主体，可以添加任意的其他数据

这个例子的请求数据为空。

##### POST请求例子，使用Charles抓取的request

\#

```
POST / HTTP1.1
Host:www.wrox.com
User-Agent:Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; .NET CLR 3.0.04506.648; .NET CLR 3.5.21022)
Content-Type:application/x-www-form-urlencoded
Content-Length:40
Connection: Keep-Alive

name=Professional%20Ajax&publisher=Wiley
```

第一部分：请求行，第一行明了是post请求，以及http1.1版本。 第二部分：请求头部，第二行至第六行。 第三部分：空行，第七行的空行。 第四部分：请求数据，第八行。

> HTTP之响应消息Response

一般情况下，服务器接收并处理客户端发过来的请求后会返回一个HTTP的响应消息。

##### HTTP响应也由四个部分组成，分别是：状态行、消息报头、空行和响应正文

![img](https:////upload-images.jianshu.io/upload_images/2964446-1c4cab46f270d8ee.jpg?imageMogr2/auto-orient/strip|imageView2/2/w/683/format/webp)

image

##### 例子

```
HTTP/1.1 200 OK
Date: Fri, 22 May 2009 06:07:21 GMT
Content-Type: text/html; charset=UTF-8

<html>
      <head></head>
      <body>
            <!--body goes here-->
      </body>
</html>
```

##### 第一部分：状态行，由HTTP协议版本号， 状态码， 状态消息 三部分组成

第一行为状态行，（HTTP/1.1）表明HTTP版本为1.1版本，状态码为200，状态消息为（ok）

##### 第二部分：消息报头，用来说明客户端要使用的一些附加信息

第二行和第三行为消息报头， Date:生成响应的日期和时间；Content-Type:指定了MIME类型的HTML(text/html),编码类型是UTF-8

##### 第三部分：空行，消息报头后面的空行是必须的

##### 第四部分：响应正文，服务器返回给客户端的文本信息

空行后面的html部分为响应正文。

> HTTP协议之状态码

状态代码有三位数字组成，第一个数字定义了响应的类别，共分五种类别:

###### 1xx：指示信息--表示请求已接收，继续处理

###### 2xx：成功--表示请求已被成功接收、理解、接受

###### 3xx：重定向--要完成请求必须进行更进一步的操作

###### 4xx：客户端错误--请求有语法错误或请求无法实现

###### 5xx：服务器端错误--服务器未能实现合法的请求

常见状态码：

200 OK                        //客户端请求成功

400 Bad Request               //客户端请求有语法错误，不能被服务器所理解

401 Unauthorized              //请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用

403 Forbidden                 //服务器收到请求，但是拒绝提供服务

404 Not Found                 //请求资源不存在，eg：输入了错误的URL

500 Internal Server Error     //服务器发生不可预期的错误

503 Server Unavailable        //服务器当前不能处理客户端的请求，一段时间后可能恢复正常

### HTTP请求方法

根据HTTP标准，HTTP请求可以使用多种请求方法。 HTTP1.0定义了三种请求方法： GET, POST 和 HEAD方法。 HTTP1.1新增了五种请求方法：OPTIONS, PUT, DELETE, TRACE 和 CONNECT 方法。

GET     请求指定的页面信息，并返回实体主体。

HEAD     类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头

POST     向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。

PUT     从客户端向服务器传送的数据取代指定的文档的内容。

DELETE      请求服务器删除指定的页面。

CONNECT     HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。

OPTIONS     允许客户端查看服务器的性能。 TRACE     回显服务器收到的请求，主要用于测试或诊断。`

> HTTP工作原理

HTTP协议定义Web客户端如何从Web服务器请求Web页面，以及服务器如何把Web页面传送给客户端。HTTP协议采用了请求/响应模型。客户端向服务器发送一个请求报文，请求报文包含请求的方法、URL、协议版本、请求头部和请求数据。服务器以一个状态行作为响应，响应的内容包括协议的版本、成功或者错误代码、服务器信息、响应头部和响应数据。

以下是 HTTP 请求/响应的步骤：

###### 1、客户端连接到Web服务器

一个HTTP客户端，通常是浏览器，与Web服务器的HTTP端口（默认为80）建立一个TCP套接字连接。例如，[http://www.oakcms.cn。](https://links.jianshu.com/go?to=http%3A%2F%2Fwww.oakcms.cn.%2F)

###### 2、发送HTTP请求

通过TCP套接字，客户端向Web服务器发送一个文本的请求报文，一个请求报文由请求行、请求头部、空行和请求数据4部分组成。

###### 3、服务器接受请求并返回HTTP响应

Web服务器解析请求，定位请求资源。服务器将资源复本写到TCP套接字，由客户端读取。一个响应由状态行、响应头部、空行和响应数据4部分组成。

###### 4、释放连接[TCP连接](https://www.jianshu.com/p/ef892323e68f)

若connection 模式为close，则服务器主动关闭[TCP连接](https://www.jianshu.com/p/ef892323e68f)，客户端被动关闭连接，释放[TCP连接](https://www.jianshu.com/p/ef892323e68f);若connection 模式为keepalive，则该连接会保持一段时间，在该时间内可以继续接收请求;

###### 5、客户端浏览器解析HTML内容

客户端浏览器首先解析状态行，查看表明请求是否成功的状态代码。然后解析每一个响应头，响应头告知以下为若干字节的HTML文档和文档的字符集。客户端浏览器读取响应数据HTML，根据HTML的语法对其进行格式化，并在浏览器窗口中显示。

例如：在浏览器地址栏键入URL，按下回车之后会经历以下流程：

1、浏览器向 DNS 服务器请求解析该 URL 中的域名所对应的 IP 地址;

2、解析出 IP 地址后，根据该 IP 地址和默认端口 80，和服务器建立[TCP连接](https://www.jianshu.com/p/ef892323e68f);

3、浏览器发出读取文件(URL 中域名后面部分对应的文件)的HTTP 请求，该请求报文作为 [TCP 三次握手](https://www.jianshu.com/p/ef892323e68f)的第三个报文的数据发送给服务器;

4、服务器对浏览器请求作出响应，并把对应的 html 文本发送给浏览器;

5、释放 [TCP连接](https://www.jianshu.com/p/ef892323e68f);

6、浏览器将该 html 文本并显示内容;

> GET和POST请求的区别

###### GET请求

```
GET /books/?sex=man&name=Professional HTTP/1.1 Host: www.wrox.com User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6) Gecko/20050225 Firefox/1.0.1 Connection: Keep-Alive
```

注意最后一行是空行

###### POST请求

```
`POST / HTTP/1.1 Host: www.wrox.com User-Agent: Mozilla/5.0 (Windows; U; Windows NT 5.1; en-US; rv:1.7.6) Gecko/20050225 Firefox/1.0.1 Content-Type: application/x-www-form-urlencoded Content-Length: 40 Connection: Keep-Alive
```

name=Professional%20Ajax&publisher=Wiley`

1、GET提交，请求的数据会附在URL之后（就是把数据放置在HTTP协议头中），以?分割URL和传输数据，多个参数用&连接；例 如：login.action?name=hyddd&password=idontknow&verify=%E4%BD%A0 %E5%A5%BD。如果数据是英文字母/数字，原样发送，如果是空格，转换为+，如果是中文/其他字符，则直接把字符串用BASE64加密，得出如： %E4%BD%A0%E5%A5%BD，其中％XX中的XX为该符号以16进制表示的ASCII。

POST提交：把提交的数据放置在是HTTP包的包体中。上文示例中红色字体标明的就是实际的传输数据

###### 因此，GET提交的数据会在地址栏中显示出来，而POST提交，地址栏不会改变

2、传输数据的大小：首先声明：HTTP协议没有对传输的数据大小进行限制，HTTP协议规范也没有对URL长度进行限制。

而在实际开发中存在的限制主要有：

**GET**:特定浏览器和服务器对URL长度有限制，例如 IE对URL长度的限制是2083字节(2K+35)。对于其他浏览器，如Netscape、FireFox等，理论上没有长度限制，其限制取决于操作系 统的支持。

因此对于GET提交时，传输数据就会受到URL长度的 限制。

**POST**:由于不是通过URL传值，理论上数据不受 限。但实际各个WEB服务器会规定对post提交数据大小进行限制，Apache、IIS6都有各自的配置。

3、安全性

POST的安全性要比GET的安全性高。比如：通过GET提交数据，用户名和密码将明文出现在URL上，因为(1)登录页面有可能被浏览器缓存；(2)其他人查看浏览器的历史纪录，那么别人就可以拿到你的账号和密码了，除此之外，使用GET提交数据还可能会造成Cross-site request forgery攻击

4、Http get,post,soap协议都是在http上运行的

（1）get：请求参数是作为一个key/value对的序列（查询字符串）附加到URL上的 查询字符串的长度受到web浏览器和web服务器的限制（如IE最多支持2048个字符），不适合传输大型数据集同时，它很不安全

（2）post：请求参数是在http标题的一个不同部分（名为entity body）传输的，这一部分用来传输表单信息，因此必须将Content-type设置为:application/x-www-form- urlencoded。post设计用来支持web窗体上的用户字段，其参数也是作为key/value对传输。 但是：它不支持复杂数据类型，因为post没有定义传输数据结构的语义和规则。

（3）soap：是http post的一个专用版本，遵循一种特殊的xml消息格式 Content-type设置为: text/xml 任何数据都可以xml化。

Http协议定义了很多与服务器交互的方法，最基本的有4种，分别是GET,POST,PUT,DELETE. 一个URL地址用于描述一个网络上的资源，而HTTP中的GET, POST, PUT, DELETE就对应着对这个资源的查，改，增，删4个操作。 我们最常见的就是GET和POST了。GET一般用于获取/查询资源信息，而POST一般用于更新资源信息.

我们看看GET和POST的区别

1. GET提交的数据会放在URL之后，以?分割URL和传输数据，参数之间以&相连，如EditPosts.aspx?name=test1&id=123456. POST方法是把提交的数据放在HTTP包的Body中.
2. GET提交的数据大小有限制（因为浏览器对URL的长度有限制），而POST方法提交的数据没有限制.
3. GET方式需要使用Request.QueryString来取得变量的值，而POST方式通过Request.Form来获取变量的值。
4. GET方式提交数据，会带来安全问题，比如一个登录页面，通过GET方式提交数据时，用户名和密码将出现在URL上，如果页面可以被缓存或者其他人可以访问这台机器，就可以从历史记录获得该用户的账号和密码.

## 工具模块

| 序号 | 模块名 & 描述                                                |
| :--- | :----------------------------------------------------------- |
| 1    | **OS 模块** 提供基本的系统操作函数。                         |
| 2    | **Path 模块** 提供了处理和转换文件路径的工具。               |
| 3    | **Net 模块** 用于底层的网络通信。提供了服务端和客户端的的操作。 |
| 4    | **DNS 模块** 用于解析域名。                                  |
| 5    | **Domain 模块** 简化异步代码的异常处理，可以捕捉处理try catch无法捕捉的。 |

## web模块

<img src="https://images.gitee.com/uploads/images/2020/0602/183736_ca5004eb_6545143.png" style="zoom: 87%;" />

> - **Client** - 客户端，一般指浏览器，浏览器可以通过 HTTP 协议向服务器请求数据。
> - **Server** - 服务端，一般指 Web 服务器，可以接收客户端请求，并向客户端发送响应数据。
> - **Business** - 业务层， 通过 Web 服务器处理应用程序，如与数据库交互，逻辑运算，调用外部程序等。
> - **Data** - 数据层，一般由数据库组成。

### Node 创建 Web 服务器

server.js

```js
var http = require('http');
var fs = require('fs');
var url = require('url');
 
 
// 创建服务器
http.createServer( function (request, response) {  
   // 解析请求，包括文件名
   var pathname = url.parse(request.url).pathname;
   
   // 输出请求的文件名
   console.log("Request for " + pathname + " received.");
   
   // 从文件系统中读取请求的文件内容
   fs.readFile(pathname.substr(1), function (err, data) {
      if (err) {
         console.log(err);
         // HTTP 状态码: 404 : NOT FOUND
         // Content Type: text/html
         response.writeHead(404, {'Content-Type': 'text/html'});
      }else{             
         // HTTP 状态码: 200 : OK
         // Content Type: text/html
         response.writeHead(200, {'Content-Type': 'text/html'});    
         
         // 响应文件内容
         response.write(data.toString());        
      }
      //  发送响应数据
      response.end();
   });   
}).listen(8080);
 
// 控制台会输出以下信息
console.log('Server running at http://127.0.0.1:8080/');
```

index.html

```js
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>grh(grh.com)</title>
</head>
<body>
    <h1>我的第一个标题</h1>
    <p>我的第一个段落。</p>
</body>
</html>
```

### Node创建Web客户端

client.js

```js
var http = require('http');
 
// 用于请求的选项
var options = {
   host: 'localhost',
   port: '8080',
   path: '/index.html'  
};
 
// 处理响应的回调函数
var callback = function(response){
   // 不断更新数据
   var body = '';
   response.on('data', function(data) {
      body += data;
   });
   
   response.on('end', function() {
      // 数据接收完成
      console.log(body);
   });
}
// 向服务端发送请求
var req = http.request(options, callback);
req.end();
```

## Express框架

> Express 框架核心特性：
>
> - 可以设置中间件来响应 HTTP 请求。
> - 定义了路由表用于执行不同的 HTTP 请求动作。
> - 可以通过向模板传递参数来动态渲染 HTML 页面。

### 安装

`$ npm install express --save`

> 会安装到node_modules目录中

几个重要的模块

> - **body-parser** - node.js 中间件，用于处理 JSON, Raw, Text 和 URL 编码的数据。
> - **cookie-parser** - 这就是一个解析Cookie的工具。通过req.cookies可以取到传过来的cookie，并把它们转成对象。
> - **multer** - node.js 中间件，用于处理 enctype="multipart/form-data"（设置表单的MIME编码）的表单数据。

```
cnpm install body-parser --save
cnpm install cookie-parser --save
cnpm install multer --save
```

### Express实例

```js
//express_demo.js 文件
var express = require('express');
var app = express();
 
app.get('/', function (req, res) {
   res.send('Hello World');
})
 
var server = app.listen(8081, function () {
 
  var host = server.address().address
  var port = server.address().port
 
  console.log("应用实例，访问地址为 http://%s:%s", host, port)
 
})
```

```shell
$ node express_demo.js 
应用实例，访问地址为 http://0.0.0.0:8081
```

### 请求和响应

> Express 应用使用回调函数的参数： **request** 和 **response** 对象来处理请求和响应的数据。

```js
app.get('/', function (req, res) {
   // --
})
```

#### **Request 对象**

> - request 对象表示 HTTP 请求，包含了请求查询字符串，参数，内容，HTTP 头部等属性。常见属性有：
>
> 1. req.app：当callback为外部文件时，用req.app访问express的实例
> 2. req.baseUrl：获取路由当前安装的URL路径
> 3. req.body / req.cookies：获得「请求主体」/ Cookies
> 4. req.fresh / req.stale：判断请求是否还「新鲜」
> 5. req.hostname / req.ip：获取主机名和IP地址
> 6. req.originalUrl：获取原始请求URL
> 7. req.params：获取路由的parameters
> 8. req.path：获取请求路径
> 9. req.protocol：获取协议类型
> 10. req.query：获取URL的查询参数串
> 11. req.route：获取当前匹配的路由
> 12. req.subdomains：获取子域名
> 13. req.accepts()：检查可接受的请求的文档类型
> 14. req.acceptsCharsets / req.acceptsEncodings / req.acceptsLanguages：返回指定字符集的第一个可接受字符编码
> 15. req.get()：获取指定的HTTP请求头
> 16. req.is()：判断请求头Content-Type的MIME类型

#### **Response 对象**

> - response 对象表示 HTTP 响应，即在接收到请求时向客户端发送的 HTTP 响应数据。常见属性有：
>
> 1. res.app：同req.app一样
> 2. res.append()：追加指定HTTP头
> 3. res.set()在res.append()后将重置之前设置的头
> 4. res.cookie(name，value [，option])：设置Cookie
> 5. opition: domain / expires / httpOnly / maxAge / path / secure / signed
> 6. res.clearCookie()：清除Cookie
> 7. res.download()：传送指定路径的文件
> 8. res.get()：返回指定的HTTP头
> 9. res.json()：传送JSON响应
> 10. res.jsonp()：传送JSONP响应
> 11. res.location()：只设置响应的Location HTTP头，不设置状态码或者close response
> 12. res.redirect()：设置响应的Location HTTP头，并且设置状态码302
> 13. res.render(view,[locals],callback)：渲染一个view，同时向callback传递渲染后的字符串，如果在渲染过程中有错误发生next(err)将会被自动调用。callback将会被传入一个可能发生的错误以及渲染后的页面，这样就不会自动输出了。
> 14. res.send()：传送HTTP响应
> 15. res.sendFile(path [，options] [，fn])：传送指定路径的文件 -会自动根据文件extension设定Content-Type
> 16. res.set()：设置HTTP头，传入object可以一次设置多个头
> 17. res.status()：设置HTTP状态码
> 18. res.type()：设置Content-Type的MIME类型

### 路由

> 路由决定了由谁(指定脚本)去响应客户端请求。

```js
var express = require('express');
var app = express();
 
//  主页输出 "Hello World"
app.get('/', function (req, res) {
   console.log("主页 GET 请求");
   res.send('Hello GET');
})
 
 
//  POST 请求
app.post('/', function (req, res) {
   console.log("主页 POST 请求");
   res.send('Hello POST');
})
 
//  /del_user 页面响应
app.get('/del_user', function (req, res) {
   console.log("/del_user 响应 DELETE 请求");
   res.send('删除页面');
})
 
//  /list_user 页面 GET 请求
app.get('/list_user', function (req, res) {
   console.log("/list_user GET 请求");
   res.send('用户列表页面');
})
 
// 对页面 abcd, abxcd, ab123cd, 等响应 GET 请求
app.get('/ab*cd', function(req, res) {   
   console.log("/ab*cd GET 请求");
   res.send('正则匹配');
})
 
 
var server = app.listen(8081, function () {
 
  var host = server.address().address
  var port = server.address().port
 
  console.log("应用实例，访问地址为 http://%s:%s", host, port)
 
})
```

### 静态文件

> - Express 提供了内置的中间件 **express.static** 来设置静态文件如：图片， CSS, JavaScript 等。
>
> - 使用 **express.static** 中间件来设置静态文件路径。
>   - 例如，如果你将图片， CSS, JavaScript 文件放在 public 目录下

```js
var express = require('express');
var app = express();
 
app.use('/public', express.static('public'));
/*
node_modules
server.js
public/
public/images
public/images/logo.png
*/
 
app.get('/', function (req, res) {
   res.send('Hello World');
})
 
var server = app.listen(8081, function () {
 
  var host = server.address().address
  var port = server.address().port
 
  console.log("应用实例，访问地址为 http://%s:%s", host, port)
 
})
```

`http://127.0.0.1:8081/public/images/logo.png`可以访问到图片

### 文件上传

```js
<html>
<head>
<title>文件上传表单</title>
</head>
<body>
<h3>文件上传：</h3>
选择一个文件上传: <br />
<form action="/file_upload" method="post" enctype="multipart/form-data">
  <input type="file" name="image" size="50" />
  <br />
  <input type="submit" value="上传文件" />
</form>
</body>
</html>
```

```js
var express = require('express');
var app = express();
var fs = require("fs");
 
var bodyParser = require('body-parser');
var multer  = require('multer');
 
app.use('/public', express.static('public'));
app.use(bodyParser.urlencoded({ extended: false }));
app.use(multer({ dest: '/tmp/'}).array('image'));
 
app.get('/index.html', function (req, res) {
   res.sendFile( __dirname + "/" + "index.html" );
})
 
app.post('/file_upload', function (req, res) {
 
   console.log(req.files[0]);  // 上传的文件信息
  
   var des_file = __dirname + "/" + req.files[0].originalname;
   fs.readFile( req.files[0].path, function (err, data) {
        fs.writeFile(des_file, data, function (err) {
         if( err ){
              console.log( err );
         }else{
               response = {
                   message:'File uploaded successfully', 
                   filename:req.files[0].originalname
              };
          }
          console.log( response );
          res.end( JSON.stringify( response ) );
       });
   });
})
 
var server = app.listen(8081, function () {
 
  var host = server.address().address
  var port = server.address().port
 
  console.log("应用实例，访问地址为 http://%s:%s", host, port)
 
})
```

### Cookie管理

```js
// express_cookie.js 文件
var express = require('express')
var cookieParser = require('cookie-parser')
var util = require('util');
 
var app = express()
app.use(cookieParser())
 
app.get('/', function(req, res) {
    console.log("Cookies: " + util.inspect(req.cookies));
})
 
app.listen(8081)
```

## 多进程

> - node是单线程的模式运行的
> - 使用事件驱动来处理并发,有助于我们在多核cpu的系统上创建多个子进程,从而太高性能
> - 每个子进程有三个对象
>   - child.stdin
>   - child.stdout
>   - child.stder
>   - 可能会共享父进程的stdio流,或者是独立的被导流的流对象
> - Node提供child_process模块来创建子进程
>   - exec - child_process.exec 使用值进程执行命令,缓存子进程的输出,并将子进程的输出以回调函数参数的形式返回
>   - spawn - child_process.spawn 使用指定命令参数创建新进程
>   - fork - child_process.fork 是spawn()的特殊形式,用于在子进程的模块,如fork('./json.js') 相当于spawn('node',['./son.js'].
>     - 和spwan()方法不同,fork会在父进程与子进程自己拿,建立一个通信管道,用于进程中之间的通信

### exec() 方法

child_process.exec 使用子进程执行命令，缓存子进程的输出，并将子进程的输出以回调函数参数的形式返回。

语法如下所示：

```
child_process.exec(command[, options], callback)
```

#### 参数

参数说明如下：

**command：** 字符串， 将要运行的命令，参数使用空格隔开

**options ：对象，可以是：**

- cwd ，字符串，子进程的当前工作目录
- env，对象 环境变量键值对
- encoding ，字符串，字符编码（默认： 'utf8'）
- shell ，字符串，将要执行命令的 Shell（默认: 在 UNIX 中为`/bin/sh`， 在 Windows 中为`cmd.exe`， Shell 应当能识别 `-c`开关在 UNIX 中，或 `/s /c` 在 Windows 中。 在Windows 中，命令行解析应当能兼容`cmd.exe`）
- timeout，数字，超时时间（默认： 0）
- maxBuffer，数字， 在 stdout 或 stderr 中允许存在的最大缓冲（二进制），如果超出那么子进程将会被杀死 （默认: 200*1024）
- killSignal ，字符串，结束信号（默认：'SIGTERM'）
- uid，数字，设置用户进程的 ID
- gid，数字，设置进程组的 ID

**callback ：**回调函数，包含三个参数error, stdout 和 stderr。

**exec() 方法返回最大的缓冲区，并等待进程结束，一次性返回缓冲区的内容。**

#### 实例

#### support.js 文件代码

`console.log("进程 " + process.argv[2] + " 执行。" );`

#### master.js 文件代码

```js
const fs = require('fs');
const child_process = require('child_process');
 
for(var i=0; i<3; i++) {
    var workerProcess = child_process.exec('node support.js '+i, function (error, stdout, stderr) {
        if (error) {
            console.log(error.stack);
            console.log('Error code: '+error.code);
            console.log('Signal received: '+error.signal);
        }
        console.log('stdout: ' + stdout);
        console.log('stderr: ' + stderr);
    });
 
    workerProcess.on('exit', function (code) {
        console.log('子进程已退出，退出码 '+code);
    });
}
```

执行以上代码，输出结果为：

```
$ node master.js 
子进程已退出，退出码 0
stdout: 进程 1 执行。

stderr: 
子进程已退出，退出码 0
stdout: 进程 0 执行。

stderr: 
子进程已退出，退出码 0
stdout: 进程 2 执行。

stderr: 
```

### spawn()方法

child_process.spawn 使用指定的命令行参数创建新进程，语法格式如下：

```
child_process.spawn(command[, args][, options])
```

参数说明如下：

**command：** 将要运行的命令

**args：** Array 字符串参数数组

**options Object**

- cwd String 子进程的当前工作目录
- env Object 环境变量键值对
- stdio Array|String 子进程的 stdio 配置
- detached Boolean 这个子进程将会变成进程组的领导
- uid Number 设置用户进程的 ID
- gid Number 设置进程组的 ID

spawn() 方法返回流 (stdout & stderr)，在进程返回大量数据时使用。进程一旦开始执行时 spawn() 就开始接收响应。

#### 实例

让我们创建两个 js 文件 support.js 和 master.js。

#### support.js 文件代码

`console.log("进程 " + process.argv[2] + " 执行。" );`

#### master.js 文件代码

```js
const fs = require('fs');
const child_process = require('child_process');
 
for(var i=0; i<3; i++) {
   var workerProcess = child_process.spawn('node', ['support.js', i]);
 
   workerProcess.stdout.on('data', function (data) {
      console.log('stdout: ' + data);
   });
 
   workerProcess.stderr.on('data', function (data) {
      console.log('stderr: ' + data);
   });
 
   workerProcess.on('close', function (code) {
      console.log('子进程已退出，退出码 '+code);
   });
}
```

执行以上代码，输出结果为：

```
$ node master.js stdout: 进程 0 执行。

子进程已退出，退出码 0
stdout: 进程 1 执行。

子进程已退出，退出码 0
stdout: 进程 2 执行。

子进程已退出，退出码 0
```

### fork 方法

child_process.fork 是 spawn() 方法的特殊形式，用于创建进程，语法格式如下：

```
child_process.fork(modulePath[, args][, options])
```

参数说明如下：

**modulePath**： String，将要在子进程中运行的模块

**args**： Array 字符串参数数组

**options**：Object

- cwd String 子进程的当前工作目录
- env Object 环境变量键值对
- execPath String 创建子进程的可执行文件
- execArgv Array 子进程的可执行文件的字符串参数数组（默认： process.execArgv）
- silent Boolean 如果为`true`，子进程的`stdin`，`stdout`和`stderr`将会被关联至父进程，否则，它们将会从父进程中继承。（默认为：`false`）
- uid Number 设置用户进程的 ID
- gid Number 设置进程组的 ID

返回的对象除了拥有ChildProcess实例的所有方法，还有一个内建的通信信道。

#### 实例

让我们创建两个 js 文件 support.js 和 master.js。

#### support.js 文件代码

`console.log("进程 " + process.argv[2] + " 执行。" );`

#### master.js 文件代码

```js
const fs = require('fs');
const child_process = require('child_process');
 
for(var i=0; i<3; i++) {
   var worker_process = child_process.fork("support.js", [i]);    
 
   worker_process.on('close', function (code) {
      console.log('子进程已退出，退出码 ' + code);
   });
}
```

执行以上代码，输出结果为：

```
$ node master.js 
进程 0 执行。
子进程已退出，退出码 0
进程 1 执行。
子进程已退出，退出码 0
进程 2 执行。
子进程已退出，退出码 0
```

## Node 连接 mongoDB

### 安装驱动

```
cnpm install mongodb
```

------

### 创建数据库

> 要在 MongoDB 中创建一个数据库，首先我们需要创建一个 MongoClient 对象，然后配置好指定的 URL 和 端口号。
>
> 如果数据库不存在，MongoDB 将创建数据库并建立连接。

### 创建连接

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/grh";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
  if (err) throw err;
  console.log("数据库已创建!");
  db.close();
})
```

------

### 创建集合

我们可以使用 createCollection() 方法来创建集合：

```js
var MongoClient = require('mongodb').MongoClient;
var url = 'mongodb://localhost:27017/grh';
MongoClient.connect(url, { useNewUrlParser: true }, function (err, db) {
    if (err) throw err;
    console.log('数据库已创建');
    var dbase = db.db("grh");
    dbase.createCollection('site', function (err, res) {
        if (err) throw err;
        console.log("创建集合!");
        db.close();
    });
});
```

------

### 数据库操作( CURD )

#### 插入一条数据insertOne()

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
    if (err) throw err;
    var dbo = db.db("grh");
    var myobj = { name: "grh", url: "www.grh" };
    dbo.collection("site").insertOne(myobj, function(err, res) {
        if (err) throw err;
        console.log("文档插入成功");
        db.close();
    });
});
```

执行以下命令输出就结果为：

```
$ node test.js
文档插入成功
```

从输出结果来看，数据已插入成功。

我们也可以打开 MongoDB 的客户端查看数据，如：

```
> show dbs
grh  0.000GB          # 自动创建了 grh 数据库
> show tables
site                     # 自动创建了 site 集合（数据表）
> db.site.find()
{ "_id" : ObjectId("5a794e36763eb821b24db854"), "name" : "grh", "url" : "www.grh" }
> 
```

#### 插入多条数据 **insertMany()**

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
    if (err) throw err;
    var dbo = db.db("grh");
    var myobj =  [
        { name: '菜鸟工具', url: 'https://c.grh.com', type: 'cn'},
        { name: 'Google', url: 'https://www.google.com', type: 'en'},
        { name: 'Facebook', url: 'https://www.google.com', type: 'en'}
       ];
    dbo.collection("site").insertMany(myobj, function(err, res) {
        if (err) throw err;
        console.log("插入的文档数量为: " + res.insertedCount);
        db.close();
    });
});
```

res.insertedCount 为插入的条数。

#### 查询数据 find()

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
    if (err) throw err;
    var dbo = db.db("grh");
    dbo.collection("site"). find({}).toArray(function(err, result) { // 返回集合中所有数据
        if (err) throw err;
        console.log(result);
        db.close();
    });
});
```

#### 查询指定条件的数据

> 以下实例检索 name 为 "grh" 的实例：

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
    if (err) throw err;
    var dbo = db.db("grh");
     var whereStr = {"name":'grh'};  // 查询条件
    dbo.collection("site").find(whereStr).toArray(function(err, result) {
        if (err) throw err;
        console.log(result);
        db.close();
    });
});
```

执行以下命令输出就结果为：

```json
[ { _id: 5a794e36763eb821b24db854,
    name: 'grh',
    url: 'www.grh.com' } ]
```

#### 更新一条数据

> 将 name 为 "grh" 的 url 改为`https://www.grh.com`：

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
    if (err) throw err;
    var dbo = db.db("grh");
    var whereStr = {"name":'grh'};  // 查询条件
    var updateStr = {$set: { "url" : "https://www.grh.com" }};
    dbo.collection("site").updateOne(whereStr, updateStr, function(err, res) {
        if (err) throw err;
        console.log("文档更新成功");
        db.close();
    });
});
```

执行成功后，进入 mongo 管理工具查看数据已修改：

```
> db.site.find().pretty()
{
    "_id" : ObjectId("5a794e36763eb821b24db854"),
    "name" : "grh",
    "url" : "https://www.grh.com"     // 已修改为 https
}
```

如果要更新所有符合条的文档数据可以使用 **updateMany()**：

#### 更新多条数据

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
    if (err) throw err;
    var dbo = db.db("grh");
    var whereStr = {"type":'en'};  // 查询条件
    var updateStr = {$set: { "url" : "https://www.grh.com" }};
    dbo.collection("site").updateMany(whereStr, updateStr, function(err, res) {
        if (err) throw err;
         console.log(res.result.nModified + " 条文档被更新");
        db.close();
    });
});
```

result.nModified 为更新的条数。

---

#### 删除一条数据

> 将 name 为 "grh" 的数据删除 :

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
    if (err) throw err;
    var dbo = db.db("grh");
    var whereStr = {"name":'grh'};  // 查询条件
    dbo.collection("site").deleteOne(whereStr, function(err, obj) {
        if (err) throw err;
        console.log("文档删除成功");
        db.close();
    });
});
```

执行成功后，进入 mongo 管理工具查看数据已删除：

```
> db.site.find()
>
```

---

#### 删除多条数据

> 将 type 为 en 的所有数据删除 :
>
> 删除多条语句可以使用 **deleteMany()** 方法

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
    if (err) throw err;
    var dbo = db.db("grh");
    var whereStr = { type: "en" };  // 查询条件
    dbo.collection("site").deleteMany(whereStr, function(err, obj) {
        if (err) throw err;
        console.log(obj.result.n + " 条文档被删除");
        db.close();
    });
});
```

obj.result.n 删除的条数。

#### 排序

排序 使用 sort() 方法，该方法接受一个参数，规定是升序(1)还是降序(-1)。

```
{ type: 1 }  // 按 type 字段升序
{ type: -1 } // 按 type 字段降序
```

按 type 升序排列:

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
    if (err) throw err;
    var dbo = db.db("grh");
    var mysort = { type: 1 };
    dbo.collection("site").find().sort(mysort).toArray(function(err, result) {
        if (err) throw err;
        console.log(result);
        db.close();
    });
});
```

#### 查询分页

如果要设置指定的返回条数可以使用 **limit()** 方法，该方法只接受一个参数，指定了返回的条数。

##### limit()：读取两条数据

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
    if (err) throw err;
    var dbo = db.db("grh");
    dbo.collection("site").find().limit(2).toArray(function(err, result) {
        if (err) throw err;
        console.log(result);
        db.close();
  });
});
```

如果要指定跳过的条数，可以使用 **skip()** 方法。

##### skip(): 跳过前面两条数据，读取两条数据

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
    if (err) throw err;
    var dbo = db.db("grh");
    dbo.collection("site").find().skip(2).limit(2).toArray(function(err, result) {
        if (err) throw err;
        console.log(result);
        db.close();
  });
});
```

#### 连接操作

mongoDB 不是一个关系型数据库，但我们可以使用 **$lookup** 来实现左连接。

例如我们有两个集合数据分别为：

集合1：orders

```
[
  { _id: 1, product_id: 154, status: 1 }
]
```

集合2：products

```
[
  { _id: 154, name: '笔记本电脑' },
  { _id: 155, name: '耳机' },
  { _id: 156, name: '台式电脑' }
]
```

#### $lookup 实现左连接

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://127.0.0.1:27017/";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
  if (err) throw err;
  var dbo = db.db("grh");
  dbo.collection('orders').aggregate([
    { $lookup:
       {
         from: 'products',            // 右集合
         localField: 'product_id',    // 左集合 join 字段
         foreignField: '_id',         // 右集合 join 字段
         as: 'orderdetails'           // 新生成字段（类型array）
       }
     }
    ]).toArray(function(err, res) {
    if (err) throw err;
    console.log(JSON.stringify(res));
    db.close();
  });
});
```

#### 删除集合 drop()

```js
var MongoClient = require('mongodb').MongoClient;
var url = "mongodb://localhost:27017/";
 
MongoClient.connect(url, { useNewUrlParser: true }, function(err, db) {
    if (err) throw err;
    var dbo = db.db("grh");
    // 删除 test 集合
    dbo.collection("test").drop(function(err, delOK) {  // 执行成功 delOK 返回 true，否则返回 false
        if (err) throw err;
        if (delOK) console.log("集合已删除");
        db.close();
    });
});
```
