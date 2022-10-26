---
title: css-expand
date: 2020-10-06 19:07:32
tags:
	- css
	- expand
---

# css-expand

## div禁用, 不可点击

> `pointer-events: none;`

## 滚动贴边 & 无视事件

> 1. 滚动贴合 ：父元素scroll-snap-type，子元素scroll-snap-align。
>
> 2. 无视事件：pointer-events: none 。

## 修改滚动条样式

demo

```css
/*滚动条的宽度*/

    ::-webkit-scrollbar {
        width:9px;
        height:9px;
    }

/*外层轨道。可以用display:none让其不显示，也可以添加背景图片，颜色改变显示效果*/

    ::-webkit-scrollbar-track {
        width: 6px;
        background-color:#0d1b20;
        -webkit-border-radius: 2em;
        -moz-border-radius: 2em;
        border-radius:2em;
    }

/*滚动条的设置*/

    ::-webkit-scrollbar-thumb {
        background-color:#606d71;
        background-clip:padding-box;
        min-height:28px;
        -webkit-border-radius: 2em;
        -moz-border-radius: 2em;
        border-radius:2em;
    }
/*滚动条移上去的背景*/

    ::-webkit-scrollbar-thumb:hover {
         background-color:#fff;
    }

```

```css
::-webkit-scrollbar 滚动条整体部分，其中的属性有width,height,background,border（就和一个块级元素一样）等。
::-webkit-scrollbar-button 滚动条两端的按钮。可以用display:none让其不显示，也可以添加背景图片，颜色改变显示效果。
::-webkit-scrollbar-track 外层轨道。可以用display:none让其不显示，也可以添加背景图片，颜色改变显示效果。
::-webkit-scrollbar-track-piece 内层轨道，滚动条中间部分（除去）。
::-webkit-scrollbar-thumb 滚动条里面可以拖动的那部分
::-webkit-scrollbar-corner 边角
::-webkit-resizer 定义右下角拖动块的样式
```

## 清除浮动

> - 1  非父元素设置高度         但这种情况 有些时候不能精确的计算出父元所需要的高度
> - 2  父元素也浮动           这又会影响父类元素的同级元素了
> - 3  父元素写 overflow:hidden    如果父元素需要 溢出显示的话  就不行了
> - 4  在父元素的后面加空的div元素   设置样式clear:both

## css盒模型

```css
//设置标准模型
box-sizing: content-box;//默认
//设置IE模型
box-sizing: border-box;
```

| 标准模型                                                     | IE模型                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| <img src="https://images.gitee.com/uploads/images/2020/0918/224618_5ce6d3cf_7984151.png" style="zoom:50%;" /> | <img src="https://images.gitee.com/uploads/images/2020/0918/224630_77d492a3_7984151.png" style="zoom:50%;" /> |

### js设置和模型的宽高

```js
//只能获取内联样式设置的宽高
dom.style.width/height

//获取渲染后即时运行的宽高，值是准确的。但只支持 IE
dom.currentStyle.width/height

//获取渲染后即时运行的宽高，值是准确的。兼容性更好
window.getComputedStyle(dom).width/height;

//获取渲染后即时运行的宽高，值是准确的。兼容性也很好，一般用来获取元素的绝对位置，getBoundingClientRect()会得到4个值：left, top, width, height
dom.getBoundingClientRect().width/height;
```

## BFC

什么是 BFC？Block Formatting Context（块级格式化上下文）。
在解释什么是BFC之前，我们需要先知道Box、Formatting Context的概念。

#### Box：css布局的基本单位

Box 是 CSS 布局的对象和基本单位， 直观点来说，就是一个页面是由很多个 Box 组成的。元素的类型和 display 属性，决定了这个 Box 的类型。 不同类型的 Box， 会参与不同的 Formatting Context（一个决定如何渲染文档的容器），因此Box内的元素会以不同的方式渲染。让我们看看有哪些盒子：

- block-level box: display 属性为 block, list-item, table 的元素，会生成 block-level box。并且参与 block fomatting context；
- inline-level box: display 属性为 inline, inline-block, inline-table 的元素，会生成 inline-level box。并且参与 inline formatting context；
- run-in box: css3 中才有， 这儿先不讲了。

#### Formatting Context

Formatting context 是 W3C CSS2.1 规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。最常见的 Formatting context 有 **Block fomatting context** (简称BFC)和 **Inline formatting context** (简称IFC)。

#### BFC的布局规则

- 内部的Box会在垂直方向，一个接一个地放置。
- Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。
- 每个盒子（块盒与行盒）的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
- BFC的区域不会与float box重叠。
- BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
- 计算BFC的高度时，浮动元素也参与计算。

#### 如何创建BFC

1. float 的值不是none。
2. position 的值不是static或者relative。
3. overflow 的值不是visible。
4. display 的值是inline-block、table-cell、flex、table-caption或者inline-flex。

#### BFC的作用

1. 利用BFC避免margin重叠
2. 自适应两栏布局
3. 清除浮动
