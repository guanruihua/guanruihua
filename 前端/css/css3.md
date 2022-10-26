---
title: css3
date: 2020-09-08 23:54:12
tags: 
	- css
	- css3
	- front-end
---

# CSS3

## 边框

### border-radius:圆角

```css

div{
  border:2px solid;
  border-radius:25px;//添加圆角
  -moz-border-radius:25px; /* Old Firefox */
}
```

### box-shadow:边框阴影

```css
div{
 box-shadow: 10px 10px 5px #888888;
}
```

### border-image:边框图片

> 语法`box-shadow: h-shadow v-shadow blur spread color inset;`

| 值         | 描述                                     |
| :--------- | :--------------------------------------- |
| *h-shadow* | 必需。水平阴影的位置。允许负值。         |
| *v-shadow* | 必需。垂直阴影的位置。允许负值。         |
| *blur*     | 可选。模糊距离。                         |
| *spread*   | 可选。阴影的尺寸。                       |
| *color*    | 可选。阴影的颜色。请参阅 CSS 颜色值。    |
| inset      | 可选。将外部阴影 (outset) 改为内部阴影。 |

```css
div{
  border-image:url(border.png) 30 30 round;
  -moz-border-image:url(border.png) 30 30 round; /* 老的 Firefox */
  -webkit-border-image:url(border.png) 30 30 round; /* Safari 和 Chrome */
 -o-border-image:url(border.png) 30 30 round; /* Opera */
}
```

## 背景

### background-size

> 用于控制背景图片的尺寸的大小
>
> `background-size` :    宽   高
>
> `background-size: cover;` :   
>
> ​  作用 :  将背景图片按照比例缩放到最小尺寸, 使其可以完整覆盖背景区域
>
> ​  主要作用于 : 背景图片 <  容器小
>
> `background: contian;` :
>
> ​  作用: 将背景图片按比例将图片缩放到最大尺寸, 使其高宽都在背景区域总
>
> ​  主要作用于 : 背景图片 > 容器 ( 背景图片可能会发生扭曲 )

```css
div{
  background:url(bg_flower.gif);
  -moz-background-size:63px 100px; /* 老版本的 Firefox */
  background-size:63px 100px;
  background-repeat:no-repeat;
}
```

### background-origin:规定背景图片的定位区域

> 背景图片可以放置于 content-box、padding-box 或 border-box 区域。

