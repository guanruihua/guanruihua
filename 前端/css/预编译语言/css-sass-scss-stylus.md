---
title: 预处理语言
date: 2020-09-09 00:58:00
tags:
	- css
	- sass
	- scss
	- stylus
	- 预处理语言
---

# CSS  Sass Scss stylus

> - css预处理器是为了给css添加一些编程的特性
>   - 无需考虑浏览器的兼容性问题
>   - 使用变量
>   - 简单的程序逻辑
>   - 函数

## 变量

> - sass:    $变量名 :  变量值
>
> - less, css:  @变量名: 变量值
>
> - stylus :
>   - 可以是任何字符开头(@除外)
>   - 变量之间可以使用冒号, 空格隔开
>   - 变量名 = 变量值
>   - 属性 属性值
>   - 栗子:
>

```stylus
maincolor = #092873
siteWidth = 1024px
borderStyle = dotted
body 
  color maincolor
  border 1px borderStyle mainColor
  max-width siteWidth
```

## 嵌套

```scss
//scss style //----------------------------------- 
nav { 
    ul { 
       margin: 0; 
       padding: 0; 
    } 
    li { 
       display: inline-block; 
    } 
    a { 
       display: block; 
       padding: 6px 12px; 
       text-decoration: none; 
    } 
}
//css style //----------------------------------- 
nav ul { 
    margin: 0; 
    padding: 0; 
    list-style: none; 
} 
nav li { 
    display: inline-block; 
} 
nav a { 
    display: block; 
    padding: 6px 12px; 
    text-decoration: none; 

```

## 运算符

```css
body {
  margin: (14px/2);
  top: 50px + 100px;
  right: 80 * 10%;
}
```

## 颜色函数

> 预处理语言都会内置一些颜色处理函数

### sass颜色函数

```sass
lighten($color, 10%); 
darken($color, 10%);  
saturate($color, 10%);   
desaturate($color, 10%);
grayscale($color);  
complement($color); 
invert($color); 
mix($color1, $color2, 50%); 

使用
@color: #0982C1;
border : 3px solid darken($color, 50%);
```

### less css颜色函数

```css
lighten(@color, 10%); 
darken(@color, 10%);  
saturate(@color, 10%);  
desaturate(@color, 10%); 
spin(@color, 10); 
spin(@color, -10); 
mix(@color1, @color2);

@color: #0982C1;
h1 {
  background: @color;
  border: 3px solid darken(@color, 50%);
}
```

### Stylus颜色处理函数

```css
lighten(color, 10%); 
darken(color, 10%);  
saturate(color, 10%);  
desaturate(color, 10%); 

color = #0982C1 
h1
  background color
  border 3px solid darken(color, 50%)
```

## 导入(import)

## 继承

### sass

```sass
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}
.success {
  @extend .message;
  border-color: green;
}
.error {
  @extend .message;
  border-color: red;
}
.warning {
  @extend .message;
  border-color: yellow;
}
```

### less css

```less
.message {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}
.success {
  .message;
  border-color: green;
}
.error {
  .message;
  border-color: red;
}
.warning {
  .message;
  border-color: yellow;
}
```

## Mixins混入

### sass

```sass
@mixin error($borderWidth: 2px) {
  border: $borderWidth solid #F00;
  color: #F00;
}
.generic-error {
  padding: 20px;
  margin: 4px;
  @ include error(); //这里调用默认 border: 2px solid #F00;
}
.login-error {
  left: 12px;
  position: absolute;
  top: 20px;
  @ include error(5px); //这里调用 border:5px solid #F00;
}
```

### less

```less
.error(@borderWidth: 2px) {
  border: @borderWidth solid #F00;
  color: #F00;
}
.generic-error {
  padding: 20px;
  margin: 4px;
  .error(); //这里调用默认 border: 2px solid #F00;
}
.login-error {
  left: 12px;
  position: absolute;
  top: 20px;
  .error(5px); //这里调用 border:5px solid #F00;
}
```

