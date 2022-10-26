# less

## 变量（Variables）

### 值变量

```less
@width: 10px;
@height: @width + 10px;

#header {
  width: @width;
  height: @height;
}
```

编译为：

```css
#header {
  width: 10px;
  height: 20px;
}
```

### 选择器变量

- 让选择器变成动态

```less
/* Less */
@mySelector: #wrap;
@Wrap: wrap;

@{mySelector}{ //变量名 必须使用大括号包裹
  color: #999;
  width: 50%;
}
.@{Wrap}{
  color:#ccc;
}
#@{Wrap}{
  color:#666;
}

/* 生成的 CSS */
#wrap{
  color: #999;
  width: 50%;
}
.wrap{
  color:#ccc;
}
#wrap{
  color:#666;
}

```

### 属性变量

```less
/* Less */
@borderStyle: border-style;
@Soild:solid;
#wrap{
  @{borderStyle}: @Soild;//变量名 必须使用大括号包裹
}

/* 生成的 CSS */
#wrap{
  border-style:solid;
}

```

### url变量

```less
/* Less */
@images: "../img";//需要加引号
body {
  background: url("@{images}/dog.png");//变量名 必须使用大括号包裹
}

/* 生成的 CSS */
body {
  background: url("../img/dog.png");
}

```

### 声明变量

> 结构: `@name:{属性: 值};`
> 使用: `@name();`

```less
@background: {background:red;};
#main{
    @background();
}
@Rules:{
    width: 200px;
    height: 200px;
    border: solid 1px red;
};
#con{
  @Rules();
}

/* 生成的 CSS */
#main{
  background:red;
}
#con{
  width: 200px;
  height: 200px;
  border: solid 1px red;
}
```

### 变量运算

> - 加减:以第一个数据单位为基准
> - 乘除: 注意单位统一

```less
/* Less */
@width:300px;
@color:#222;
#wrap{
  width:@width-20;
  height:@width-20*5;
  margin:(@width-20)*5;
  color:@color*2;
  background-color:@color + #111;
}

/* 生成的 CSS */
#wrap{
  width:280px;
  height:200px;
  margin:1400px;
  color:#444;
  background-color:#333;
}

```

### 变量作用域

> 就近原则

```less
/* Less */
@var: @a;
@a: 100%;
#wrap {
  width: @var;
  @a: 9%;
}

/* 生成的 CSS */
#wrap {
  width: 9%;
}
```

### 用变量去定义变量

```less
/* Less */
@fnord:  "I am fnord.";
@var:    "fnord";
#wrap::after{
  content: @@var; //将@var替换为其值 content:@fnord;
}
/* 生成的 CSS */
#wrap::after{
  content: "I am fnord.";
}

```

## 混合（Mixins）

> 混合（Mixin）是一种将一组属性从一个规则集包含（或混入）到另一个规则集的方法。假设我们定义了一个类（class）如下：

```less
.bordered {
  border-top: dotted 1px black;
  border-bottom: solid 2px black;
}
```

> 如果我们希望在其它规则集中使用这些属性呢？没问题，我们只需像下面这样输入所需属性的类（class）名称即可，如下所示：

```css
#menu a {
  color: #111;
  .bordered();
}

.post a {
  color: red;
  .bordered();
}
```

> .bordered 类所包含的属性就将同时出现在 #menu a 和 .post a 中了。（也可以使用 #ids 作为 mixin 使用。）

## 嵌套（Nesting）

> Less 提供了使用嵌套（nesting）代替层叠或与层叠结合使用的能力。假设我们有以下 CSS 代码：

```css
#header {
  color: black;
}
#header .navigation {
  font-size: 12px;
}
#header .logo {
  width: 300px;
}
```

用 Less 语言我们可以这样书写代码：

```less
#header {
  color: black;
  .navigation {
    font-size: 12px;
  }
  .logo {
    width: 300px;
  }
}
```

### & 用法

> `&`: 表示当前选择器的父级

```less
.clearfix {
  display: block;
  zoom: 1;

  &:after { //(& 表示当前选择器的父级）
    content: " ";
    display: block;
    font-size: 0;
    height: 0;
    clear: both;
    visibility: hidden;
  }
}
```

### @规则嵌套和冒泡 ( 媒体查询 )

> @ 规则（例如 @media 或 @supports）可以与选择器以相同的方式进行嵌套。@ 规则会被放在前面，同一规则集中的其它元素的相对顺序保持不变。这叫做冒泡（bubbling）。

```less
.component {
  width: 300px;
  @media (min-width: 768px) {
    width: 600px;
    @media  (min-resolution: 192dpi) {
      background-image: url(/img/retina2x.png);
    }
  }
  @media (min-width: 1280px) {
    width: 800px;
  }
}
```

