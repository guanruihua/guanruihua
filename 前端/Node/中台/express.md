# Express

## 安装

### 直接安装

`cnpm install express --save`

### 脚手架安装

#### 使用express-generator安装

使用命令行进入项目目录，依次执行：

```
cnpm i express-generator
```

可通过express -h查看命令行的指令含义

```
express -h
```

```
Usage: express [options] [dir]
```

```basic
Options:
    --version        输出版本号
-e, --ejs            添加对 ejs 模板引擎的支持
    --pug            添加对 pug 模板引擎的支持
    --hbs            添加对 handlebars 模板引擎的支持
-H, --hogan          添加对 hogan.js 模板引擎的支持
-v, --view <engine>  添加对视图引擎（view） <engine> 的支持 (ejs|hbs|hjs|jade|pug|twig|vash) （默认是 jade 模板引擎）
    --no-view        创建不带视图引擎的项目
-c, --css <engine>   添加样式表引擎 <engine> 的支持 (less|stylus|compass|sass) （默认是普通的 css 文件）
    --git            添加 .gitignore
-f, --force          强制在非空目录下创建
-h, --help           输出使用方法
```

创建了一个名为 myapp 的 Express 应用，并使用ejs模板引擎

```
express --view=ejs app
```

进入app，并安装依赖

```
cd myapp
npm install
```

**在Windows 下，使用以下命令启Express应用：**

```
set DEBUG=app:* & npm start
```

**在 MacOS 或 Linux 下，使用以下命令启Express应用：**

```
DEBUG=app:* npm start
```

## 第一个Express应用

```js
const express = require('express');     //引入express模块
var app= express();     //express()是express模块顶级函数

app.get('/',function(req,res){      //访问根路径时输出hello world
    res.send(`<h1 style='color: blue'>hello world</h1>`);
});

app.listen(8080);       //设置访问端口号
```

## get请求

一般在网站开发中，get都用作数据获取和查询，类似于数据库中的查询操作，当服务器解析前台资源后即传输相应内容；而查询字符串是在URL上进行的，形如：

```js
http://localhost:8080/login?goods1=0001&goods2=0002
```

### 二、获取前台get请求

通过req.query可以获得用户发送的get请求，之后通过node操作将相应数据返回给用户。

如果发送的是：

```js
http://localhost:8080/login?goods1=0001&goods2=0002
```

响应的话则通过：

```js
req.query
```

他会获取到全部数据，或

```js
req.query.goods1 // 0001
req.query.goods2 // 0002
```

来单独或去每一个数据。总之不同的需求对应不同的业务，大家按自己的需要来获取；

### 三、实例

下面通过一个实例来对获取get参数进行一个总结：

HTML:

```html
<form action="http://localhost:8080/login" method="get">
  用户：
  <input type="text" name="user" id="user" placeholder="用户名"/>
  <br>
  密码：
  <input type="password" name="password" id="password" placeholder="密码"/>
  <br>
  <input type="submit" value="提交"/>
</form>
```

NODE:

```javascript
const express = require("express");
var app = express();

app.get("/",function(req,res){
    res.send("主页");
});

app.get("/login",function(req,res){
    console.log(req.query);
    res.send("登录路由，user为："+req.query.user+"==>   password为："+req.query.password);
});

app.listen(8080);
```

当在html页面中输入用户和密码提交后：

就能得到想要的传输数据； 总之，虽然获取get参数并不复杂，但使用频率却相当的高，对于任何技术我们都应该秉持认真的态度去了解和学习它。

## POST请求

post方法作为http请求很重要的一部分，几乎所有的网站都有用到它，与get不同，post请求更像是在服务器上做修改操作，它一般用于数据资源的更新。 相比于get请求，post所请求的数据会更加安全。上一章中我们发现get请求会在地址栏显示输入的用户名和密码(有中文时会转化为BASE64加密)，而post请求则会将数据放入http包的包体中，这使得别人无法直接看到用户名和密码！

### 二、Express如何设置POST请求

1.我们的知道，首先我们得知道在form表单进行post请求，enctype属性一般设置为“application/x-www-form-urlencoded”，如果设置成multipart/form-data，则多用于文件上传，如下：

```html
<form action="#" method="post" enctype="application/x-www-form-urlencoded">
</form>
```

2设置解析body中间件

```js
app.use(express.urlencoded())
```

3获取body数据

```javascript
req.body.username 
```

登陆案例：

HTML:

```html
<h1>登陆</h1>
<form action="/login" method="POST">
    <div>
        用户名：<input type="text" name="username">
    </div>
    <div>
        密码：<input type="password" name="password">
    </div>
    <button>登陆</button>
</form>
```

APP.JS

