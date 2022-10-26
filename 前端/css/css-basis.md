---
title: css-Basis
date: 2020-09-08 22:45:04
tags: 
	- css
	- front-end
	- basis
---

# CSS-basis

## 内核前缀

> 主流浏览器内核私有属性css前缀：
>
> mozilla内核 (firefox,flock等)       -moz
> webkit内核(safari,chrome等)         -webkit
> opera内核(opera浏览器)              -o
> trident内核(ie浏览器)               -ms

## 选择器

| 属性       | 选择器                  |
| ---------- | ----------------------- |
| html元素   | 直接html标签, h1 span p |
| id         | #                       |
| class      | .                       |
| type       | [type="属性"]           |
| !important | 最高级选择器            |

### `[attr](属性)`

用法:

1. `[attr](有该属性)`
2. [attr =xxx] (属性值 是xxx)
3. `[attr^=xxx](属性是xxx开头)`
4. `[attr$=xxx](属性是xxx结尾)`
5. `[attr*=xxx](属性包含xxx)`

### 权重计算

| 选择器                    | 权重  |
| ------------------------- | ----- |
| !important                | >1000 |
| 内联(style='color: #fff') | 1000  |
| id                        | 100   |
| class                     | 10    |
| 伪类                      | 10    |
| 元素                      | 1     |
| *                         | 0     |
| 继承                      | 无    |

### 选择器的使用

> 内部样式和外部样式   采用就近原则

| 选择器         | 书写                                                         |
| -------------- | ------------------------------------------------------------ |
| 后代选择器     | `h1 span`                                                    |
| 子代选择器     | `h1>span`                                                    |
| 伪类选择器     | `:hover  :link :visted  :active`                             |
| 相邻兄弟选择器 | `span+span`                                                  |
| 通用兄弟选择器 | `p~span`( 不一定要紧跟着, 但是必须要有一个共同的父元素 )<br>`p`元素之后的所有`span`元素 |
| 通用选择器     | *                                                            |
| 群组选择器     | span, p, h1                                                  |
| 结构伪类选择器 | :first-child      : nth-child(n)    : last-child             |
| 伪元素选择器   | :after    :before                                            |

## background

background-color : #fff000 | rgb(255,255,255) | red | agba(255.255.255.0.9)

background-image : url('paper.gif')

background-repeat : no-repreat(没有平铺) | repeat-x(水平平铺) | repeat-y(垂直平铺)

background-attachment :

| 值      | 说明                                            |
| ------- | ----------------------------------------------- |
| scroll  | 背景图片随页面的其余部分滚动。这是默认          |
| fixed   | 背景图像是固定的                                |
| inherit | 指定background-attachment的设置应该从父元素继承 |
| local   | 背景图片随滚动元素滚动                          |

background-posittion : center

| 单一关键字 | 等价的关键字                   |
| :--------- | :----------------------------- |
| center     | center center                  |
| top        | top center 或 center top       |
| bottom     | bottom center 或 center bottom |
| right      | right center 或 center right   |
| left       | left center 或 center left     |

## CSS Text(文本)

### 文本颜色

> color : blue | #00ff00 | rgb(255,0,0)

### 对齐方式

> text-align : center | left | right | justify [ 每一行被展开为宽度相等，左，右外边距是对齐（如杂志和报纸）]

### 文字方向

> text-direction : ltr [左到右] | rtl [右到左] | inherit[从父元素继承direction属性值]

### 字符间距

> letter-spacing : normal(默认) |  length(数字,可以为负) | inherit(从父元素继承)

### 行高

> line-height : normal(默认) | number(行间距=当前字体尺寸*number) | lenght |  % | inherit

### 文本的首行元素 缩进

> text-indent : length(默认值: 0) | %(为父元素的百分比缩进) | inherit

### 文字阴影

