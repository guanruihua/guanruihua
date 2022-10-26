---
title: css-layout
date: 2020-09-09 00:00:58
tags:
	- css
	- layout
	- front-end
	- grid
	- flex
---

# css-layout

## 静态布局

- 制作的网页上的**元素尺寸一律以px为单位**。

- 布局特点： 页面上的布局是按最初写代码时候的布局方式进行布局的，常规的pc网站是进行设置了宽度值进行布局的，不会随着pc端的屏幕的大小而变化。

- 缺点： 不会随着pc端的屏幕大小而变化。

## 三栏布局

> 左右固定, 中间自适应

```html
<section class="layout float">
  <style type="text/css" media="screen">
    .layout.float .wrapper>div{
      min-height: 100px;
    }
    .layout.float .left{
      float: left;
      width: 300px;
      background: red;
    }
    .layout.float .center{
      background: yellow;
    }
    .layout.float .right{
      float: right;
      width: 300px;
      background: blue;
    }

  </style>
  <article class="wrapper">
    <div class="left"></div>
    <div class="right"></div>
    <div class="center">
      <h1>float布局</h1>
      1.我是float布局的中间部分
      2.我是float布局的中间部分
    </div>
  </article>
</section>


<section class="layout absolute">
  <style type="text/css" media="screen">
    .layout.absolute .wrapper{
      width: 100%;
      margin-top: 20px;
    }
    .layout.absolute .wrapper>div{
      min-height: 100px;
    }
    .layout.absolute .left{
      position: absolute;
      left: 0;
      width: 300px;
      background: red;
    }
    .layout.absolute .center{
      position: absolute;
      left: 300px;
      right: 300px;
      background: yellow;
    }
    .layout.absolute .right{
      position: absolute;
      right: 0;
      width: 300px;
      background: blue;
    }
  </style>
  <article class="wrapper">
    <div class="left"></div>
    <div class="center">
      <h1>absolute布局</h1>
      1.我是absolute布局的中间部分
      2.我是absolute布局的中间部分
    </div>
    <div class="right"></div>
  </article>
</section>


<section class="layout flex">
  <style type="text/css" media="screen">
    .layout.flex .wrapper{
      width: 100%;
      min-height: 100px;
      display: flex;
      margin-top: 140px;
    }
    .layout.flex .left{
      width: 300px;
      background: red;
    }
    .layout.flex .center{
      flex: 1;
      background: yellow;
    }
    .layout.flex .right{
      width: 300px;
      background: blue;
    }
  </style>
  <article class="wrapper">
    <div class="left"></div>
    <div class="center">
      <h1>flex布局</h1>
      1.我是flex布局的中间部分
      2.我是flex布局的中间部分
    </div>
    <div class="right"></div>
  </article>
</section>


<section class="layout table">
  <style type="text/css" media="screen">
    .layout.table .wrapper{
      display: table;
      width: 100%;
      min-height: 100px;
      margin-top: 20px;
    }
    .layout.table .left{
      display: table-cell;
      width: 300px;
      background: red;
    }
    .layout.table .center{
      display: table-cell;
      background: yellow;
    }
    .layout.table .right{
      display: table-cell;
      width: 300px;
      background: blue;
    }

  </style>
  <article class="wrapper">
    <div class="left"></div>
    <div class="center">
      <h1>table布局</h1>
      1.我是table布局的中间部分
      2.我是table布局的中间部分
    </div>
    <div class="right"></div>
  </article>
</section>


<section class="layout grid">
  <style type="text/css" media="screen">
    .layout.grid .wrapper{
      display: grid;
      grid-template-columns: 300px auto 300px;
      grid-template-rows: 100px;
      width: 100%;
      margin-top: 20px;
    }
    .layout.grid .left{
      background: red;
    }
    .layout.grid .center{
      background: yellow;
    }
    .layout.grid .right{
      background: blue;
    }

  </style>
  <article class="wrapper">
    <div class="left"></div>
    <div class="center">
      <h1>grid布局</h1>
      1.我是grid布局的中间部分
      2.我是grid布局的中间部分
    </div>
    <div class="right"></div>
  </article>
</section>

```