```javascript
var express = require('express');
var path = require('path')
var app = express();
var sqlQuery = require('./lcMysql')

// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'ejs');
app.use(express.static(path.join(__dirname, 'public')));
//解析post提交的数据
app.use(express.urlencoded())

//搜索首页
app.get('/',(req,res)=>{
  res.render('index.ejs')
})

//登陆页
app.get('/login',(req,res)=>{
  res.render('login')
})
//处理登陆请求
app.post('/login',async (req,res)=>{
  //获取用户名和密码
  let username = req.body.username 
  let password = req.body.password
  //查询数据库是否由此用户名和密码
  let sqlStr = 'select * from user where username = ? and password = ?';
  let arr = [username,password];
  let result = await sqlQuery(sqlStr,arr)
  if(result.length == 0 ){
    res.send("登陆失败")
  }else{
    res.send("登陆成功")
  }

})



module.exports = app;

```

## 中间件

> 从字面意思，我们可以了解到它大概就是做中间代理操作，事实也是如此；大多数情况下，中间件就是在做接收到请求和发送响应中间的一系列操作。事实上，express是一个路由和中间件的web框架，Express 应用程序基本上是一系列中间件函数的调用。

1. 浏览器发送请求

2.express接受请求

中间处理的过程

3.路由函数处理渲染（req,res）

4.res.render渲染

中间件函数可以执行以下任务：

- 执行任何代码。
- 对请求和响应对象进行更改。
- 结束请求/响应循环。
- 调用堆栈中的下一个中间件函数。

中间件也分为应用层中间件、路由中间件、内置中间件、错误处理中间件和第三方中间件。下面分别对以下进行说明：

### 1.应用层中间件

应用级中间键绑定到app对象使用app.use和app.METHOD()-需要处理http请求的方法，例如GET、PUT、POST，将之前的get或者post替换为use就行。 例如下面实例：

```javascript
const express=require("express");

var app=express();

//匹配路由之前的操作
app.use(function(req,res,next()){
    console.log("访问之前");
});

app.get("/",function(req,res){
    res.send("主页");
});

app.listen(8080);
```

这时我们会发现<http://localhost:8080/地址一直在加载，但命令行里显示了“访问之前”，说明程序并不会同步执行，如果使用next>来是路由继续向下匹配，那么就能又得到主页数据了：

```js
const express=require("express");

var app=express();

//匹配路由之前的操作
app.use(function(req,res,next){
    console.log("访问之前");
    next();
});

app.get("/",function(req,res){
    res.send("主页");
});

app.listen(8080);
```

当然也可以简化写法：

```js
const express=require("express");

var app=express();

app.use(function(req,res,next){
    console.log("访问之前");
    next();
},function(req,res){
    res.send("主页");
});

app.listen(8080);
```

因此，在进行路由匹配之前或再录又要继续向下执行时想做个操作，那么应用层中间件无疑是好的选择。

### 2.路由中间件

路由级中间件和应用级中间件类似，只不过他需要绑定express.Router();

```js
var router = express.Router()
```

在匹配路由时，我们使用 router.use() 或 router.VERB() ,路由中间件结合多次callback可用于用户登录及用户状态检测。

```js
const express = require("express");
var app = express();
var router=express.Router();

router.use("/",function(req,res,next){
    console.log("匹配前");
    next();
});

router.use("/user",function(req,res,next){
    console.log("匹配地址：",req.originalUrl);
    next();
},function(req,res){
    res.send("用户登录");
});

app.use("/",router);

app.listen(8080);
```

总之在检测用户登录和引导用户应该访问哪个页面是，路由中间件绝对好用。

### 3.错误处理中间件

顾名思义，它是指当我们匹配不到路由时所执行的操作。错误处理中间件和其他中间件基本一样，只不过其需要开发者提供4个自变量参数。

```js
app.use((err, req, res, next) => {
        res.sendStatus(err.httpStatusCode).json(err);
});
```

一般情况下，我们把错误处理放在最下面，这样我们即可对错误进行集中处理。

```js
const express=require("express");

var app=express();

app.get("/",function(req,res,next){
    const err=new Error('Not Found');
    res.send("主页");
    next(err);
});

app.use("/user",function(err,req,res,next){
    console.log("用户登录");
    next(err);
},function(req,res,next){
    res.send("用户登录");
    next();
});

app.use(function(req,res){
    res.status(404).send("未找到指定页面");
});

app.listen(8080);
```

### 4.内置中间件

从版本4.x开始，Express不再依赖Content，也就是说Express以前的内置中间件作为单独模块，express.static是Express的唯一内置中间件。

```js
express.static(root, [options]);
```

通过express.static我们可以指定要加载的静态资源。