编译为：

```css
.component {
  width: 300px;
}
@media (min-width: 768px) {
  .component {
    width: 600px;
  }
}
@media (min-width: 768px) and (min-resolution: 192dpi) {
  .component {
    background-image: url(/img/retina2x.png);
  }
}
@media (min-width: 1280px) {
  .component {
    width: 800px;
  }
}
```

## 运算（Operations）

### 算术运算符 +、-、*、/

可以对任何数字、颜色或变量进行运算。如果可能的话，算术运算符在加、减或比较之前会进行单位换算。==计算的结果以最左侧操作数的单位类型为准==。如果单位换算无效或失去意义，则忽略单位。无效的单位换算例如：px 到 cm 或 rad 到 % 的转换。

```less
// 所有操作数被转换成相同的单位
@conversion-1: 5cm + 10mm; // 结果是 6cm
@conversion-2: 2 - 3cm - 5mm; // 结果是 -1.5cm

// conversion is impossible
@incompatible-units: 2 + 5px - 3cm; // 结果是 4px

// example with variables
@base: 5%;
@filler: @base * 2; // 结果是 10%
@other: @base + @filler; // 结果是 15%
```

### 乘法和除法不作转换

因为这两种运算在大多数情况下都没有意义，一个长度乘以一个长度就得到一个区域，而 CSS 是不支持指定区域的。Less 将按数字的原样进行操作，并将为计算结果指定明确的单位类型。

```less
@base: 2cm * 3mm; // 结果是 6cm
```

你还可以对颜色进行算术运算：

```less
@color: #224488 / 2; //结果是 #112244
background-color: #112244 + #111; // 结果是 #223355
```

### calc() 特例

> 为了与 CSS 保持兼容，calc() 并不对数学表达式进行计算，但是在嵌套函数中会计算变量和数学公式的值。

```less
@var: 50vh/2;
width: calc(50% + (@var - 20px));  // 结果是 calc(50% + (25vh - 20px))
```

## 转义（Escaping）

> ==转义（Escaping）允许你使用任意字符串作为属性或变量值==。任何 `~"anything"` 或 `~'anything'` 形式的内容都将按原样输出，除非 interpolation。

```less
@min768: ~"(min-width: 768px)";
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}
```

编译为：

```css
@media (min-width: 768px) {
  .element {
    font-size: 1.2rem;
  }
}
```

> ==注意，从 Less 3.5 开始，可以简写为：==

```less
@min768: (min-width: 768px);
.element {
  @media @min768 {
    font-size: 1.2rem;
  }
}
```

## 函数（Functions）

- Less 内置了多种函数用于转换颜色、处理字符串、算术运算等

函数的用法非常简单。下面这个例子将介绍如何利用 percentage 函数将 0.5 转换为 50%，将颜色饱和度增加 5%，以及颜色亮度降低 25% 并且色相值增加 8 等用法：

```css
@base: #f04615;
@width: 0.5;

.class {
  width: percentage(@width); // returns `50%`
  color: saturate(@base, 5%);
  background-color: spin(lighten(@base, 25%), 8);
}
```

