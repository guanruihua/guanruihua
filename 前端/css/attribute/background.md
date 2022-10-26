---
title: background
date: 2020-10-07 22:16:52
tags: 
	- css
	- background
	- basic
	- demo
---

# background

## basic

> `background`: 是简写属性, 可以在一次声明中定义或多个属性, 除去特定的属性, 其他属性可以按照任意位置放置

```css
background ： [background-color] | [background-image] | [background-position][/background-size] | [background-repeat] | [background-attachment] | [background-clip] | [background-origin],...
```

CSS3 `background`支持多重背景，主要是靠`origin`、`clip`和`size`组成新的`background`多次叠加

### 注意

- `background` 属性被指定多个背景层时，使用逗号分隔每个背景层。每个属性用空格隔开
- 如果有`size`的值必须紧跟着`position`，用 / 隔开
- `background-color`只能设置一个值 且被包含在最后一层
- `border-box/padding-box/content-box` 如果出现一次则同时设置`origin`和`clip`，如果出现两次 第一个指定`origin`，第二个指定`clip`
- 多重背景，其他属性只有一个表示所有的背景共用一个属性的属性值

### background-color

> - `background-color`会设置元素的背景色，属性的值为颜色值或者关键字“transparent”二者选其一。
> - `background-color`的优先级比`background-image`低，如果同时存在`backgound-color`和`background-image`时，image会显示在color上面

```css
/* Keyword values */
background-color: red;

/* Hexadecimal value */
background-color: #bbff00;

/* RGB value */
background-color: rgb(255, 255, 128);

/* HSLA value */
background-color: hsla(50, 33%, 25%, 0.75);

/* Special keyword values */
background-color: currentColor; /* 纯白色*/
background-color: transparent;

/* Global values */
background-color: inherit;
background-color: initial;
background-color: unset;
```

### background-image

> background-image属性用于为一个元素设置一个或多个背景图像, 过关背景图像用`,`隔开
>
> `background-image:url('...') || linerat-gradient(渐变)|| none[无背景图]`

```css
/* html */
<div class="border-image-color"></div>
/* css */
.border-image-color {
  width: 500px;
  height: 400px;
  border: 20px dashed yellowgreen;
  background-image: url(https://user-gold-cdn.xitu.io/2020/3/19/170f20b3c106f518?w=642&h=339&f=png&s=107259);
  background-repeat: no-repeat;
  background-origin: border-box;
  background-color: lightSkyBlue;
  background-position: -60px -20px;
}

```

> `background-image`的绘制方向在Z轴上堆叠方式，先指定的图像会在后指定图像上面
>
> `background-image`绘制在`border`之下，`background-color`之上
>
> `background-image`的绘制、显示位置与`background-position`、`background-clip`、`background-origin`相关