### 5.第三方中间件

形如之前我们的body-parser，采用引入外部模块的方式来获得更多的应用操作。如后期的cookie和session。

```js
var express = require('express');
var app = express();
var cookieParser = require('cookie-parser');
```

以上就是关于express中间件类型，在实际项目中，中间件都是必不可少的，因此熟悉使用各种中间件会加快项目的开发效率。

## Cookie

### 关于Cookie

在我们关闭一个登录过的网址并重新打开它后，我们的登录信息依然没有丢失；当我们浏览了商品后历史记录里出现了我们点击过的商品；当我们推回到首页后，推荐商品也为我们选出了相似物品；事实上当我们有过此类操作后，浏览器会将我们的操作信息保存到cookie上面。阿进而言之，cookie就是储存在用户本地终端上的数据。

**Cookie的特点**

1. cookie保存在浏览器本地，只要不过期关闭浏览器也会存在。
2. 正常情况下cookie不加密，用户可轻松看到
3. 用户可以删除或者禁用cookie
4. cookie可以被篡改
5. cookie可用于攻击
6. cookie存储量很小，大小一般是4k
7. 发送请求自动带上登录信息

### Cookie的安装及使用

#### 1.安装

```bash
cnpm install cookie-parser --save
```

#### 2.引入

```
const cookieParser=require("cookie-parser"); 
```

#### 3.设置中间件

```
app.use(cookieParser());
```

#### 4.设置cookie

```js
res.cookie("name",'zhangsan',{maxAge: 900000, httpOnly: true});
//res.cookie(名称,值,{配置信息})
```

关于设置cookie的参数说明：

1. domain: 域名  
2. name=value：键值对，可以设置要保存的 Key/Value，注意这里的 name 不能和其他属性项的名字一样
3. Expires： 过期时间（秒），在设置的某个时间点后该 Cookie 就会失效，如 expires=Wednesday, 09-Nov-99 23:12:40 GMT。
4. maxAge： 最大失效时间（毫秒），设置在多少后失效 。
5. secure： 当 secure 值为 true 时，cookie 在 HTTP 中是无效，在 HTTPS 中才有效 。
6. Path： 表示 在那个路由下可以访问到cookie。
7. httpOnly：是微软对 COOKIE 做的扩展。如果在 COOKIE 中设置了“httpOnly”属性，则通过程序（JS 脚本、applet 等）将无法读取到COOKIE 信息，防止 XSS 攻击的产生 。
8. singed：表示是否签名cookie, 设为true 会对这个 cookie 签名，这样就需要用 res.signedCookies 而不是 res.cookies 访问它。被篡改的签名 cookie 会被服务器拒绝，并且 cookie 值会重置为它的原始值。

#### 5.获取cookie

```
req.cookies.name;
```

下面是一个基础实例：

```js
const express=require("express");
const cookieParser=require("cookie-parser");

var app=express();

//设置中间件
app.use(cookieParser());

app.get("/",function(req,res){
 res.send("首页");
});

//设置cookie
app.get("/set",function(req,res){
 res.cookie("userName",'张三',{maxAge: 20000, httpOnly: true});
 res.send("设置cookie成功");
});

//获取cookie
app.get("/get",function(req,res){
 console.log(req.cookies.userName);
 res.send("获取cookie成功，cookie为："+ req.cookies.userName);
});

app.listen(8080);
```

当访问set路由后会设置cookie，当访问get路由后会获取到设置的cookie值。当然你也可以在其他页面继续获取当前cookie，以实现cookie共享。

### 多个二级域名共享cookie

只需要增加res.cookie中option对象的值，即可实现对相应路由下多个二级路由的cookie进行共享，代码如下：

```js
const express=require("express");
const cookieParser=require("cookie-parser");

var app=express();

//设置中间件
app.use(cookieParser());

app.get("/",function(req,res){
 res.send("首页");
});

//设置cookie
app.get("/set",function(req,res){
 res.cookie("userName",'张三',{maxAge: 200000, httpOnly: true,domain: "ccc.com"});
 res.send("设置cookie成功");
});

//获取cookie
app.get("/get",function(req,res){
 console.log(req.cookies.userName);
 res.send("获取cookie成功，cookie为："+ req.cookies.userName);
});

app.listen(8080);
```

我们可以看到

不同的二级域名也能访问到相同的cookie，只要满足ccc.com这个顶级域名就行。

## Cookie加密

cookie加密是让客户端用户无法的获取cookie明文信息，是数据安全的重要部分；一般的我们可以在保存cookie时对cookie信息进行加密，或者在res.cookie中对option对象的signed属性设置设置成true即可。

### 二、使用 signed 属性进行cookie加密

如下列代码：