> text-shadow : none(默认值) 
>
> 语法: `text-shadow: *h-shadow v-shadow blur color*;`
>
> | 值         | 描述                                                         |
> | :--------- | :----------------------------------------------------------- |
> | *h-shadow* | 必需。水平阴影的位置。允许负值。                             |
> | *v-shadow* | 必需。垂直阴影的位置。允许负值。                             |
> | *blur*     | 可选。模糊的距离。                                           |
> | *color*    | 可选。阴影的颜色。参阅 [CSS 颜色值](https://www.w3cschool.cn/cssref/css-colors-legal.html)。 |

### 元素中的字母

> text-transform :  
>
> ​  none:    默认
>
> ​  capitalize:  文本中每个单词以大写字母开头
>
> ​  uppercase:  定义仅有大写字母
>
> ​  lowercase: 定义无大写字母,仅有小写字母
>
> ​  inherit:   规定从父元素继承text-transform属性

### 设置或返回文本是否被重写

> `unicode-bidi: normal|embed|bidi-override|initial|inherit;`
>
> | 值            | 描述                                                         | 测试                                                     |
> | :------------ | :----------------------------------------------------------- | :------------------------------------------------------- |
> | normal        | 默认。不使用附加的嵌入层面。                                 | [测试 »](https://www.w3cschool.cn/css/css-css_quiz.html) |
> | embed         | 创建一个附加的嵌入层面。                                     | [测试 »](https://www.w3cschool.cn/css/css-css_quiz.html) |
> | bidi-override | 创建一个附加的嵌入层面。重新排序取决于 direction 属性。      | [测试 »](https://www.w3cschool.cn/css/css-css_quiz.html) |
> | initial       | 设置该属性为它的默认值。请参阅 [*initial*](https://www.w3cschool.cn/cssref/css-initial.html)。 | [测试 »](https://www.w3cschool.cn/css/css-css_quiz.html) |
> | inherit       | 从父元素继承该属性。请参阅 [*inherit*](https://www.w3cschool.cn/cssref/css-inherit.html)。 |                                                          |

### 元素垂直对齐

> vertical-align:
>
> | 值          | 描述                                                         |
> | ----------- | ------------------------------------------------------------ |
> | baseline    | 默认。元素放置在父元素的基线上。                             |
> | sub         | 垂直对齐文本的下标。                                         |
> | super       | 垂直对齐文本的上标                                           |
> | top         | 把元素的顶端与行中最高元素的顶端对齐                         |
> | text-top    | 把元素的顶端与父元素字体的顶端对齐                           |
> | middle      | 把此元素放置在父元素的中部。                                 |
> | bottom      | 把元素的顶端与行中最低的元素的顶端对齐。                     |
> | text-bottom | 把元素的底端与父元素字体的底端对齐。                         |
> | length      |                                                              |
> | %           | 使用 "line-height" 属性的百分比值来排列此元素。允许使用负值。 |
> | inherit     | 规定应该从父元素继承 vertical-align 属性的值。               |

### 元素中空白的处理方式

> white-space:
>
> | 值       | 描述                                                         |
> | :------- | :----------------------------------------------------------- |
> | normal   | 默认。空白会被浏览器忽略。                                   |
> | pre      | 空白会被浏览器保留。其行为方式类似 HTML 中的 <pre> 标签。    |
> | nowrap   | 文本不会换行，文本会在在同一行上继续，直到遇到 <br> 标签为止。 |
> | pre-wrap | 保留空白符序列，但是正常地进行换行。                         |
> | pre-line | 合并空白符序列，但是保留换行符。                             |
> | inherit  | 规定应该从父元素继承 white-space 属性的值。                  |

### 字间距

> word-spacing:
>
> | 值       | 描述                                         |
> | :------- | :------------------------------------------- |
> | normal   | 默认。定义单词间的标准空间。                 |
> | *length* | 定义单词间的固定空间。                       |
> | inherit  | 规定应该从父元素继承 word-spacing 属性的值。 |

## Fonts(字体)

> CSS字体属性定义字体，加粗，大小，文字样式

![](https://images.gitee.com/uploads/images/2020/0524/134419_71a084e3_6545143.png)

| 值             | 描述                                                         | 值&描述                                                      |
| :------------- | :----------------------------------------------------------- | ------------------------------------------------------------ |
| *font-style*   | 规定字体样式。                                               | normal\|italic(斜体)\|bolique(倾斜)\|inherit                 |
| *font-variant* | 规定字体异体。                                               | normal\|small-caps(显示小心大写字母的字体)\|inherit          |
| *font-weight*  | 规定字体粗细                                                 | normal\|bold(粗体)\|bolder(更粗的字符)\|lighter(更细的字符)\|100(数字)\|inherit |
| *font-size*    | 字体尺寸和行高                                               | xx-small \| x-small \| small \| medium(默认) \| large \| x-large \| xx-large\| smaller(比父元素更小的尺寸) \| larger(比父元素更大的尺寸) \| length(固定值) \| %(为父元素的一个百分值) \| inherit \| |
| *line-height*  | 行高                                                         |                                                              |
| *font-family*  | 规定字体系列。参阅：[font-family](https://www.w3cschool.cn/cssref/pr-font-font-family.html) 中可能的值。 |                                                              |
| caption        | 定义被标题控件（比如按钮、下拉列表等）使用的字体。           |                                                              |
| icon           | 定义被图标标记使用的字体。                                   |                                                              |
| menu           | 定义被下拉列表使用的字体。                                   |                                                              |
| message-box    | 定义被对话框使用的字体。                                     |                                                              |
| small-caption  | caption 字体的小型版本。                                     |                                                              |
| status-bar     | **定义被窗口状态栏使用的字体**                               |                                                              |