![](https://images.gitee.com/uploads/images/2020/0521/204114_f4fa818f_6545143.png)

```css
div{
background:url(bg_flower.gif);
background-repeat:no-repeat;
background-size:100% 100%;
-webkit-background-origin:content-box; /* Safari */
background-origin:content-box;
}
```

![](https://images.gitee.com/uploads/images/2020/0521/204314_74d8bc39_6545143.png)

```css
//多重背景图片
body{ 
background-image:url(bg_flower.gif),url(bg_flower_2.gif);
}
```

### background-clip:规定背景的绘制区域

```css
background-clip: border-box|padding-box|content-box;
```

## 文字效果

### text-shadow:文本阴影

> 水平阴影 垂直阴影 模糊距离 阴影颜色

```css
h1{
text-shadow: 5px 5px 5px #FF0000;
}
```

![](https://images.gitee.com/uploads/images/2020/0521/205537_5885dc8a_6545143.png)

### word-wrap:允许单词换行

> ```css
> word-wrap: normal|break-word;
> ```

### text-overflow:规定当文本溢出包含元素时发生的事情

```css
text-overflow: clip|ellipsis|string;
```

> clip:修剪文本
>
> ellipsis:显示省略符号来替代被修剪的文本
>
> string: 使用给定的字符串来代表修剪的文本

![](https://images.gitee.com/uploads/images/2020/0521/210327_4f5ac834_6545143.png)

## 字体

### @font-face

> 可以使用自己喜欢的任意字体

```css
@font-face{
 font-family: myFirstFont;
 src: url('Sansation_Light.ttf'),
     url('Sansation_Light.eot'); /* IE9+ */
  font-weight:bold;//可选
}

div{
font-family:myFirstFont;
}
```

| 描述符        | 值                                                           | 描述                                                         |
| :------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| font-family   | *name*                                                       | 必需。规定字体的名称。                                       |
| src           | *URL*                                                        | 必需。定义字体文件的 URL。                                   |
| font-stretch  | normal \|  condensed \| ultra-condensed\|extra-condensed \| semi-condensed   expanded \| semi-expanded \| extra-expanded \| ultra-expanded | 可选。定义如何拉伸字体。默认是 "normal"。                    |
| font-style    | ormal     italic    oblique                                  | 可选。定义字体的样式。默认是 "normal"。                      |
| font-weight   | normal   bold   100...                                       | 可选。定义字体的粗细。默认是 "normal"。                      |
| unicode-range | *unicode-range*                                              | 可选。定义字体支持的 UNICODE 字符范围。默认是 "U+0-10FFFF"。 |

## 2D转换

> 能对元素进行移动、缩放、转动、拉长或拉伸

### rotate():顺时针旋转

```css
div{
transform: rotate(30deg);
-ms-transform: rotate(30deg);  /* IE 9 */
-webkit-transform: rotate(30deg); /* Safari and Chrome */
-o-transform: rotate(30deg);  /* Opera */
-moz-transform: rotate(30deg);  /* Firefox */
}
```

> deg:单位 度

### translate(x,y):移动

> | translateX(*n*) | 定义 2D 转换，沿着 X 轴移动元素。 |
> | --------------- | --------------------------------- |
> | translateY(*n*) | 定义 2D 转换，沿着 Y 轴移动元素。 |

```css
div{
transform: translate(50px,100px);
-ms-transform: translate(50px,100px);  /* IE 9 */
-webkit-transform: translate(50px,100px); /* Safari and Chrome */
-o-transform: translate(50px,100px);  /* Opera */
-moz-transform: translate(50px,100px);  /* Firefox */
}
```

### scale(x,y):放大

> 宽放大x倍,    高扩大y倍
>
> | scaleX(*n*) | 定义 2D 缩放转换，改变元素的宽度。 |
> | ----------- | ---------------------------------- |
> | scaleY(*n*) | 定义 2D 缩放转换，改变元素的高度。 |

```css
div{
transform: scale(2,4);
-ms-transform: scale(2,4); /* IE 9 */
-webkit-transform: scale(2,4); /* Safari 和 Chrome */
-o-transform: scale(2,4); /* Opera */
-moz-transform: scale(2,4); /* Firefox */
}
```

### skew():翻转

> 值 skew(30deg,20deg) 围绕 X 轴把元素翻转 30 度，围绕 Y 轴翻转 20 度
>
> | skewX(*angle*) | 定义 2D 倾斜转换，沿着 X 轴。 |
> | -------------- | ----------------------------- |
> | skewY(*angle*) | 定义 2D 倾斜转换，沿着 Y 轴。 |

```css
div{
transform: skew(30deg,20deg);
-ms-transform: skew(30deg,20deg); /* IE 9 */
-webkit-transform: skew(30deg,20deg); /* Safari and Chrome */
-o-transform: skew(30deg,20deg); /* Opera */
-moz-transform: skew(30deg,20deg); /* Firefox */
}
```

### matrix():把所有 2D 转换方法组合在一起

```css
transform: matrix(a,b,c,d,e,f);
```

![](https://images.gitee.com/uploads/images/2020/0521/232156_7bf18ee4_6545143.png)

> ​    x  =>  `ax+cy+e`
>
> ​ y  => `bx+dy+f`

```css
div{
transform:matrix(0.866,0.5,-0.5,0.866,0,0);
-ms-transform:matrix(0.866,0.5,-0.5,0.866,0,0);  /* IE 9 */
-moz-transform:matrix(0.866,0.5,-0.5,0.866,0,0); /* Firefox */
-webkit-transform:matrix(0.866,0.5,-0.5,0.866,0,0); /* Safari and Chrome */
-o-transform:matrix(0.866,0.5,-0.5,0.866,0,0);  /* Opera */
}
```

## 3D转换

### rotateX():绕X翻转

```css
div{
  transform: rotateX(120deg);
  -webkit-transform: rotateX(120deg); /* Safari 和 Chrome */
  -moz-transform: rotateX(120deg); /* Firefox */
}
```

### rotateY():绕Y旋转

```css
div{
  transform: rotateY(130deg);
  -webkit-transform: rotateY(130deg); /* Safari 和 Chrome */
  -moz-transform: rotateY(130deg); /* Firefox */
}
```

| 函数                                                         | 描述                                      |
| :----------------------------------------------------------- | :---------------------------------------- |
| matrix3d(*n*,*n*,*n*,*n*,*n*,*n*, *n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*) | 定义 3D 转换，使用 16 个值的 4x4 矩阵。   |
| translate3d(*x*,*y*,*z*)                                     | 定义 3D 转化。                            |
| translateX(*x*)                                              | 定义 3D 转化，仅使用用于 X 轴的值。       |
| translateY(*y*)                                              | 定义 3D 转化，仅使用用于 Y 轴的值。       |
| translateZ(*z*)                                              | 定义 3D 转化，仅使用用于 Z 轴的值。       |
| scale3d(*x*,*y*,*z*)                                         | 定义 3D 缩放转换。                        |
| scaleX(*x*)                                                  | 定义 3D 缩放转换，通过给定一个 X 轴的值。 |
| scaleY(*y*)                                                  | 定义 3D 缩放转换，通过给定一个 Y 轴的值。 |
| scaleZ(*z*)                                                  | 定义 3D 缩放转换，通过给定一个 Z 轴的值。 |
| rotate3d(*x*,*y*,*z*,*angle*)                                | 定义 3D 旋转。                            |
| rotateX(*angle*)                                             | 定义沿 X 轴的 3D 旋转。                   |
| rotateY(*angle*)                                             | 定义沿 Y 轴的 3D 旋转。                   |
| rotateZ(*angle*)                                             | 定义沿 Z 轴的 3D 旋转。                   |
| perspective(*n*)                                             | 定义 3D 转换元素的透视视图。              |

## 过渡

### transition

```css
div{
  transition: width 2s, height 2s, transform 2s;
  -moz-transition: width 2s, height 2s, -moz-transform 2s;
  -webkit-transition: width 2s, height 2s, -webkit-transform 2s;
  -o-transition: width 2s, height 2s,-o-transform 2s;
}

div:hover{
  width:200px;
  height:200px;
  transform:rotate(180deg);
  -moz-transform:rotate(180deg); /* Firefox 4 */
  -webkit-transform:rotate(180deg); /* Safari and Chrome */
  -o-transform:rotate(180deg); /* Opera */
}
//实现鼠标移至元素div上,div会变成div:hover{},过渡时间为2s
```

| 属性                                                         | 描述                                         |
| :----------------------------------------------------------- | :------------------------------------------- |
| [transition](https://www.w3school.com.cn/cssref/pr_transition.asp) | 简写属性，用于在一个属性中设置四个过渡属性。 |
| [transition-property](https://www.w3school.com.cn/cssref/pr_transition-property.asp) | 规定应用过渡的 CSS 属性的名称。              |
| [transition-duration](https://www.w3school.com.cn/cssref/pr_transition-duration.asp) | 定义过渡效果花费的时间。默认是 0。           |
| [transition-timing-function](https://www.w3school.com.cn/cssref/pr_transition-timing-function.asp) | 规定过渡效果的时间曲线。默认是 "ease"。      |
| [transition-delay](https://www.w3school.com.cn/cssref/pr_transition-delay.asp) | 规定过渡效果何时开始。默认是 0。             |

## 动画

### @keyframes

> 规定某项 CSS 样式，就能创建由当前样式逐渐改为新样式的动画效果。

#### 改变背景颜色

```css
@keyframes myfirst
{
from {background: red;}
to {background: yellow;}
}

@-moz-keyframes myfirst /* Firefox */
{
from {background: red;}
to {background: yellow;}
}

@-webkit-keyframes myfirst /* Safari 和 Chrome */
{
from {background: red;}
to {background: yellow;}
}

@-o-keyframes myfirst /* Opera */
{
from {background: red;}
to {background: yellow;}
}

div{
  animation: myfirst 5s;
  -moz-animation: myfirst 5s; /* Firefox */
  -webkit-animation: myfirst 5s; /* Safari 和 Chrome */
  -o-animation: myfirst 5s; /* Opera */
}
//把 "myfirst" 动画捆绑到 div 元素，时长：5 秒：
```

#### 多次改变背景颜色

> 当动画为 25% 及 50% 时改变背景色，然后当动画 100% 完成时再次改变：

```css
@keyframes myfirst
{
0%   {background: red;}
25%  {background: yellow;}
50%  {background: blue;}
100% {background: green;}
}

@-moz-keyframes myfirst /* Firefox */
{
0%   {background: red;}
25%  {background: yellow;}
50%  {background: blue;}
100% {background: green;}
}

@-webkit-keyframes myfirst /* Safari 和 Chrome */
{
0%   {background: red;}
25%  {background: yellow;}
50%  {background: blue;}
100% {background: green;}
}

@-o-keyframes myfirst /* Opera */
{
0%   {background: red;}
25%  {background: yellow;}
50%  {background: blue;}
100% {background: green;}
}

```

#### 改变背景颜色和位置

```css
@keyframes myfirst
{
0%   {background: red; left:0px; top:0px;}
25%  {background: yellow; left:200px; top:0px;}
50%  {background: blue; left:200px; top:200px;}
75%  {background: green; left:0px; top:200px;}
100% {background: red; left:0px; top:0px;}
}

@-moz-keyframes myfirst /* Firefox */
{
0%   {background: red; left:0px; top:0px;}
25%  {background: yellow; left:200px; top:0px;}
50%  {background: blue; left:200px; top:200px;}
75%  {background: green; left:0px; top:200px;}
100% {background: red; left:0px; top:0px;}
}

@-webkit-keyframes myfirst /* Safari 和 Chrome */
{
0%   {background: red; left:0px; top:0px;}
25%  {background: yellow; left:200px; top:0px;}
50%  {background: blue; left:200px; top:200px;}
75%  {background: green; left:0px; top:200px;}
100% {background: red; left:0px; top:0px;}
}

@-o-keyframes myfirst /* Opera */
{
0%   {background: red; left:0px; top:0px;}
25%  {background: yellow; left:200px; top:0px;}
50%  {background: blue; left:200px; top:200px;}
75%  {background: green; left:0px; top:200px;}
100% {background: red; left:0px; top:0px;}
}
```

| 属性                                                         | 描述                                                     |
| :----------------------------------------------------------- | :------------------------------------------------------- |
| [@keyframes](https://www.w3school.com.cn/cssref/pr_keyframes.asp) | 规定动画。                                               |
| [animation](https://www.w3school.com.cn/cssref/pr_animation.asp) | 所有动画属性的简写属性，除了 animation-play-state 属性。 |
| [animation-name](https://www.w3school.com.cn/cssref/pr_animation-name.asp) | 规定 @keyframes 动画的名称。                             |
| [animation-duration](https://www.w3school.com.cn/cssref/pr_animation-duration.asp) | 规定动画完成一个周期所花费的秒或毫秒。默认是 0。         |
| [animation-timing-function](https://www.w3school.com.cn/cssref/pr_animation-timing-function.asp) | 规定动画的速度曲线。默认是 "ease"。                      |
| [animation-delay](https://www.w3school.com.cn/cssref/pr_animation-delay.asp) | 规定动画何时开始。默认是 0。                             |
| [animation-iteration-count](https://www.w3school.com.cn/cssref/pr_animation-iteration-count.asp) | 规定动画被播放的次数。默认是 1。                         |
| [animation-direction](https://www.w3school.com.cn/cssref/pr_animation-direction.asp) | 规定动画是否在下一周期逆向地播放。默认是 "normal"。      |
| [animation-play-state](https://www.w3school.com.cn/cssref/pr_animation-play-state.asp) | 规定动画是否正在运行或暂停。默认是 "running"。           |
| [animation-fill-mode](https://www.w3school.com.cn/cssref/pr_animation-fill-mode.asp) | 规定对象动画时间之外的状态。                             |

## 多列

#### column-count:规定元素分隔成多列

```css
//分隔成三列
div{
-moz-column-count:3;  /* Firefox */
-webkit-column-count:3; /* Safari 和 Chrome */
column-count:3;
}
```

#### column-gap:列间隔

```css
div{
-moz-column-gap:40px;  /* Firefox */
-webkit-column-gap:40px; /* Safari 和 Chrome */
column-gap:40px;
}
```

#### column-rule:列规则

```css
div{
-moz-column-rule:3px outset #ff0000; /* Firefox */
-webkit-column-rule:3px outset #ff0000; /* Safari and Chrome */
column-rule:3px outset #ff0000;
}
```

| [column-count](https://www.w3school.com.cn/cssref/pr_column-count.asp) | 规定元素应该被分隔的列数。                         |
| ------------------------------------------------------------ | -------------------------------------------------- |
| [column-fill](https://www.w3school.com.cn/cssref/pr_column-fill.asp) | 规定如何填充列。                                   |
| [column-gap](https://www.w3school.com.cn/cssref/pr_column-gap.asp) | 规定列之间的间隔。                                 |
| [column-rule](https://www.w3school.com.cn/cssref/pr_column-rule.asp) | 设置所有 column-rule-* 属性的简写属性。            |
| [column-rule-color](https://www.w3school.com.cn/cssref/pr_column-rule-color.asp) | 规定列之间规则的颜色。                             |
| [column-rule-style](https://www.w3school.com.cn/cssref/pr_column-rule-style.asp) | 规定列之间规则的样式。                             |
| [column-rule-width](https://www.w3school.com.cn/cssref/pr_column-rule-width.asp) | 规定列之间规则的宽度。                             |
| [column-span](https://www.w3school.com.cn/cssref/pr_column-span.asp) | 规定元素应该横跨的列数。                           |
| [column-width](https://www.w3school.com.cn/cssref/pr_column-width.asp) | 规定列的宽度。                                     |
| [columns](https://www.w3school.com.cn/cssref/pr_columns.asp) | 规定设置 column-width 和 column-count 的简写属性。 |

## 用户界面

### resize

```css
div{
resize:both;
overflow:auto;
}
//可有用户调整大小
```

### box-sizing

> 允许您以确切的方式定义适应某个区域的具体内容

```css
div{
box-sizing:border-box;
-moz-box-sizing:border-box; /* Firefox */
-webkit-box-sizing:border-box; /* Safari */
width:50%;
float:left;
}
```

### outline-offset

> 对轮廓进行偏移，并在超出边框边缘的位置绘制轮廓

> 规定边框边缘之外 15 像素处的轮廓：

```css
div{
border:2px solid black;
outline:2px solid red;
outline-offset:15px;
}
```

| [appearance](https://www.w3school.com.cn/cssref/pr_appearance.asp) | 允许您将元素设置为标准用户界面元素的外观           |
| ------------------------------------------------------------ | -------------------------------------------------- |
| [box-sizing](https://www.w3school.com.cn/cssref/pr_box-sizing.asp) | 允许您以确切的方式定义适应某个区域的具体内容。     |
| [icon](https://www.w3school.com.cn/cssref/pr_icon.asp)       | 为创作者提供使用图标化等价物来设置元素样式的能力。 |
| [nav-down](https://www.w3school.com.cn/cssref/pr_nav-down.asp) | 规定在使用 arrow-down 导航键时向何处导航。         |
| [nav-index](https://www.w3school.com.cn/cssref/pr_nav-index.asp) | 设置元素的 tab 键控制次序。                        |
| [nav-left](https://www.w3school.com.cn/cssref/pr_nav-left.asp) | 规定在使用 arrow-left 导航键时向何处导航。         |
| [nav-right](https://www.w3school.com.cn/cssref/pr_nav-right.asp) | 规定在使用 arrow-right 导航键时向何处导航。        |
| [nav-up](https://www.w3school.com.cn/cssref/pr_nav-up.asp)   | 规定在使用 arrow-up 导航键时向何处导航。           |
| [outline-offset](https://www.w3school.com.cn/cssref/pr_outline-offset.asp) | 对轮廓进行偏移，并在超出边框边缘的位置绘制轮廓。   |
| [resize](https://www.w3school.com.cn/cssref/pr_resize.asp)   | 规定是否可由用户对元素的尺寸进行调整。             |

## 伪类&伪元素

### :hover&:focus显示浮层

```html
<a href="javascript:;">图片链接</a>
<img src="xxx" alt="" />
<style> 
  img{
    visiblity: hidden;
    position: absolute;
    transition: visiblity .2s;
  }
  a:hover + img, img:hover{
    visibility: visible;
  }
  
  //非PC端,用这个
  a:focus + img,
  img:focus{
    visibility: visible;
    transition: none;
  }
</style>
```

### :not()判断显示元素

```css
.list:not([class="show"]) { display: none; }
```

### ::after & ::before & content

> - content 属性常常用于在元素::before和::after伪元素中插入内容
> - ::after     在指定元素后加入content
> - ::before  在指定元素前加入content

#### content

> - 值
>   - none : 不会产生伪类元素
>   - normal : `:before` 和 `:after`伪类元素会被视为none
>   - `<string>`: 文本内容
>   - `<uri> url()` :  uri会指定一个外部资源 (比如图片) , 不能正常显示就会被忽略, 或显示一些占位符(无图片标志)
>   - `<counter>`
>     - counter(计数器名 [, style])
>     - counters(计数器名称 [, string] [,style])
>   - attr(X) : 将元素的X属性以字符串的形式返回, 该元素没有该属性就会返回空字符串
>   - open-quote | close-quote : 这些值会被quotes中定义的字符串替换
>   - no-open-quote | no-close-quote : 不会产生任何内容, 但是会改变(增加或降低) 引号的层级

##### content counter

> 1. counter属性对多个项目进行连续编号, 计数器课任意命名
> 2. counter-increment属性设置counter属性值所指定的计数器名
> 3. 使用content 追加内容 coutent : '第' counter(计数器名称) '个'
> 4. 指定彪悍种类: content: counter(计数器名, 种类["upper-alpha"])
> 5. 编号嵌套需要在大标题中使用counter-reset进行编号重置

###### 插入编号

```html
<style>
    h4::before{
      content: counter(num)'.';
      color: #f00;
    }
    h4{
      counter-increment: num;
    }
</style>
<body>
    <h4>标题一</h4>
    <h4>标题二</h4>
    <h4>标题三</h4>
    <h4>标题四</h4>
    <h4>标题五</h4>
</body>
```

###### 指定编号种类

```html
<style>
    h4::before{
      content: counter(num,upper-alpha)'.';
      color: #f00;
    }
    h4{
      counter-increment: num;
    }
</style>
<body>
    <h4>标题一</h4>
    <h4>标题二</h4>
    <h4>标题三</h4>
    <h4>标题四</h4>
    <h4>标题五</h4>
</body>
```

###### 编号嵌套

```html
<style>
    h1::before{
      content: counter(num)'.';
    }
    h1{
      counter-increment: num;
      counter-reset: littenum; /* 重置小标题编号 */
    }
    h4::before{
      content: counter(littenum)'.';
    }
    h4{
      counter-increment: littenum;
      margin-left: 40px;
    }
</style>
<body>
 <h1>标题</h1>
    <h4>小标题</h4>
    <h4>小标题</h4>
    <h4>小标题</h4>
    <h1>标题</h1>
    <h4>小标题</h4>
    <h4>小标题</h4>
    <h4>小标题</h4>
    <h1>标题</h1>
    <h4>小标题</h4>
    <h4>小标题</h4>
    <h4>小标题</h4>
</body>
```

### :lang
