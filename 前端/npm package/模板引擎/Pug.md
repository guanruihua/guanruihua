---
title: pug
date: 2020-09-10 20:51:24
tags: 
	- pug
	- npm
	- node
	- html
---



# Pug

> - 安装 `npm install pug`
> - 全局安装pug命令行工具 `npm isstall pug-cli -g`
> - 执行编译 `index.pug ` : `pug index.pug`(默认是压缩版)
>   - `pug index.pug -P`:标准版的html
>   - `pug index.pug -o a` 将index.html输入到a目录下
>   - 批量编译: `pug 文件夹名`: 会编译当前文件夹下全部pug文件
> - 概要: `pug.compile()`会把Pug代码编译成一个js函数, 该函数可以传入数据(局部变量, locals);
>   - 传入响应的数据, 会返回数据渲染的HTML字符串
> - [pug官方文档](https://www.pugjs.cn/api/express.html)

```js
// template.pug
p #{name}的 Pug 代码!

// js代码
const pug = require('pug');
// 编译这份代码
const compiledFunction = pug.compileFile('template.pug');
// 渲染一组数据
console.log(compiledFunction({
  name: '李莉'
}));
// "<p>李莉的 Pug 代码！</p>"
// 渲染另外一组数据
console.log(compiledFunction({
  name: '张伟'
}));
// "<p>张伟的 Pug 代码！</p>"

// 编译并使用一组数据渲染 template.pug
console.log(pug.renderFile('template.pug', {
  name: 'Timothy'
}));
// "<p>Timothy 的 Pug 代码！</p>"
```

## 基础语法

### 属性Attribute

| Pug代码                                                      | html                                                         | 描述                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ | --------------------- |
| a(href='baidu.com') 百度                                     | `<a href="baidu.com">百度</a>`                               |                       |
| a(class='button' href='baidu.com') 百度                      | `<a class="button" href="baidu.com">百度</a>`                |                       |
| a(class='button', href='baidu.com') 百度                     | `<a class="button" href="baidu.com">百度</a>`                | 可以加入,号作为分隔符 |
| a(style={color:'red'})                                       | `<a style="color: red;"></a>`                                |                       |
| - var authenticated = true<br/>body(class=authenticated ? 'authed' : 'anon') | `<body class="authed"></body>`                               |                       |
| input(<br/>  type='checkbox'<br/>  name='agreement'<br/>  checked<br/>) | `<input type="checkbox" name="agreement" checked="checked" />` | 多行属性              |
| div(class='div-class', (click)='play()')<br/>div(class='div-class' '(click)'='play()') | `<div class="div-class" (click)="play()"></div>`<br/>`<div class="div-class" (click)="play()"></div>` | 特殊字符              |
| div(escaped="<code>">)<br>div(unescaped!="<code>")           | `<div escaped="&lt;code&gt;"></div>`<br>`<div unescaped="<code>"></div>` | 转义属性优先使用!=    |

### 类和ID

```pug
a.button
.content
="\n"
a#main-link
#content
```

```html
<a class="button"></a>
<div class="content"></div>
<a id="main"></a>
<div id="content"></div>
```



### 结构语法

#### 树状

```pug
ul
	li Item A
	li Item B
	li Item C
```

```html
<ul>
  <li>Item A</li>
  <li>Item B</li>
  <li>Item C</li>
</ul>
```

#### 内联

> - pug: `a:img`
> - html: `<a><img/></a>`

#### 自闭合

```js
img
input
img/
input/
```

```html
<img/>
<input/><img/>
<input/>
```

### 标签

#### DOCTYPE

| PUG                                                   | HTML                                                       |
| ----------------------------------------------------- | ---------------------------------------------------------- |
| doctype html                                          | `<!DOCTYPE html>`                                          |
| doctype html PUBLIC "-//W3C//DTD XHTML Basic 1.1//EN" | `<!DOCTYPE HTML PUBLIC "-//W3C//DTD XHTML Basic 1.1//EN">` |

#### 标签嵌入

> -  写法`#[标签名(标签属性) 标签内容]`

```pug
p.
  这是一个很长很长而且还很无聊的段落，还没有结束，是的，非常非常地长。
  突然出现了一个 #[strong 充满力量感的单词]，这确实让人难以 #[em 忽视]。
```

```html
<p>
   这是一个很长很长而且还很无聊的段落，还没有结束，是的，非常非常地长。
  突然出现了一个 <strong> 充满力量感的单词</strong>，这确实让人难以 <em> 忽视</em>。
</p>
```



### 内容

#### 管道文本

> - 每行前面添加`|`字符

```pug
| 纯文本当然也可以包括 <strong>HTML</strong> 内容。
p
  | 但它必须单独起一行。
```

```html
纯文本当然也可以包括 <strong>HTML</strong> 内容。
<p>但它必须单独起一行。</p>
```

#### 标签内文本

> - 一个空格
>
> - ```pug
>   p 纯文本当然也可以包括 <strong>HTML</strong> 内容。
>   ```
>
> - ```html
>   <p>纯文本当然也可以包括 <strong>HTML</strong> 内容。</p>
>   ```

#### 嵌入大文本

> - 标签后接一个`.`
> - 日常嵌入脚本或样式

```pug
script.
   console.log('using Pug')
```

```html
<script>
  console.log('using Pug')
</script>
```

### 注释

#### 单行注释

```pug
// 注释
```



#### 不输出注释

```pug
//- 不输出的代码
```



#### 块注释

```pug
body
	//
		块状注释
```



#### 条件注释

```pug
<!--[if IE 8]>
<html lang="en" class="lt-ie9">
<![endif]-->
<!--[if gt IE 8]><!-->
<html lang="en">
<!--<![endif]-->
```

```html
<!--[if IE 8]>
<html lang="en" class="lt-ie9">
<![endif]-->
<!--[if gt IE 8]><!-->
<html lang="en">
<!--<![endif]-->
```



## 逻辑语法

```pug
- for (var x=0;x<3:x++)
	li item
```
```html
	<li>item</li>
	<li>item</li>
	<li>item</li>
```

### 变量

### 内容变量

```pug
- var title = "On Dogs: Man's Best Friend";
- var author = "enlore";
- var theGreat = "<span>转义!</span>";
- var msg = "not my inside voice";
- var url = 'pug-test.html';
- var btnType = 'info'
- var btnSize = 'lg'

button(type='button' class='btn btn-' + btnType + ' btn-' + btnSize)
= '\n'
button(type='button' class=`btn btn-${btnType} btn-${btnSize}`)//模板字符串
a(href='/' + url) 链接
p This is #{msg.toUpperCase()}
h1= title
p #{author} 笔下源于真情的创作。
p 这是安全的：#{theGreat}

div#foo(data-bar="foo")&attributes({'data-foo': 'bar'}) //<div id="foo" data-bar="foo" data-foo="bar"></div>
```

### 条件

#### if-else

```pug
- var user = { description: 'foo bar baz' }
- var authorised = false
#user
  if user.description
    h2.green 描述
    p.description= user.description
  else if authorised
    h2.blue 描述
    p.description.
      用户没有添加描述。
      不写点什么吗……
  else
    h2.red 描述
    p.description 用户没有描述
```

```html
<div id="user">
  <h2 class="green">描述</h2>
  <p class="description">foo bar baz</p>
</div>
```

#### switch

```pug
- var friends = 10
case friends
  when 0
    p break
  when 1
    p 您有一个朋友
  default
    p 您有 #{friends} 个朋友
```

### 循环

> - 目前支持each 和while
#### each

```pug
ul
	each val,index in [1, 2, 3, 4, 5]
	 li= val +  ':' + index
	 
ul
  each val, index in {1:'一',2:'二',3:'三'}
    li= index + ': ' + val

- var values = [];
ul
  each val in values.length ? values : ['没有内容']
    li= val


//当values为空时, 就会执行else
- var values = [];
ul
  each val in values
    li= val
  else
    li 没有内容
```

#### while

```pug
- var n = 0;
ul
  while n < 4
    li= n++
```



### 混入mixin

> - `+link(class='btn')`  等价于 `+link()(class="btn')`

```
//- 定义
mixin list(name)
  ul
  	 li= name
    li foo
    li bar
    li baz
//- 使用
+list
+list
	p 这是文章
+list('grh')

//生成两个ul列表
```

## 文件包含

> - include

```pug
//- index.pug
doctype html
html
  head
    style
      include style.css
  body
    h1 我的网站
    p 欢迎来到我这简陋得不能再简陋的网站。
    script
      include script.js
```

## 文件继承

> - block , extends

layout.pug

```pug
html
  head
　　 meta(charset="UTF-8")
    title 我的站点 - #{title}
    block scripts
      script(src='/jquery.js')
  body
    block content
    block foot
      #footer
        p 一些页脚的内容
```

pet.pug

```pug
p= petName
```



page-a.pug

```pug
extends layout.pug

block scripts
  script(src='/jquery.js')
  script(src='/pets.js')

block content
  h1= title
  - var pets = ['猫', '狗']
  each petName in pets
    include pet.pug
```

### 继承拓展

> - prepend 在前面添加head
> - append 在后面添加footer
> - block关键词可以省略

page-b.pug

```pug
extends layout.pug

block prepend head
  script(src='/vendor/three.js')

block append footer
  script(src='/game.js')
```

## 模板

index.pug

```pug
doctype html
html
    head
        meta(charset="UTF-8")
        title= documentTitle
        each val in srcStyles
            link(href= baseStyle +'/' + val)
    body
        header.hd
            nav.hd-navbar.m-navbar.m-navbar_primary
                .hd-navbar-tel 联系方式: #{tel}
            ul.hd-navbar-nav
                each val in mainNavItem
                    li.Hnn-item.m-btn.m-btn_info
                        a(href="#")= val

        section.main
            h1.main-title 我的文档
            p.main-content.
                这是一个很长很长而且还很无聊的段落，还没有结束，是的，非常非常地长。
                突然出现了一个 #[strong 充满力量感的单词]，这确实让人难以 #[em 忽视]。

        footer.ft
            p Copyright (c) 小火柴的蓝色理想

        each val in srcScripts
            script(src=baseScript + '/' + val)
```

```json
//- data.json
{
    "documentTitle":"测试文档",
    "tel":"400-888-8888",
    "mainNavItem":['登录','注册','关于','帮助'],
    "baseStyle":'style',
    "srcStyles":['bootstrap.css','main.css'],
    "baseScript":'/js',
    "srcScripts":['jquery.js','app.js']
}
```



执行`pug index.pug -P -w -O data.json`