## 浮动布局

浮动布局进行调用浮动属性改变页面中元素的位置，浮动布局应该是目前各大网站用的最多的一种布局方式了，但是也特别复杂。浮动元素是脱离文档流的，但不脱离文本流。浮动元素有左浮动（float : left）和右浮动（float : right）两种

```css
.lian{
 width: 90%;
 padding-left: 5%;
}
.lian img{
 float: right;
 margin-top: -180px;
}
.phone ul li{
 list-style: none;
    margin-top: 50px;
    margin-left: 70px;
    color: #808080;
}
.phone ul li img{
 position: absolute;
 margin-left: -80px;
 float: left;
 margin-top: -5px;
}
.view{
 margin-top: 50px;
 margin-left: -5px;
 float: left;
}
.view input{
 width: 120px;
 height: 40px;
 border-radius: 6px;
 border: 1px solid #3CB371;
 background-color: #3CB371;
 font-size: 16px;
 color: white;

}
```

- 优点： 兼容性比较好
- 缺点： 浮动带来的影响比较多，页面宽度不够的时候会影响布局。

## 响应式布局

> 响应式设计与自适应设计的区别：
>
> - 响应式开发一套界面，通过检测视口分辨率，针对不同客户端在客户端做代码处理，来展现不同的布局和内容；
> - 自适应需要开发多套界面，通过检测视口分辨率，来判断当前访问的设备是pc端、平板、手机，从而请求服务层，返回不同的页面。

### **1. 媒体查询**

> - CSS3媒体查询可以让我们针对不同的媒体类型定义不同的样式，当重置浏览器窗口大小的过程中，页面也会根据浏览器的宽度和高度重新渲染页面。
> - 移动端优先首先使用的是min-width，PC端优先使用的max-width

#### PC优先

```css
/* pc width > 1024px */
    body {
        background-color: yellow;
    }
/* ipad pro */
@media screen and (max-width: 1024px) {
    body {
        background-color: #FF00FF;
    }
}
/* ipad */
@media screen and (max-width: 768px) {
    body {
        background-color: green;
    }
}
/* iphone6 7 8 plus */
@media screen and (max-width: 414px) {
    body {
        background-color: blue;
    }
}
/* iphoneX */
@media screen and (max-width: 375px) and (-webkit-device-pixel-ratio: 3) {
    body {
        background-color: #0FF000;
    }
}
/* iphone6 7 8 */
@media screen and (max-width: 375px) and (-webkit-device-pixel-ratio: 2) {
    body {
        background-color: #0FF000;
    }
}
/* iphone5 */
@media screen and (max-width: 320px) {
    body {
        background-color: #0FF000;
    }
}

```

#### 移动端优先

```css
/* iphone6 7 8 */
body {
    background-color: yellow;
}
/* iphone 5 */
@media screen and (max-width: 320px) {
    body {
      background-color: red;
    }
}
/* iphoneX */
@media screen and (min-width: 375px) and (-webkit-device-pixel-ratio: 3) {
    body {
      background-color: #0FF000;
    }
}
/* iphone6 7 8 plus */
@media screen and (min-width: 414px) {
    body {
      background-color: blue;
    }
}
/* ipad */
@media screen and (min-width: 768px) {
    body {
      background-color: green;
    }
}
/* ipad pro */
@media screen and (min-width: 1024px) {
    body {
      background-color: #FF00FF;
    }
}
/* pc */
@media screen and (min-width: 1100px) {
    body {
      background-color: black;
    }
}

```

### **2.百分比布局**

> - 计算复杂, 难以还原设计稿