Stylus

```
error(borderWidth = 2px){
 border: borderWidth solid #F00;
 color: #F00;
}
.login-error {
  left: 12px;
  position: absolute;
  top: 20px;
  error(5px); 
}
```

最后编译结果

```css
.generic-error {
  padding: 20px;
  margin: 4px;
  border: 2px solid #f00;
  color: #f00;
}
.login-error {
  left: 12px;
  position: absolute;
  top: 20px;
  border: 5px solid #f00;
  color: #f00;
}
```

## 3D文本

### Sass

```sass
@mixin text3d($color) {
  color: $color;
  text-shadow: 1px 1px 0px darken($color, 5%),
               2px 2px 0px darken($color, 10%),
               3px 3px 0px darken($color, 15%),
               4px 4px 0px darken($color, 20%),
               4px 4px 2px #000;
}
 
h1 {
  font-size: 32pt;
  @ include text3d(#0982c1);
}
```

### less

```less
.text3d(@color) {
  color: @color;
  text-shadow: 1px 1px 0px darken(@color, 5%),
               2px 2px 0px darken(@color, 10%),
               3px 3px 0px darken(@color, 15%),
               4px 4px 0px darken(@color, 20%),
               4px 4px 2px #000;
}
 
span {
  font-size: 32pt;
  .text3d(#0982c1);
}
```

### stylus

```stylus
text3d(color)
  color: color
  text-shadow: 1px 1px 0px darken(color, 5%), 
               2px 2px 0px darken(color, 10%), 
               3px 3px 0px darken(color, 15%), 
               4px 4px 0px darken(color, 20%), 
               4px 4px 2px #000
span
  font-size: 32pt
  text3d(#0982c1)
```

### css

```css
span {
  font-size: 32pt;
  color: #0982c1;
  text-shadow:  1px 1px 0px #097bb7,
            2px 2px 0px #0875ae,
            3px 3px 0px #086fa4,
            4px 4px 0px #07689a,
            4px 4px 2px #000;
}
```

## 高级语法

> sass支持条件控制语句:@if @else  @for @while @each

### @if @else

```sass
$lte7: true;
$type: monster;
.ib{
    display:inline-block;
    @if $lte7 {
        *display:inline;
        *zoom:1;
    }
}
p {
  @if $type == ocean {
    color: blue;
  } @else if $type == matador {
    color: red;
  } @else if $type == monster {
    color: green;
  } @else {
    color: black;
  }
}
```

### @for

两种形式

```sass
@for $var from <start> throuh <end> (包括end这个数)
@for $var from <start> to <end>   (不包括end这个数)
```

### @while

```sass
$i: 6;
@while $i > 0 {
  .item-#{$i} { width: 2em * $i; }
  $i: $i - 2;
}
```

### @each

```sass
$animal-list: puma, sea-slug, egret, salamander;
@each $animal in $animal-list {
  .#{$animal}-icon {
    background-image: url('/images/#{$animal}.png');
  }
}
```

## Sass 和Scss区别

> - scss是sass3引入的新的语法
>   - 区别:
>     - 拓展名
>     - sass不带大括号和分号

## Sass 和Less区别

### Sass和Less编译环境不一样

> - Sass的安装需要Ruby环境，是在服务端处理的
> - Less是需要引入less.js来处理Less代码输出css到浏览器，也可以在开发环节使用Less，然后编译成css文件，直接放到项目中，也有 Less.app、SimpleLess、CodeKit.app这样的工具，也有在线编译地址。

### 输出设置不同

> Less没有输出设置，Sass提供4中输出选项：nested, compact, compressed 和 expanded。

输出样式的风格可以有四种选择，默认为nested

- nested：嵌套缩进的css代码
- expanded：展开的多行css代码
- compact：简洁格式的css代码
- compressed：压缩后的css代码