```js
const express = require("express");
const cookieParser = require("cookie-parser");

var app = express();
app.use(cookieParser('secret'));

app.get("/",function(req,res){
 res.send("主页");
});

//获取cookie
app.use(function(req,res,next){
 console.log(req.signedCookies.name);
 next();
});

//设置cookie
app.use(function(req,res,next){
 console.log(res.cookie("name","zhangsan",{httpOnly: true,maxAge: 200000,signed: true}));
 res.end("cookie为："+req.signedCookies.name);
});

app.listen(8080);
```

**签名原理**
Express用于对cookie签名，而cookie-parser则是实现对签名的解析。实质是把cookie设置的值和cookieParser(‘secret’);中的secret进行hmac加密，之后和cookie值加“.”的方式拼接起来。
当option中signed设置为true后，底层会将cookie的值与“secret”进行hmac加密；

**如何解析**
cookie-parser中间件在解析签名cookie时做了两件事：

1. 将签名cookie对应的原始值提取出来
2. 验证签名cookie是否合法

### 3、直接对cookie值加密

node为我们提供了一个核心安全模块“crypto”，它提供了很多安全相关的功能，如摘要运算、加密、电子签名等。
这是，我们便可很轻易的封装一个加密模块：

```js
const crypto=require('crypto');

module.exports={
 //MD5封装
 MD5_SUFFIX:'s5w84&&d4d473885s2025s5*4s2',
 md5:function(str){
  var obj=crypto.createHash('md5');
  obj.update(str);  
  return obj.digest('hex');
 }
}
```

之后只需要进行相应导入即可

```js
const common=require('./MD5');

var str='123456';
var str=common.md5(str+'s5w84&&d4d473885s2025s5*4s2');
console.log(str);
```

设置cookie代码如下：

```js
const express=require("express");
const cookieParser=require("cookie-parser");
var cry = require('./md5');

var app=express();

var str='hello-123';
var str=cry.md5(str+'s5w84&&d4d473885s2025s5*4s2');

//设置中间件
app.use(cookieParser());

//获取加密cookie
app.use(function(req,res,next){
 console.log(req.cookies.userName);
 next();
});

//设置并加密cookie
app.use(function(req,res,next){
 res.cookie("userName", str, {maxAge: 5*60*1000, httpOnly: true});
 res.end("set ok");
});

app.listen(8080);
```

如果是在判断登录时，只需将用户输入的账号进行同样加密操作在进行比较即可知道账户是否正确。
crypto所涉及的加密方式有很多，推荐大家都写模块引用，这样更方便后期的维护。

## session

### 一、关于session

session是另一种记录客户状态的机制，与cookie保存在客户端浏览器不同，session保存在服务器当中；
当客户端访问服务器时，服务器会生成一个session对象，对象中保存的是key:value值，同时服务器会将key传回给客户端的cookie当中；当用户第二次访问服务器时，就会把cookie当中的key传回到服务器中，最后服务器会吧value值返回给客户端。
因此上面的key则是全局唯一的标识，客户端和服务端依靠这个全局唯一的标识来访问会话信息数据。

### 二、设置session

我们使用express-session模块来设置session

##### 1.安装express-session

```
cnpm install express-session --save
```

##### 2.引入express-session模块

```
const session=require("express-session");
```

##### 3.设置session

```
session(options);
```

如下列代码：

```js
const express=require("express");
const session=require("express-session");

var app=express();

//配置中间件
app.use(session({
 secret: "keyboard cat",
  resave: false,
  saveUninitialized: true,
  cookie: ('name', 'value',{maxAge:  5*60*1000,secure: false})
}));

app.use('/login',function(req,res){
 //设置session
 req.session.userinfo='张三';
 res.send("登陆成功！");
});

app.use('/',function(req,res){
 //获取session
 if(req.session.userinfo){
  res.send("hello "+req.session.userinfo+"，welcome");
 }else{
  res.send("未登陆");
 }
});

app.listen(8080);
```

在session(option)中对session进行设置

### 三、session的常用方法

```js
//设置session
req.session.username="张三"

//获取session
req.session.username

//重新设置cookie的过期时间
req.session.cookie.maxAge=1000;

//销毁session
req.session.destroy(function(err){
 
})
```

以下演示通过销毁session的方式来退出登录