参见：[函数手册](https://less.bootcss.com/functions/)  

### 判断类型

|函数名|描述|使用|
|:----|:----|:----|
|isnumber| 判断类型|`isnumber(1234) //true`|
|iscolor| 是否为颜色|`iscolor(123) //false`|
|isurl|是否为url|`isurl(123)//false`|

### 颜色操作

|函数名|描述|使用|
|:----|:----|:----|
|saturate| 增加一定数值的颜色饱和度|
|lighten| 增加一定数值的颜色亮度|
|darken| 降低一定数值的颜色亮度|
|fade| 给颜色设定一定数值的透明度|
|mix| 根据比例混合两种颜色|

### 数学相关

|函数名|描述|使用|
|:----|:----|:----|
| ceil| 向上取整|
| floor| 向下取整|
| percentage| 将浮点数转换为百分比字符串|
| round| 四舍五入|
| sqrt| 计算一个数的平方根|
| abs| 计算数字的绝对值，原样保持单位|
| pow| 计算一个数的乘方|

## 命名空间和访问符

(不要和 CSS @namespace 或 namespace selectors 混淆了)。

有时，出于组织结构或仅仅是为了提供一些封装的目的，你希望对混合（mixins）进行分组。你可以用 Less 更直观地实现这一需求。假设你希望将一些混合（mixins）和变量置于 #bundle 之下，为了以后方便重用或分发：

```less
#bundle() {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
    &:hover {
      background-color: white;
    }
  }
  .tab { ... }
  .citation { ... }
}
```

现在，如果我们希望把 .button 类混合到 #header a 中，我们可以这样做：

```less
#header a {
  color: orange;
  #bundle.button();  // 还可以书写为 #bundle > .button 形式
}
```

注意：如果不希望它们出现在输出的 CSS 中，例如 #bundle .tab，请将 () 附加到命名空间（例如 #bundle()）后面。

## 映射（Maps）

从 Less 3.5 版本开始，你还可以将混合（mixins）和规则集（rulesets）作为一组值的映射（map）使用。

```less
#colors() {
  primary: blue;
  secondary: green;
}

.button {
  color: #colors[primary];
  border: 1px solid #colors[secondary];
}
```

输出符合预期：

```css
.button {
  color: blue;
  border: 1px solid green;
}
```

## 作用域（Scope）

> Less 中的作用域与 CSS 中的作用域非常类似。首先在本地查找变量和混合（mixins），如果找不到，则从“父”级作用域继承。

```less
@var: red;

#page {
  @var: white;
  #header {
    color: @var; // white
  }
}
```

> 与 CSS 自定义属性一样，混合（mixin）和变量的定义不必在引用之前事先定义。因此，下面的 Less 代码示例和上面的代码示例是相同的：

```less
@var: red;

#page {
  #header {
    color: @var; // white
  }
  @var: white;
}
```

参见：懒加载

## 注释（Comments）

块注释和行注释都可以使用：

```less
/* 一个块注释
 * style comment! */
@var: red;

// 这一行被注释掉了！
@var: white;
```

## 导入（Importing）

> “导入”的工作方式和你预期的一样。你可以导入一个 .less 文件，此文件中的所有变量就可以全部使用了。如果导入的文件是 .less 扩展名，则可以将扩展名省略掉：

```less
@import "library"; // library.less
@import "typo.css";
```

## 混合方法

```less
/* Less */
.generate-columns(4);

.generate-columns(@n, @i: 1) when (@i =< @n) {
  .column-@{i} {
    width: (@i * 100% / @n);
  }
  .generate-columns(@n, (@i + 1));
}
/* 生成后的 CSS */
.column-1 {
  width: 25%;
}
.column-2 {
  width: 50%;
}
.column-3 {
  width: 75%;
}
.column-4 {
  width: 100%;
}

```

### 无参方法

> 直接名称即可(建议还是写上括号, 避免混淆)

```less
/* Less */
.card { // 等价于 .card()
    background: #f6f6f6;
    -webkit-box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
    box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
}
#wrap{
  .card;//等价于.card();
}
/* 生成的 CSS */
#wrap{
  background: #f6f6f6;
  -webkit-box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
  box-shadow: 0 1px 2px rgba(151, 151, 151, .58);
}
```

### 默认参数方法

> `@arguments` 类似 js 中的 `arguments` 指代全部参数

```less
/* Less */
.border(@a:10px, @b:50px, @c:30px, @color:#000){
  border:solid 1px @color;
  box-shadow: @arguments;//指代的是 全部参数
}
#main{
  .border(0px,5px,30px,red);//必须带着单位
}
#wrap{
  .border(0px);
}
#content{
  .border;//等价于 .border()
}

/* 生成的 CSS */
#main{
  border:solid 1px red;
  box-shadow:0px,5px,30px,red;
}
#wrap{
  border:solid 1px #000;
  box-shadow: 0px 50px 30px #000;
}
#content{
  border:solid 1px #000;
  box-shadow: 10px 50px 30px #000;
}
```

### 方法匹配模式

- 类似java多态

```less
/* Less */
.triangle(top,@width:20px,@color:#000){
    border-color:transparent  transparent @color transparent ;
}
.triangle(right,@width:20px,@color:#000){
    border-color:transparent @color transparent  transparent ;
}
.triangle(bottom,@width:20px,@color:#000){
    border-color:@color transparent  transparent  transparent ;
}
.triangle(left,@width:20px,@color:#000){
    border-color:transparent  transparent  transparent @color;
}

.triangle(@_,@width:20px,@color:#000){
    border-style: solid;
    border-width: @width;
}

#main{
  .triangle(left, 50px, #999)
}

/* 生成的 CSS */
#main{
  border-color:transparent  transparent  transparent #999;
  border-style: solid;
  border-width: 50px;
}
```

### 方法命名空间

```less
/* Less */
#card(){
    background: #723232;
    .d(@w:300px){
        width: @w;
        
        #a(@h:300px){
            height: @h;//可以使用上一层传进来的方法
        }
    }
}
#wrap{
    #card > .d > #a(100px); // 父元素不能加 括号
}
#main{
    #card .d();
}
#con{
    //不得单独使用命名空间的方法
    //.d() 如果前面没有引入命名空间 #card ，将会报错
    
    #card; // 等价于 #card();
    .d(20px); //必须先引入 #card
}
/* 生成的 CSS */
#wrap{
  height:100px;
}
#main{
  width:300px;
}
#con{
  width:20px;
}
```

### 方法的条件筛选

```less
/* Less */
#card{
    
    // and 运算符 ，相当于 与运算 &&，必须条件全部符合才会执行
    .border(@width,@color,@style) when (@width>100px) and(@color=#999){
        border:@style @color @width;
    }

    // not 运算符，相当于 非运算 !，条件为 不符合才会执行
    .background(@color) when not (@color>=#222){
        background:@color;
    }

    // , 逗号分隔符：相当于 或运算 ||，只要有一个符合条件就会执行
    .font(@size:20px) when (@size>50px) , (@size<100px){
        font-size: @size;
    }
}
#main{
    #card>.border(200px,#999,solid);
    #card .background(#111);
    #card > .font(40px);
}
/* 生成后的 CSS */
#main{
  border:solid #999 200px;
  background:#111;
  font-size:40px;
}

```

## 继承

### extend

```less
/* Less */
.animation{
    transition: all .3s ease-out;
    .hide{
      transform:scale(0);
    }
}
#main{
    &:extend(.animation);
}
#con{
    &:extend(.animation .hide);
}

/* 生成后的 CSS */
.animation,#main{
  transition: all .3s ease-out;
}
.animation .hide , #con{
    transform:scale(0);
}

```

### all

```less
/* Less */
#main{
  width: 200px;
}
#main {
  &:after {
    content:"Less is good!";
  }
}
#wrap:extend(#main all) {}

/* 生成的 CSS */
#main,#wrap{
  width: 200px;
}
#main:after, #wrap:after {
    content: "Less is good!";
}

```

## 导入

### 导入less文件

```less
import "main"; 
//等价于
import "main.less";
#main{
  font-size:15px;
}

// 可以随意放置
@import "style";
```

### reference

> 不会编译, 值会引入

```less
/* Less */
@import (reference) "bootstrap.less"; 

#wrap:extend(.navbar all){}
```

### once

> `@import`语句的默认行为。这表明相同的文件只会被导入一次，而随后的导入文件的重复代码都不会解析

```less
@import (once) "foo.less";
@import (once) "foo.less"; // this statement will be ignored
```

### multiple

> 使用`@import (multiple)`允许导入多个同名文件

```less
/*Less*/

// file: foo.less
.a {
  color: green;
}
// file: main.less
@import (multiple) "foo.less";
@import (multiple) "foo.less";

/*生成后的 CSS*/
.a {
  color: green;
}
.a {
  color: green;
}
```

## 其他

### 注释

> `/**/` CSS原生注释，会被编译在 CSS 文件中。
> `/   /` Less提供的一种注释，不会被编译在 CSS 文件中。

```less

// 避免编译
/*Less*/
# main{
  width:~'calc(300px-30px)';
}

/*生成后的 CSS*/
# main{
  width:calc(300px-30px);
}
```

结构： `~' 值 '`

### 变量拼串

在平时工作中，这种需求 太常见了。 在下面例子中， 实现了不同的 transtion-delay、animation、@keyframes

```less
.judge(@i) when(@i=1){
  @size:15px;
}
.judge(@i) when(@i>1){
  @size:16px;
}
.loopAnimation(@i) when (@i<16) {
  
  .circle:nth-child(@{i}){
      .judeg(@i);
      border-radius:@size @size 0 0;
      animation: ~"circle-@{i}" @duration infinite @ease;
      transition-delay:~"@{i}ms";
  }
  @keyframes ~"circle-@{i}" {
      // do something...
  }
  .loopAnimation(@i + 1);
}
```

结构： `~"字符@{变量}字符"`

### 使用js

> less 是使用js编写

```less
/* Less */
@content:`"aaa".toUpperCase()`;
#randomColor{
  @randomColor: ~"rgb(`Math.round(Math.random() * 256)`,`Math.round(Math.random() * 256)`,`Math.round(Math.random() * 256)`)";
}
#wrap{
  width: ~"`Math.round(Math.random() * 100)`px";
  &:after{
      content:@content;
  }
  height: ~"`window.innerHeight`px";
  alert:~"`alert(1)`";
  #randomColor();
  background-color: @randomColor;
}
/* 生成后的 CSS */

// 弹出 1
#wrap{
  width: 随机值（0~100）px;
  height: 743px;//由电脑而异
  background: 随机颜色;
}
#wrap::after{
  content:"AAA";
}
```