```css
/* pc width > 1100px */
html, body { margin: 0;padding: 0;width: 100%;height: 100%;}
aside {
    width: 10%;
    height: 100%;
    background-color: red;
    float: left;
}
main {
    height: 100%;
    background-color: blue;
    overflow: hidden;
}
/* ipad pro */
@media screen and (max-width: 1024px) {
    aside {
      width: 8%;
      background-color: yellow;
    }
}
/* ipad */
@media screen and (max-width: 768px) {
    aside {
      float: none;
      width: 100%;
      height: 10%;
      background-color: green;
    }
    main {
      height: calc(100vh - 10%);
      background-color: red;
    }
}
/* iphone6 7 8 plus */
@media screen and (max-width: 414px) {
    aside {
      float: none;
      width: 100%;
      height: 5%;
      background-color: yellow;
    }
    main {
      height: calc(100vh - 5%);
      background-color: red;
    }
}
/* iphoneX */
@media screen and (max-width: 375px) and (-webkit-device-pixel-ratio: 3) {
    aside {
      float: none;
      width: 100%;
      height: 10%;
      background-color: blue;
    }
    main {
      height: calc(100vh - 10%);
      background-color: red;
    }
}
/* iphone6 7 8 */
@media screen and (max-width: 375px) and (-webkit-device-pixel-ratio: 2) {
    aside {
      float: none;
      width: 100%;
      height: 3%;
      background-color: black;
    }
    main {
      height: calc(100vh - 3%);
      background-color: red;
    }
}
/* iphone5 */
@media screen and (max-width: 320px) {
    aside {
      float: none;
      width: 100%;
      height: 7%;
      background-color: green;
    }
    main {
      height: calc(100vh - 7%);
      background-color: red;
    }
}

```

### **3.rem布局**

> - REM是CSS3新增的单位，并且移动端的支持度很高，Android2.x+,ios5+都支持。
> - rem单位都是相对于根元素html的font-size来决定大小的,根元素的font-size相当于提供了一个基准，当页面的size发生变化时，只需要改变font-size的值，那么以rem为固定单位的元素的大小也会发生响应的变化。 因此，如果通过rem来实现响应式的布局，只需要根据视图容器的大小，动态的改变font-size即可（而em是相对于父元素的）。

rem响应式的布局思想：

一般不要给元素设置具体的宽度，但是对于一些小图标可以设定具体宽度值

高度值可以设置固定值，设计稿有多大，我们就严格有多大

所有设置的固定值都用rem做单位（首先在HTML总设置一个基准值：px和rem的对应比例，然后在效果图上获取px值，布局的时候转化为rem值)

js获取真实屏幕的宽度，让其除以设计稿的宽度，算出比例，把之前的基准值按照比例进行重新的设定，这样项目就可以在移动端自适应了

rem布局的缺点：

在响应式布局中，必须通过js来动态控制根元素font-size的大小，也就是说css样式和js代码有一定的耦合性，且必须将改变font-size的代码放在css样式之前

```css
/*将视图容器分为10份，font-size用十分之一的宽度来表示，最后在header标签中执行这段代码，就可以动态定义font-size的大小，从而1rem在不同的视觉容器中表示不同的大小，用rem固定单位可以实现不同容器内布局的自适应。*/
function refreshRem() {
    var docEl = doc.documentElement;
    var width = docEl.getBoundingClientRect().width;
    var rem = width / 10;
    docEl.style.fontSize = rem + 'px';
    flexible.rem = win.rem = rem;
}
win.addEventListener('resize', refreshRem);

```

rem布局适配多屏幕适配的最佳方式