```js
const express=require("express");
const session=require("express-session");

var app=express();

//配置中间件
app.use(session({
 secret: "keyboard cat",
  resave: false,
  saveUninitialized: true,
  cookie: ('name', 'value',{ maxAge:  5*60*1000,
        secure: false,
        name: "seName",
        resave: false})
}));

app.use('/login',function(req,res){
 //设置session
 req.session.userinfo='张三';
 res.send("登陆成功！");
});

app.use('/loginOut',function(req,res){
 //注销session
 req.session.destroy(function(err){
  res.send("退出登录！"+err);
 });
});

app.use('/',function(req,res){
 //获取session
 if(req.session.userinfo){
  res.send("hello "+req.session.userinfo+"，welcome to index");
 }else{
  res.send("未登陆");
 }
});

app.listen(8080);
```

当我们进入到主页时，未显示任何信息，进入login路由后，自动设置session，这是回到主页则显示session信息，之后进入loginOut路由已注销session信息，再回到首页显示为登陆。

## sobooks登陆功能

### 1-引入session和cookie相关模块

```js
var cookieParser = require('cookie-parser');
//引入session模块
let session = require('express-session');
```

### 2-引入session

```js
app.use(session({
  secret: "xzsagjasoigjasoi",
  resave:true,//强制保存session
  cookie:{
    maxAge:7*24*60*60*1000,//设置session的有效期为1周
  },
  saveUninitialized:true//是否保存初始化的session
}))
```

### 3-引入cookie中间件

```json
app.use(cookieParser('secret'));
```

### 4-写判断是否登陆的中间件

```js
function isLoginMid(req,res,next){
    if(req.session.username==undefined){
        res.render('info',{
            title:"未登录",
            content:"尚未登陆，请进入登陆页面登陆",
            href:"/login",
            hrefTxt:"登录页"
        })
    }else{
        //一登录进入正常页面
        next()
    }
}
```

### 5-引入一个跳转的模板信息页面

可以显示登陆成功或者失败的信息内容，并且可以在一定时间内进行跳转。

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title><%-title%></title>
</head>
<body>
    <h1><%-title%></h1>
    <h3><%-content%></h3>
    <p><span class="num">5</span>秒后跳转至：<a href="<%-href%>"><%-hrefTxt%></a></p>
    <script>
        n = 5;
        setInterval(()=>{
            n--;
            if(n<0){
                location.href = '<%-href%>';
            }else{
                document.querySelector('.num').innerHTML = n;
            }
        },1000)
    </script>
</body>
</html>
```

### 6-登陆页面

```javascript
<form action="/login" method="POST">
    <div class="form-group">
        <input class="form-control" type="email" name="mail" placeholder="邮箱" required="required"/>
            </div>
<div class="form-group">
    <input class="form-control" type="password" name="password" placeholder="密码" required="required"/>
        </div>
<p><a href="#">忘记密码?</a></p>
    <button class="btn btn-lg">登录</button>
</form>
```

### 7-处理POST方式提交的请求

1-获取表单提交的数据

2-查询表单提交的账号密码是否正确

3-如果正确，设置session,req.session.username = user.username;

4-显示登陆是否成功信息

```js
router.post('/',async function(req,res){
    console.log(req.body)
    //根据提交的邮箱和密码判断是否是正确的账号密码
    let strSql = "select * from user where mail=? and password = ?"
    let arr = [req.body.mail,req.body.password]
    let result = await sqlQuery(strSql,arr)
    if(result.length!=0){
        //登陆成功
        user = result[0];
        req.session.username = user.username;
        res.render('info',{
            title:"登陆成功",
            content:"账号密码正确，即将进入首页",
            href:"/",
            hrefTxt:"首页"
        })
    }else{
        res.render('info',{
            title:"登陆失败",
            content:"账号或密码不正确，即将进入登录页",
            href:"/login",
            hrefTxt:"登录页"
        })
    }
})
```

## sobook注册

### 1-注册页

1-设置表单

```HTML
<form action="/register" method="POST">
    <div class="form-group">
        <input class="form-control" type="email" name="mail" placeholder="邮箱" required="required"/>
    </div>
    <div class="form-group">
        <input class="form-control" type="text" name="username" placeholder="用户名" required="required"/>
    </div>
    <div class="form-group">
        <input class="form-control" type="password" name="password" placeholder="密码" required="required"/>
    </div>
    <div class="form-group">
        <input class="form-control" type="password" name="repassword" placeholder="再次输入密码" required="required"/>
    </div>

    <p><a href="/login">登陆</a></p>
    <button id="registerBtn" disabled="disable" style="cursor: not-allowed;" class="btn btn-lg">注册</button>
</form>
```

2-前端校验表单数据

```javascript
var formDiv = document.querySelector('form');
 var inputs =  document.querySelectorAll('form input');
 var btn = document.querySelector('#registerBtn');
 formDiv.oninput = function(){
  //判断是否有内容为空
  isAble = true;
  inputs.forEach((item,i)=>{
   if(item.value==""){
    isAble = false;
   }
  })
  //正则匹配邮箱地址
  let reg = /^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/
  //密码与再次输入的密码是否匹配
  if(inputs[2].value==inputs[3].value&&isAble&&reg.test(inputs[0].value)){
   btn.disabled = false;
   btn.style.cursor = 'pointer'
  }else{
   btn.disabled = true;
   btn.style.cursor = 'not-allow'
  }
 }
