# express

## 简单使用

`npm i`

npm i express

`npm i nodemon`

```js
//server.js
var express = require('express');
var app = express();

app.get('/',function(req, res){
    res.send('this is the homepage');
})

app.listen(3000);
```

运行 `nodemon server.js`

浏览器打开`http://localhost:3000/`

## id的使用

```js
var express = require('express');
var app = express();
app.get('/user/:id/and/:name',function(req,res){
    console.log(req.params);
    res.send(req.params.id+"  "+req.params.name);
  //可以输出id和name的值
})
app.listen(3000);
```

## url中使用正则

```js
var express = require('express');
var app = express();
//这里的?就是正则表达式
app.get('/user/ab?cd',function(req,res){
    console.log(req.params);
    res.send("匹配成功");
})
app.listen(3000);
//输入http://localhost:3000/acd也可以匹配成功
```

## 查询字符串

```js
//server.js
var express = require('express');
var app = express();

app.get('/',function(req, res){
   console.log(req.query);
    res.send('this is a epage'+ req.query.user);
   //req.query.user可以拿到值
})
app.listen(3000);
```

> `http://localhost:3000/?user=grh` req.query可以返回{user:'grh'}

## 使用body-parser中间件

> `npm install body-parser --save`
>
> [官网](https://www.expressjs.com.cn/en/resources/middleware/body-parser.html)

### Express/Connect top-level generic

> 这个示例演示了添加一个通用的JSON和URL编码的解析器作为顶级中间件，它将解析所有传入请求的主体。 这是最简单的设置。

```javascript
var express = require('express')
var bodyParser = require('body-parser')

var app = express()

// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))

// parse application/json
app.use(bodyParser.json())

app.use(function (req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  res.end(JSON.stringify(req.body, null, 2))
})
```

### Express route-specific

> 此示例演示了将主体解析器专门添加到需要它们的路由。通常，这是与Express结合使用body解析器的最推荐方法。

```javascript
var express = require('express')
var bodyParser = require('body-parser')

var app = express()

// create application/json parser
var jsonParser = bodyParser.json()

// create application/x-www-form-urlencoded parser
var urlencodedParser = bodyParser.urlencoded({ extended: false })

// POST /login gets urlencoded bodies
app.post('/login', urlencodedParser, function (req, res) {
  res.send('welcome, ' + req.body.username)
})

// POST /api/users gets JSON bodies
app.post('/api/users', jsonParser, function (req, res) {
  // create user in req.body
})
```

### Change accepted type for parsers

> 所有解析器都接受一个“类型”选项，该选项允许您更改中间件将解析的“内容类型”。

```javascript
var express = require('express')
var bodyParser = require('body-parser')

var app = express()

// parse various different custom JSON types as JSON
app.use(bodyParser.json({ type: 'application/*+json' }))

// parse some custom thing into a Buffer
app.use(bodyParser.raw({ type: 'application/vnd.custom-type' }))

// parse an HTML body into a string
app.use(bodyParser.text({ type: 'text/html' }))
```

## HTML 5 `<form>` enctype 属性

| 值                                | 描述                                                         |
| :-------------------------------- | :----------------------------------------------------------- |
| application/x-www-form-urlencoded | 在发送前对所有字符进行编码（默认）。                         |
| multipart/form-data               | 不对字符编码。当使用有==文件上传==控件的表单时，该值是必需的。 |
| text/plain                        | 将空格转换为 "+" 符号，但不编码特殊字符。                    |

## req.params,req.query,req.body的差别

|                   | 对应的url                              |
| ----------------- | -------------------------------------- |
| `req.params`      | `http://localhost:3000/10`             |
| `req.query["id"]` | `http://localhost:3000/?id=10`         |
| `req.body.id`     | `http://localhost:3000`一般不在url显示 |

## 上传文件使用multer

Multer 是一个 node.js 中间件，用于处理 `multipart/form-data` 类型的表单数据，它主要用于上传文件。它是写在 [busboy](https://github.com/mscdex/busboy) 之上非常高效。

**注意**: Multer 不会处理任何非 `multipart/form-data` 类型的表单数据。

server.js

```javascript
var express = require('express');
var app = express();

//上传文件
app.post('/form_file', upload.single('logo') , function(req,res,next){
    // var form = fs.readFileSync('./form.html', { encoding: "utf8"});
    // res.send(form);
    console.log(req.file);
    res.send({'ret_code': 0 });
})

// 通过http://localhost:3000/form 打开formhtml
app.get('/form', function(req,res){
    // 方法一
    // var form = fs.readFileSync('./form.html', {encoding:"utf8"});
    // res.send(form);
    // 方法二
    res.sendFile(__dirname + '/form.html');
})

app.listen(3000);
```

form.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Comptatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
   <!--这里的action填/form_file也是可以的-->
    <form action="http://localhost:3000/form_file" method="post" enctype="multipart/form-data">
        <h2>单图上传</h2>
        <input type="file" name="logo">
        <input type="submit" value="提交">
    </form>
</body>
</html>
```

## 使用模板引擎ejs

### 安装`npm install ejs --save`

```js
var express = require('express');
var app = express();

app.set('view engine', 'ejs');
app.get('/ejs/:name',function(req,res){
    var myname = req.params.name;
  //这里也可以传输对象等数据类型
    res.render('ejs_text',{pn: myname });
})//这里的ejs_text是指向iview的文件

app.listen(3000);
```

ejs_text.ejs

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>
        <%= pn %>
    </h1>
</body>
</html>
```

> 通过`localhost:3000/ejs/grh`访问

## 中间件

server.js

```javaScript
var express = require('express');
var app = express();

app.use(function(req, res, next){
  console.log('first middleware');
  next();//可以接着运行下一个
})

app.use(function(req, res, next){
  console.log('second middleware');
  res.send('ok');
})

app.listen(3000);
```

## Express托管静态文件

`express.static(root, [options])`

```javascript
app.use(express.static('public'))
```

现在，你就可以访问 `public` 目录中的所有文件了：

```plain-text
http://localhost:3000/images/kitten.jpg
http://localhost:3000/css/style.css
http://localhost:3000/js/app.js
http://localhost:3000/images/bg.png
http://localhost:3000/hello.html
```

`app.use('/aset',express.static('public'))`

通过`http://localhost:3000/aset/images/kitten.jpg`来访问

## 路由中间件

### 原server.js

```javascript
var express = require('express')
var app = express();
app.get('/home', function(req, res, next){
  res.send('home');
})

app.get('/users', function(req, res, next){
  res.send('users');
})

app.get('/', function(req, res, next){
  res.send('root');
})
app.listen(3000);
```

### 使用路由中间件后

> 1. 先新建routes文件夹
>
> 2. 然后再在这文件夹下,建立模块index.js,users,js
> 3. 再在server.js引入这两个路由

index.js

```javascript
var express = require('express');
var router =  express.Router();
router.get('/', function(req, res, next){
  res.send('root');
})

module.exports = router;
```

users.js

```javascript
var express = require('express');
var router =  express.Router();
router.get('/', function(req, res, next){
  res.send('users');
})

module.exports = router;
```

server.js

```javascript
var express =  require('express')
var app  = express();
var indexRouter = require('./routes/index')
var userRouter = require('./route/users')

app.use('/', indexRouter);
app.use('/users', usersRouter);


app.listen(3000);
```