```css
/* pc width > 1100px */
html{ font-size: 100%;}
body {
    background-color: yellow;
    font-size: 1.5rem;
}
/* ipad pro */
@media screen and (max-width: 1024px) {
    body {
      background-color: #FF00FF;
      font-size: 1.4rem;
    }
}
/* ipad */
@media screen and (max-width: 768px) {
    body {
      background-color: green;
      font-size: 1.3rem;
    }
}
/* iphone6 7 8 plus */
@media screen and (max-width: 414px) {
    body {
      background-color: blue;
      font-size: 1.25rem;
    }
}
/* iphoneX */
@media screen and (max-width: 375px) and (-webkit-device-pixel-ratio: 3) {
    body {
      background-color: #0FF000;
      font-size: 1.125rem;
    }
}
/* iphone6 7 8 */
@media screen and (max-width: 375px) and (-webkit-device-pixel-ratio: 2) {
    body {
      background-color: #0FF000;
      font-size: 1rem;
    }
}
/* iphone5 */
@media screen and (max-width: 320px) {
    body {
      background-color: #0FF000;
      font-size: 0.75rem;
    }
}
```

### **4.视口单位**

> - css3中引入了一个新的单位vw/vh，与视图窗口有关

| 单位 | 含义                                                      |
| ---- | --------------------------------------------------------- |
| vw   | 相对于视窗的宽度，1vw 等于视口宽度的1%，即视窗宽度是100vw |
| vh   | 相对于视窗的高度，1vh 等于视口高度的1%，即视窗高度是100vh |
| vmin | vw和vh中的较小值                                          |
| vmax | vw和vh中的较大值                                          |

用视口单位度量，视口宽度为100vw，高度为100vh（左侧为竖屏情况，右侧为横屏情况）。

例如，在桌面端浏览器视口尺寸为650px，那么 1vw = 650 * 1% = 6.5px（这是理论推算的出，如果浏览器不支持0.5px，那么实际渲染结果可能是7px）。

使用视口单位来实现响应式有两种做法：

1. 仅使用vw作为CSS单位

2. 搭配vw和rem

## 定位布局

> 定位布局时利用position属性控制页面元素设置一些不规则布局。
> 定位元素的各个属性：

1. static 静态定位： HTML元素的默认值，即没有定位，元素出现在正常的流中。

```css
div.static {
    position: static;
    border: 3px solid #73AD21;
}
```

2. fixed 固定定位： 元素的位置相对于浏览器窗口是固定位置。即使窗口是滚动的它也不会移动。Fixed定位使元素的位置与文档流无关，因此不占据空间。Fixed定位的元素和其他元素重叠。

```css
p.pos_fixed{
    position:fixed;
    top:30px;
    right:5px;
}
```

3. relative 相对定位： 相对定位元素的定位是以自身为参照物。对象不可层叠、不脱离文档流，移动相对定位元素，但它原本所占的空间不会改变。通过 top,bottom,left,right 定位。

```css
h2.pos_top
{
    position:relative;
    top:-50px;
}
```

4. absolute 绝对定位 absolute 定位使元素的位置与文档流无关，因此不占据空间。元素和其他元素重叠。通过 top,bottom,left,right 定位。选取其最近一个最有定位设置的父级对象进行绝对定位，如果对象的父级没有设置定位属性，absolute元素将以body坐标原点进行定位。

```css
h2{
    position:absolute;
    left:100px;
    top:150px;
}
```

5. sticky 粘性定位 基于用户的滚动位置来定位。粘性定位的元素是依赖于用户的滚动，在 position:relative 与 position:fixed 定位之间切换。它的行为就像 position:relative; 而当页面滚动超出目标区域时，它的表现就像 position:fixed，它会固定在目标位置。元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。这个特定阈值指的是 top, right, bottom 或 left 之一，换言之，指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。

```css
div.sticky {
    position: -webkit-sticky; /* Safari */
    position: sticky;
    top: 0;
    background-color: green;
    border: 2px solid #4CAF50;
}
```

6. z-index 因为页面中元素的定位与文档流无关，所以定位的元素可以覆盖在文档流上面。所以z-index属性指定了一个元素的堆叠顺序（哪个元素应该放在前面/后面）。z-index的值必须取正整数，数值越大显示的优先级就越高。 如果两个定位元素重叠，而且还没有指定z - index，name最后定位在HTML代码中的元素将被显示在最前面。