```

### 2-注册页的路由

1-正常浏览器GET请求的路由

```
router.get('/', function(req, res, next) {
  res.render('register')
});
```

2-表单提交的POST请求路由，先判断是否已注册，没有注册即将数据插入到数据库

```javascript
router.post('/',async function(req,res){
    //获取表单提交的邮箱，密码，用户名
    console.log(req.body)
    let mail = req.body.mail;
    let password = jiami(req.body.password);
    let username = req.body.username;
    //判断邮箱是否已注册，如果已注册，将不在注册；
    let strSql = "select * from user where mail=?"
    let result = await sqlQuery(strSql,[mail])
    if(result.length!=0){
        //邮箱已注册
        res.render('info',{
            title:"注册失败",
            content:"此邮箱已注册过，可直接登陆，或找寻密码",
            href:"/register",
            hrefTxt:"注册页"
        })
        
    }else{
       //此邮箱尚未注册，可注册
       strSql = "insert into user (mail,username,password) values (?,?,?)"
       await sqlQuery(strSql,[mail,username,password])
       res.render('info',{
        title:"注册成功",
        content:"注册成功请登陆，即将进入登陆页面",
        href:"/login",
        hrefTxt:"登录页"
    })
    }
})
```

### 3-加密密码并保存至数据库

加密：

```
function jiami(str){
    let salt = "fjdsoigijasoigjasdiodgjasdiogjoasid"
    let obj = crypto.createHash('md5')
    str = salt+str;
    obj.update(str)
    return obj.digest('hex')
}
```

### 4-修改登陆也为加密操作

```javascript
function jiami(str){
    let salt = "fjdsoigijasoigjasdiodgjasdiogjoasid"
    let obj = crypto.createHash('md5')
    str = salt+str;
    obj.update(str)
    return obj.digest('hex')
}

router.post('/',async function(req,res){
    console.log(req.body)
    //根据提交的邮箱和密码判断是否是正确的账号密码
    let strSql = "select * from user where mail=? and password = ?"
    let arr = [req.body.mail,jiami(req.body.password)]
    let result = await sqlQuery(strSql,arr)
    if(result.length!=0){
        //登陆成功
        user = result[0];
        req.session.username = user.username;
        res.render('info',{
            title:"登陆成功",
            content:"账号密码正确，即将进入首页",
            href:"/",
            hrefTxt:"首页"
        })
    }else{
        res.render('info',{
            title:"登陆失败",
            content:"账号或密码不正确，即将进入登录页",
            href:"/login",
            hrefTxt:"登录页"
        })
    }
    
})
```

## 文件上传

### 一、multer中间件

再上传文件时，我们通常会使用到他。Multer用于处理multipart/form-data 类型的表单数据。首先我们先安装它：

```
cnpm install multer --save
```

### 二、使用

首先在form表单中我们需要设置enctype为：multipart/form-data表单类型。同时我们也需要用到fs模块对文件重命名。下面是单文件上传实例：

```
<!DOCTYPE html>
<html>
 <head>
  <meta charset="utf-8">
  <title></title>
 </head>
 <body>
  <form action="http://localhost:8080/" method="post" enctype="multipart/form-data">
   <input type="file" name="files" value="指定文件">
   <br><br>
   <input type="submit" value="上传">
  </form>
 </body>
</html>
```

NODE代码：

```
const express=require("express");
const multer=require('multer');
//初始化上传对象
var upload=multer({dest:'./upload/'});
var fs = require('fs');


var app=express();

app.use("/",upload.single("files"),function(req,res){ //files为input type="file"的name值
 var oldFile=req.file.destination+req.file.filename; //指定旧文件
 var newFile=req.file.destination + req.file.originalname; //指定新文件
 fs.rename(oldFile,newFile,function(err){
  if(err){
   res.send('上传失败！');
  }else{
   res.send('上传成功！');
  }
 });
});

app.listen(8080);
```

在这里我们可以指定单个文件上传到根目录的upload文件夹里。这里值得注意的是req.file会返回文件的基本信息：

```
  fieldname: ***, //input type="file"的name值
  originalname: ***, //用户计算机上的文件的名称
  encoding: '***',  //文件编码
  mimetype: ***',  //文件的 MIME 类型
  destination: './***/',  //保存路径
  filename: ***,  //保存在 destination 中的文件名
  path: ***,  //已上传文件的完整路径
  size: **  //文件大小（字节单位）