![实例](https://user-gold-cdn.xitu.io/2020/3/20/170f597acad1d3a8?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### background-origin

  `background-origin`规定了背景图片`background-image`属性的原点位置的背景相对区域，为`background-position`设置初始位置。但是当`background-attachment`为`fixed`时，此属性将被忽略，不起作用。

#### 属性值： 默认值/padding-box

- border-box 背景图片的摆放以border区域为参考
- padding-box 背景图片的摆放以padding区域为参考
- content-box 背景图片的摆放以content区域为参考

### background-position

  `background-position`为每个背景图片设置初始位置，`background-position`的原点是`background-origin`定义的位置，相对偏移量都是以原点为基准。

#### 属性值：默认值/left top

三种取值类型可以混合使用

- 关键字
  - `center`用来居中背景图片
  - `left`、`right`指定图片放置于X轴的左右边缘
  - `top`、`bottom`指定图片放置于Y轴的上下边缘
- length 数值+ 'px'，指定相对于X,Y坐标的位置
- percentage  百比分，指定相对于X,Y坐标的位置

#### 值个数

- 关键字取单个值，另外一个位置默认为 center
- length、percentage取一个值 当前值同时指定X轴与Y轴的坐标
- length、percentage取两个值，第一个值定义X轴的方向、第二值定义Y轴的方向
- 四个值 position: bottom 10px right 20px;  关键字定义位置，length/percentage定义距离

#### 扩展

`background-position`属性已经得到扩展，它允许我们指定背景图片距离任意角的偏移量，只要我们在偏移量(`length`/`percentage`)前面指定关键字(`top`,`bottom`,`left`,`right`)

#### percentage语法

  百分比值的偏移指定图片的相对位置和容器相对位置重合。值0%表示图片的(左上)边界与容器(左上)边界重合，值100%代表图片的右边界（或下边界）和容器的右边界（或下边界）重合。值50%则代表图片的中点和容器的中点重合。

percentage公式：
  `(container width - image width) * (position x%) = (x offset value)`
  `(container height - image height) * (position y%) = (y offset value)`  

![img](https://user-gold-cdn.xitu.io/2020/3/20/170f5c2c5723acd3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```css
/* html */
<div class="border-image-color"></div>
/* css */
/*
    图片宽度： 642px
    容器宽度: 742px
    容器宽度：，容器box-sizing与背景background-origin同步作用
    若想X方向上移动20px：20px / (742px - 642px) = 20%
    X方向上移动20%，实际移动像素值： (742px - 642px) * 20% = 20px
    
    其中若background-origin为padding-box
    实际容器宽度应为包裹图片时原点origin控制的部分
    容器宽度则需要减去边框40px，实际为702px，再在X轴移动20%时实际移动的像素：
    (702px - 642px) * 20% = 12px
*/
.border-image-color {
  box-sizing: border-box;
  width: 742px;
  height: 400px;
  border: 20px dashed yellowgreen;
  background-image: url(https://user-gold-cdn.xitu.io/2020/3/19/170f20b3c106f518?w=642&h=339&f=png&s=107259);
  background-repeat: no-repeat;
  background-origin: border-box;
  background-color: lightSkyBlue;
  background-position: 20% 0px;
}
```

### background-size

  `background-size`设置背景图片的大小。图片可以保有原有的尺寸，或者拉伸到新的尺寸，或者保持其原有比例的同时缩放到元素的可用空间尺寸

#### 属性值：默认值/auto auto

- 关键字
  - auto 以背景图片的比例缩放背景图片。
  - cover 缩放背景图片以完全覆盖背景区，可能背景图片部分看不见。和 contain 值相反，cover值尽可能大的缩放背景图像并保持图像的宽高比例(图像不会被压扁)
  - contain (背景区容纳背景图片)缩放背景图片以完全装入背景区，可能背景区部分空白。contain尽可能的缩放背景并保持图像的宽高比例(图像不会被压缩)

![img](https://user-gold-cdn.xitu.io/2020/3/20/170f5f4c42de2970?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- length 指定背景图的宽高，不能为负值
- percentage 指定背景图片相对背景区(同`position`的容器宽高)的百分比。背景区由`background-origin`设置

### background-repeat

  `background-repeat`属性定义背景图像的重复方式。背景图像可以沿着水平轴，垂直轴，两个轴重复，或者根本不重复。

| 属性      | 定义                                                         |
| :-------- | :----------------------------------------------------------- |
| repeat    | 图像会按需**重复来覆盖整个背景图片所在的区域**. 最后一个图像会被裁剪, 如果它的大小不合适的话. repeat-x、repeat-y指定单方向重复 |
| space     | 图像会尽可能得重复, 但是不会裁剪. 第一个和最后一个图像会被固定在元素(element)的相应的边上, 同时空白会均匀地分布在图像之间. background-position属性会被忽视, 除非只有一个图像能被无裁剪地显示. 只在一种情况下裁剪会发生, 那就是图像太大了以至于没有足够的空间来完整显示一个图像. |
| round     | 随着允许的空间在尺寸上的增长, 被重复的图像将会伸展(没有空隙), 直到有足够的空间来添加一个图像. 当下一个图像被添加后, 所有的当前的图像会被压缩来腾出空间. 例如, 一个图像原始大小是260px, 重复三次之后, 可能会被伸展到300px, 直到另一个图像被加进来. 这样他们就可能被压缩到225px. |
| no-repeat | 图像不会被重复(因为背景图像所在的区域将可能没有完全被覆盖). 那个没有被重复的背景图像的位置是由background-position属性来决定. |

| <img src="https://user-gold-cdn.xitu.io/2020/3/20/170f60a5313a224c?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" alt="round" style="zoom: 33%;" /> | <img src="https://user-gold-cdn.xitu.io/2020/3/20/170f609015dc74b3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" alt="space" style="zoom: 33%;" /> |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
|                                                              |                                                              |

### background-attachment

  `background-attachment`属性决定背景图像的位置是在视口内固定，还是随着包含它的区块滚动。

#### 属性值

- fixed 表示背景相对于视口固定。即使一个元素拥有滚动机制，背景也不会随着元素的内容滚动
- local 表示背景相对于元素的内容固定。如果一个元素拥有滚动机制，背景将会随着元素的内容滚动， 并且背景的绘制区域和定位区域是相对于可滚动的区域而不是包含他们的边框。
- scroll 示背景相对于元素本身固定，而不是随着它的内容滚动（对元素边框是有效的）。

### background-clip

  `background-clip`设置元素的背景（背景图片或颜色）是否延伸到边框下面

#### 取值：默认值/border-box

- border-box 背景延伸至边框外沿（但是在边框下层）
- padding-box 背景延伸至内边距（padding）外沿。不会绘制到边框处
- content-box 背景被裁剪至内容区（content box）外沿
- text 背景被裁剪成文字的前景色,属性值为text时 属性应设置为 `-webkit-background-clip`, 字体的颜色使用背景色 即`color: transparent`/`-webkit-text-fill-color: transparent`;

```css
/* html */
<div class="border-image-color">
  text
</div>
/* css */
.border-image-color {
  box-sizing: border-box;
  width: 642px;
  height: 400px;
  font-size: 100px;
  line-height: 300px;
  text-align: center;
  border: 20px dashed yellowgreen;
  background-image: url(https://user-gold-cdn.xitu.io/2020/3/19/170f20b3c106f518?w=642&h=339&f=png&s=107259);
  background-repeat: no-repeat;
  background-origin: border-box;
  -webkit-background-clip: text;
  color: transparent;
}
```

![clip:text](https://user-gold-cdn.xitu.io/2020/3/20/170f616e1f91ffb1?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 多重背景使用实例

> 工作场景： 官网主页底部为铺满不同的图片，图片之上再进行 基本的图片文字排版

> 使用多重背景处理可以避免冗余空的html元素格外铺图片，避免伪元素和伪类去进行多余的position定位

![多图片为底](https://user-gold-cdn.xitu.io/2020/3/20/170f6d6502c3d6c7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

```css
/* html */
<div class="border-image-color">
  <div class="section">
    我是排版一
  </div>
  <div class="section">
    我是排版二
  </div>
</div>

/* css */
.border-image-color {
  display: flex;
  flex-direction: column;
  box-sizing: border-box;
  width: 600px;
  height: 580px;
  border: 1px solid yellowgreen;
  text-align: center;
  background: url(https://user-gold-cdn.xitu.io/2020/3/20/170f5f4c42de2970?w=672&h=299&f=png&s=146779) 0px 0px,
              url(https://user-gold-cdn.xitu.io/2020/3/19/170f20b3c106f518?w=642&h=339&f=png&s=107259) 0px 250px;
  background-size: contain 250px;
  background-repeat: no-repeat;
}
.section {
  width: 400px;
  height: 200px;
  line-height: 200px;
  margin: 30px auto;
  text-align: center;
  background-color: rgba(0,0,0, .8);
  color: #fff;
}
```

## CSS backgroundImage 7个好用的技巧

### 背景图完美适配视口

```css
body {
  background-image: url('https://images.unsplash.com/photo-1573480813647-552e9b7b5394?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2253&q=80');
  background-repeat: no-repeat;
  background-position: center;
  background-attachment: fixed;
  background-size: cover;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
}
```

![clipboard.png](https://segmentfault.com/img/bVbHhuO)

### 在CSS中使用多个背景图片

```css
body {
  background-image: url(https://image.flaticon.com/icons/svg/748/748122.svg), url(https://images.unsplash.com/photo-1478719059408-592965723cbc?ixlib=rb-1.2.1&auto=format&fit=crop&w=2212&q=80);
  background-position: center, top;
  background-repeat: repeat, no-repeat;
  background-size: contain, cover;
}
```

![clipboard.png](https://segmentfault.com/img/bVbG9V2)

### 创建一个三角形的背景图像

另一个很酷的背景特效就是三角形背景，当我们想展示某些完全不同的选择（例如白天和黑夜或冬天和夏天）时，这种特效就更加棒。

思路是这样的，首先创建两个`div`，然后将两个背景都添加到其中，然后，第二个`div`使用`clip-path`属性画出三角形。

![clipboard.png](https://segmentfault.com/img/bVbHhsp)

```html
<body>
  <div class="day"></div>
  <div class="night"></div>
</body>
```

```css
body {
  margin: 0;
  padding: 0;
}

div {
  position: absolute;
  height: 100vh;
  width: 100vw;
}

.day {
  background-image: url("https://images.unsplash.com/photo-1477959858617-67f85cf4f1df?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2613&q=80");
  background-size: cover;
  background-repeat: no-repeat;
}

.night {
  background-image: url("https://images.unsplash.com/photo-1493540447904-49763eecf55f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80");
  background-size: cover;
  background-repeat: no-repeat;
  clip-path: polygon(100vw 0, 0% 0vh, 100vw 100vh);
}
```

> `clip-path`: 使用裁剪方式穿啊金元素的可显示区域, 区域内的部分显示, 区域外隐藏
>
> `clip-path: polygon(100vw 0, 0% 0vh, 100vw 100vh);`
>
> - polygon是多边形里面是指定的端点
>
> <https://developer.mozilla.org/zh-CN/docs/Web/CSS/clip-path>

### 背景图像上添加叠加渐变

有时我们想在背景上添加一些文字，但有的图片太亮，导致字看不清楚，所以这里我们就需要让背景图叠加一些暗乐来突出文字效果。

例如，可以通过添加粉红橙色渐变或红色至透明渐变来增强日落图像，这些情况下使用叠加的渐变就很容易做到。

![clipboard.png](https://segmentfault.com/img/bVbHhtM)

**css**

```css
body {
  background-image: 
    linear-gradient(4deg, rgba(38,8,31,0.75) 30%, rgba(213,49,127,0.3) 45%, rgba(232,120,12,0.3) 100%),
    url("https://images.unsplash.com/photo-1503803548695-c2a7b4a5b875?ixlib=rb-1.2.1&auto=format&fit=crop&w=2250&q=80");
  background-size: cover;
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-position: center
}
```

### 创建一个颜色动态变化的背景

如果你很多颜色，你想确认哪种颜色更适合背景图片的颜色，刚动态更改背景颜色的技巧就很有用。

```css
@keyframes background-overlay-animation {
  0%   {
      background-image: 
        linear-gradient(4deg, rgba(255,78,36,0.3) 50%, rgba(255,78,36,0.3) 100%), url("https://images.unsplash.com/photo-1559310589-2673bfe16970?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80");
  }
  25%  {
      background-image: 
         linear-gradient(4deg, rgba(213,49,127,0.3) 50%, rgba(213,49,127,0.3) 100%), url("https://images.unsplash.com/photo-1559310589-2673bfe16970?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80");
  }
  50%  {
    background-image: 
       linear-gradient(4deg, rgba(36,182,255,0.3) 50%, rgba(36,182,255,1) 100%),
     url("https://images.unsplash.com/photo-1559310589-2673bfe16970?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80");
  }
  100% {
    background-image: 
        linear-gradient(4deg, rgba(0,255,254,0.3) 50%, rgba(0,255,254,0.3) 100%),
        url("https://images.unsplash.com/photo-1559310589-2673bfe16970?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80");
  }
}

@-webkit-keyframes background-overlay-animation {
  0%   {
      background-image: 
        linear-gradient(4deg, rgba(255,78,36,0.3) 50%, rgba(255,78,36,0.3) 100%)
        url("https://images.unsplash.com/photo-1559310589-2673bfe16970?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80");
  }
  25%  {
      background-image: 
         linear-gradient(4deg, rgba(213,49,127,0.3) 50%, rgba(213,49,127,0.3) 100%),
        url("https://images.unsplash.com/photo-1559310589-2673bfe16970?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80");
  }
  50%  {
    background-image: 
       linear-gradient(4deg, rgba(36,182,255,0.3) 50%, rgba(36,182,255,1) 100%),
     url("https://images.unsplash.com/photo-1559310589-2673bfe16970?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80");
  }
  100% {
    background-image: 
        linear-gradient(4deg, rgba(0,255,254,0.3) 50%, rgba(0,255,254,0.3) 100%),
  
```

![图片描述](https://segmentfault.com/img/bVbHhtY)

### 制作网格背景图像

![clipboard.png](https://segmentfault.com/img/bVbHhum)

```html
<body>
<div class="container">
  <div class="item_img"></div>
  <div class="item"></div>
  <div class="item_img"></div>
  <div class="item"></div>
  <div class="item"></div>
  <div class="item_img"></div>
  <div class="item"></div>
  <div class="item_img"></div>
  <div class="item"></div>
  <div class="item"></div>
  <div class="item_img"></div>
  <div class="item"></div>
  <div class="item_img"></div>
  <div class="item"></div>
  <div class="item_img"></div>
  <div class="item"></div>
</div>
</body>
```

```scss
body {
 margin: 0;
  padding: 0;
}

.container {
    position: absolute;
    width: 100%;
    height: 100%;
    background: black;
    display: grid;
    grid-template-columns: 25fr 30fr 40fr 15fr;
    grid-template-rows: 20fr 45fr 5fr 30fr;
    grid-gap: 20px;
    .item_img {
      background-image: url('https://images.unsplash.com/photo-1499856871958-5b9627545d1a?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2207&q=80');
    background-repeat: no-repeat;
    background-position: center;
    background-attachment: fixed;
    background-size: cover;
  }
}
```

> `fr`: 是一个自适应单位, 用于在一系列长度值中分配剩余空间，如果多个已指定了多个部分，则剩下的空间根据各自的数字按比例分配。

### 将背景图像设置为文本颜色

使用`background-image`与` background-clip `，可以实现背景图像对文字的优美效果。 在某些情况下，它可能非常有用，尤其是当我们想创建一个较大的文本标题而又不如普通颜色那么枯燥的情况。

![图片描述](https://segmentfault.com/img/bVbHhuz)

```html
<body>
  <h1>Hello world!</h1>
</body>
```

```scss
body {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  width: 100%;
  text-align: center;
  min-height: 100vh;
  font-size: 120px;
  font-family:Arial, Helvetica, sans-serif;
}

h1 {
   background-image: url("https://images.unsplash.com/photo-1462275646964-a0e3386b89fa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2600&q=80");
  background-clip: text;
  -webkit-background-clip: text;
  color: transparent;
}
```