```

### 三、上传多个文件

在HTML找中input type="file"需要加上multiple来实现过滤，multiple不写参数则可以读取·所有文件。而在服务端上，我们需要将single()改为array(“name”,num);的形式来接收多个文件的上传请求。最后对他们全部进行重命名。在这之前我们首先看看multer支持哪些文件上传方式：

```
.single(fieldname) //接受一个以 fieldname 命名的文件。.fields(fields)
.array(fieldname[, maxCount]) //接受一个以 fieldname 命名的文件数组。可以配置 maxCount 来限制上传的最大数量。
.fields(fields) //接受指定 fields 的混合文件。fields是一个拥有name和maxCount的数组对象。
.none()  //只接受文本域。如果任何文件上传到这个模式，将发生 "LIMIT_UNEXPECTED_FILE" 错误。
.any() //接受一切上传的文件。
```

下面我们将会演示如何上传多个文件：
html：

```
<!DOCTYPE html>
<html>
 <head>
  <meta charset="utf-8">
  <title></title>
 </head>
 <body>
  <form action="http://localhost:8080/" method="post" enctype="multipart/form-data">
   <input type="file" name="files" value="指定文件" multiple>
   <br><br>
   <input type="submit" value="上传">
  </form>
 </body>
</html>
```

node代码：

```
const express=require("express");
const multer=require('multer');
var upload=multer({dest:'./upload/'});
var fs = require('fs');


var app=express();

app.use("/",upload.array("files",5),function(req,res,next){
 req.files.forEach(function(ele,index){
  console.log(ele);
  var oldFile=ele.destination+ele.filename; //指定旧文件
  var newFile=ele.destination+ele.originalname; //指定新文件
  fs.rename(oldFile,newFile,function(err){
   err?console.log('上传失败！'):console.log('上传成功！');
  });
 });
 res.send("成功上传");
});

app.listen(8080);
```

这里，我们获取文件信息是通过req.files来获取，他是由数组构成的对象，之后用foreach循环对其进行重命名即可。

### 四、通过limits来限制上传文件

Multer通过使用limits这个对象来对数据进行限制，它允许使用以下参数：

```
Key Description Default
fieldNameSize field 名字最大长度 100 bytes
fieldSize field 值的最大长度 1MB
fields 非文件 field 的最大数量 无限
fileSize 在 multipart 表单中，文件最大长度 (字节单位) 无限
files 在 multipart 表单中，文件最大数量 无限
parts 在 multipart 表单中，part 传输的最大数量(fields + files) 无限
headerPairs 在 multipart 表单中，键值对最大组数 2000
如果你上传的文件超出这些设定，MulterError模块将会启用，该模块在node_modules/multer/lib/multer-error.js上：
```

我们可以使用err.code定位到该错误，他有7种code方式，不同设置会返回不同code;

```
LIMIT_PART_COUNT
LIMIT_FILE_SIZE
LIMIT_FILE_COUNT
LIMIT_FIELD_KEY
LIMIT_FIELD_VALUE
LIMIT_FIELD_COUNT
LIMIT_FIELD_COUNT
```

下面就给大家做个简单实例：
html依然不变，js代码如下：

```
const express=require("express");
const multer=require('multer');
var upload=multer({dest:'./upload/',limits:{fileSize: 1024 * 1024 * 20,files: 5}});
var fs = require('fs');

var app=express();

app.use("/",upload.array("files",5),function(req,res,next){
 req.files.forEach(function(ele,index){
  var oldFile=ele.destination+ele.filename; //指定旧文件
  var newFile=ele.destination+ele.originalname; //指定新文件
  fs.rename(oldFile,newFile,function(err){
   err?console.log('上传失败！'):console.log('上传成功！');
  });
 });
 next();
 res.send("上传成功！");
});

app.use(function(err,req,res,next){
 if (err.code==='LIMIT_FILE_SIZE'){
  res.send('File is too large');
 }else if(err.code==='LIMIT_FILE_COUNT'){
  res.send('Too many files');
 }
})

app.listen(8080);
```

## 文件下载

### 文件下载

文件下载非常简单，仅需通过res.download()执行即可，他可以写为3种形式：

```
res.download('/report-12345.pdf');
```

以下是一个对选择对应文件进行下载的实例：
html：

```
<!DOCTYPE html>
<html>
 <head>
  <meta charset="utf-8">
  <title></title>
 </head>
 <body>
  <form action="http://localhost:8080/" method="post" enctype="application/x-www-form-urlencoded">
   <input type="file" name="files" value="选择下载的文件"><br><br>
   <input type="submit" value="下载">
  </form>
 </body>
</html>
```

js：

```
const express=require("express");
const bodyParser=require("body-parser");

var app=express();

var jsonParser = bodyParser.json();
var urlencodedParser = bodyParser.urlencoded({ extended: false });

app.post('/',urlencodedParser,function(req,res){
 res.download("./public/"+req.body.files,err=>{
  if(err){
   res.send("下载失败！");
  }else{
   console.log("下载成功！");
  }
 });
});

app.listen(8080);
```

我们可以选择根目录public下的文件对其进行下载。

## AJAX上传图片

### 图片上传实现步骤

#### 图片上传

通过jquery监听input change事件，这样我们可以获取到上传的图片流信息，从而可以获取到图片的地址、大小、格式以及名称等信息

这里创建3个数组，imgName、imgSrc、imgFile分别用于存放上传图片的名称、url地址以及图片流信息

```
var fileList = this.files;
        for(var i = 0; i < fileList.length; i++) {
            var imgSrcI = getObjectURL(fileList[i]);
            imgName.push(fileList[i].name);
            imgSrc.push(imgSrcI);
            imgFile.push(fileList[i]);
        }
```

getObjectURL方法是一个用于获取本地图片的地址，使用该url可以显示图片

```
function getObjectURL(file) {
    var url = null ;
    if (window.createObjectURL!=undefined) { // basic
        url = window.createObjectURL(file) ;
    } else if (window.URL!=undefined) { // mozilla(firefox)
        url = window.URL.createObjectURL(file) ;
    } else if (window.webkitURL!=undefined) { // webkit or chrome
        url = window.webkitURL.createObjectURL(file) ;
    }
    return url ;
}
```

#### 控制上传图片大小、格式以及上传数量

```
    $('#upload').on('change',function(){        
          if(imgSrc.length==4){
            return alert("最多只能上传4张图片");
        }
        var imgSize = this.files[0].size;  //b
        if(imgSize>1024*1024*1){//1M
            return alert("上传图片不能超过1M");
        }
        if(this.files[0].type != 'image/png' && this.files[0].type != 'image/jpeg' && this.files[0].type != 'image/gif'){
            return alert("图片上传格式不正确");
        }
    })
```

#### 图片预览

创建一个addNewContent方法用于动态展示添加的图片实现图片预览，在每次上传图片的时候调用该方法

```
function addNewContent(obj) {
    $(obj).html("");
    for(var a = 0; a < imgSrc.length; a++) {
        var oldBox = $(obj).html();
        $(obj).html(oldBox + '<li class="content-img-list-item"><img src="'+imgSrc[a]+'" alt=""><a index="'+a+'" class="hide delete-btn"><i class="ico-delete"></i></a></li>');
    }
}
```

#### 图片删除

1.通过监听鼠标的mouseover事件，显示图片删除按钮

```
$('.content-img-list').on('mouseover','.content-img-list-item',function(){
        $(this).children('a').removeClass('hide');
    });
```

2.监听鼠标的mouseleave事件，隐藏图片删除按钮

```
$('.content-img-list').on('mouseleave','.content-img-list-item',function(){
        $(this).children('a').addClass('hide');
    });
```

3.获取图片index下标属性，通过js的splice方法删除数组元素，重新调用addNewContent方法遍历图片数组显示预览图片

```
$(".content-img-list").on("click",'.content-img-list-item a',function(){
            var index = $(this).attr("index");
            imgSrc.splice(index, 1);
            imgFile.splice(index, 1);
            imgName.splice(index, 1);
            var boxId = ".content-img-list";
            addNewContent(boxId);
            if(imgSrc.length<4){//显示上传按钮
                $('.content-img .file').show();
            }
      });
```

#### 图片上传提交

这里主要使用FormData来拼装好数据参数，提交到后台

```
var formFile = new FormData();
```

遍历imgFile图片流数组拼装到FormData中

```
 $.each(imgFile, function(i, file){
            formFile.append('myFile[]', file);
        });
```

添加其他参数

```
    formFile.append("type", type); 
        formFile.append("content", content); 
        formFile.append("mobile", mobile); 
```

最后使用ajax提交内容

```
 $.ajax({
            url: 'http://zhangykwww.yind123.com/webapi/feedback',
            type: 'POST',
            data: formFile,
            async: true,  
            cache: false,  
            contentType: false, 
            processData: false, 
            // traditional:true,
            dataType:'json',
            success: function(res) {
                console.log(res);
            }
        })
```

以上就实现了图片上传、图片预览和图片删除的功能

jquery设置 ajax属性

```
processData : false, // 告诉jQuery不要去处理发送的数据
contentType : false,// 告诉jQuery不要去设置Content-Type请求头
```
