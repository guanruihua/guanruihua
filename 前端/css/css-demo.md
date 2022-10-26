---
title: css-demo
date: 2020-09-08 23:57:12
tags: 
	- css
	- demo
	- front-end
---

# css-demo

## datalist,progress,audio,video

未完成



## 水平垂直居中

#### absolute + 负margin

这种方式比较好理解，兼容性也很好，缺点是需要知道子元素的宽高

```html
<div class="out">
  <div class="inner">12345</div>
</div>

<style type="text/css">
  .out{
    position: relative;
    width: 300px;
    height: 300px;
    background: red;
  }

  .inner{
    position: absolute;
    width: 100px;
    height: 100px;
    background: yellow;
    left: 50%;
    top: 50%;
    margin-left: -50px;
    margin-top: -50px;
  }
</style>
```

#### absolute + auto margin

这种方法兼容性也很好，缺点是需要知道子元素的宽高

```html
<style type="text/css">
  .out{
    position: relative;
    width: 300px;
    height: 300px;
    background: red;
  }

  .inner{
    position: absolute;
    width: 100px;
    height: 100px;
    background: yellow;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
    margin: auto;
  }
</style>
```

#### absolute + calc

这种方法的兼容性依赖于 calc，且也需要知道宽高

```html
<style type="text/css">
  .out{
    position: relative;
    width: 300px;
    height: 300px;
    background: red;
  }

  .inner{
    position: absolute;
    width: 100px;
    height: 100px;
    background: yellow;
    left: calc(50% - 50px);
    top: calc(50% - 50px);
  }
</style>
```

#### absolute + transform

兼容性依赖 translate，不需要知道子元素宽高

```html
<style type="text/css">
  .out{
    position: relative;
    width: 300px;
    height: 300px;
    background: red;
  }

  .inner{
    position: absolute;
    background: yellow;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
  }
</style>
```

#### table

css新增的table属性，可以让我们把普通元素，变为table元素的显示效果，通过这个特性也可以实现水平垂直居中。
这种方法兼容性也不错。

```html
<style type="text/css">
  .out{
    display: table-cell;
    width: 300px;
    height: 300px;
    text-align: center;
    vertical-align: middle;
    background: red;
  }

  .inner{
    display: inline-block;
    background: yellow;
    width: 100px;
    height: 100px;
  }
</style>
```

#### flex

flex 实现起来比较简单，三行代码即可搞定。可通过父元素指定子元素的对齐方式，也可通过 子元素自己指定自己的对齐方式来实现。第二种方式见 grid 布局。

```html
<style type="text/css">
  .out{
    display: flex;
    justify-content: center;
    align-items: center;
    width: 300px;
    height: 300px;
    background: red;
  }

  .inner{
    background: yellow;
    width: 100px;
    height: 100px;
  }
</style>
```

#### grid

grid 布局也很强大，大体上属性跟 flex 差不多。

```html
//方法一：父元素指定子元素的对齐方式
<style type="text/css">
  .out{
    display: grid;
    align-content: center;
    justify-content: center;
    width: 300px;
    height: 300px;
    background: red;
  }

  .inner{
    background: yellow;
    width: 100px;
    height: 100px;
  }
</style>

//方法二：子元素自己指定自己的对齐方式
<style type="text/css">
  .out{
    display: grid;
    width: 300px;
    height: 300px;
    background: red;
  }

  .inner{
    background: yellow;
    width: 100px;
    height: 100px;
    align-self: center;
    justify-self: center;
  }
</style>
```

## 多行文本实现移除显示省略号

单行

```css
overflow:hidden;
text-overflow: ellipsis;
white-space: nowrap;
```

webkit浏览器或移动端的页面

多行

```css
overflow : hidden;
text-overflow: ellipsis;//显示省略号
display: -webkit-box;//设置对象为弹性盒子模型显示
-webkit-line-clamp: 2;//限制一个块元素显示的文本的行数
-webkit-box-orient: vertical;//必须结合的属性,设置或检索伸缩和对象
```

跨浏览器兼容的方案

```css
p {
    position:relative;
    line-height:1.4em;
    /* 3 times the line-height to show 3 lines */
    height:4.2em;
    overflow:hidden;
}
p::after {
    content:"...";
    font-weight:bold;
    position:absolute;
    bottom:0;
    right:0;
    padding:0 20px 1px 45px;
    background:url(/newimg88/2014/09/ellipsis_bg.png) repeat-y;
}
```



## 更改有序列表的样式

有序列表

```html
<ol>
  <li>第一行</li>
  <li>第二行</li> 
  <li>第三行</li> 
  <li>第四行</li> 
</ol>
     
```

效果：

```
1.第一行
2.第二行
3.第三行
4.第四行
```

新的需求效果

```css
1、第一行
2、第二行
3、第三行
4、第四行
//使用:before伪类
//样式：
ol{
  counter-reset: num;
}
ol li:after{
  content: counter(num)"、";
  counter-increment: num;
}
// 结构
<ol>
  <li>第一行</li>
  <li>第二行</li>
  <li>第三行</li>
  <li>第四行</li>
</ol>
```



## 修改谷歌没浏览器滚动条的样式

```css
/*定义滚动条高宽及背景 高宽分别对应横竖滚动条的尺寸*/
::-webkit-scrollbar {
  width: 8px;
  background-color: #fff;
  z-index: 11;
}

/*定义滚动条轨道 内阴影+圆角*/
::-webkit-scrollbar-track {
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  border-radius: 10px;
  background-color: #ededed;
}

/*定义滑块 内阴影+圆角*/
::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background-color: #46cdff;
  background-image: -webkit-linear-gradient(
    45deg,
    rgba(255, 255, 255, 0.2) 25%,
    transparent 25%,
    transparent 50%,
    rgba(255, 255, 255, 0.2) 50%,
    rgba(255, 255, 255, 0.2) 75%,
    transparent 75%,
    transparent
  );
}

/*定义最上方和最下方的按钮*/
::-webkit-scrollbar-button {
  background-color: #000;
  border: 1px solid yellow;
  display: none;
}
```



## 元素隐藏

```
display: none; 设置为隐藏, 且不会渲染, 不会占据空间
visibility: hidden; 设置为隐藏, 但是还是会占用空间
rgba() 或opacity 改变其透明度为1 , 也同样可以达到元素隐藏的效果
```



## CSS属性默认值

```
border-top-width:默认值medium,定义中等的上边框。
outline-width:默认值medium,规定中等的轮廓。
padding-top, margin-top默认值都是0;
```

## max-device-width 与 max-width 区别

**max-width 指的是显示区域的宽度，比如浏览器的显示区域宽度**

（max-width is the width of the target display area, e.g. the browser）

**max-device-width 指的是设备整个渲染（显示）区域的宽度，比如设备的实际屏幕大小，也就是设备分辨率** 

## FF2.0

> 父元素不能支持自适应子元素的高度

**Mozilla Firefox 2**是**Firefox**的版本的问题,



## 置换元素,不可替换元素

a) 置换元素：浏览器根据元素的标签和属性，来决定元素的具体显示内容。 
例如：浏览器会根据<img>标签的src属性的 值来读取图片信息并显示出来，而如果查看(x)html代码，则看不到图片的实际内容；<input>标签的type属性来决定是显示输入 框，还是单选按钮等。 (x)html中 的<img>、<input>、<textarea>、<select>、<object> 都是置换元素。这些元素往往没有实际的内容，即是一个空元素。

置换元素在其显示中生成了框，这也就是有的内联元素能够设置宽高的原因。

b) 不可替换元素：(x)html 的大多数元素是不可替换元素，即其内容直接表现给用户端（如浏览器）。

例如： <label>label中的内容</label> 标签<label>是一个非置换元素，文字label中的内容”将全被显示。

## 默认支持跨域的标签

> image, iframe,img,

## fieldset>legend

```html
<fieldset>
  <legend>类型名</legend>
  内容显示
</fieldset>
```

## 预备格式化标签

```html
<pre>定义预格式文本，保持文本原有的格式
<meta> 元素可提供有关页面的元信息（meta-information），比如针对搜索引擎和更新频度的描述和关键词。
<meta> 标签位于文档的头部，不包含任何内容。
<meta> 标签的属性定义了与文档相关联的名称/值对。

<mark> 标签定义带有记号的文本。请在需要突出显示文本时使用 <m> 标签。
eg:<mark>milk</mark>   
       milk 

<small>标签呈现小号字体效果。
```

## 修改radio和checkbox的样式

- 无法通过 css 样式来直接修改原生的 radio 和 checkbox 

- 通过 label 和 radio 或checkbox 的绑定关系, 让label 当做 checkbox 来显示

  ```pug
  // pug
  
  input( type='checkbox',id='mycheck')
  label(for='mycheck')
  
  input(type='radio',id='myradio')
  label(for='myradio')
  ```

```css
  // css
  input[type='radio'], input[type='checkbox'] {
    display: none !important;
  }
  input[type='checkbox']+label {
    background-color: #fff;
    border-radius: 5px;
    border: 1px solid #d3d3d3;
    width: 20px;
    height: 20px;
    display: inline-block;
    text-align: center;
    vertical-align: middle;
    line-height: 20px;
  }
  // 通过checked状态 , 再通过兄弟组件 替换checkbox 的label
  input[type='checkbox']:checked+label {
    background-color: #c0c0c0;
  }
  input[type='radio']:checked+label {
    background-color: #c0c0c0;
  }
  // 通过after 和 context 来替换 radio中点  
  #myCheck:checked+label:after {
    content: "\2714";  // √ 
    display: block;
    width: 20px;
    height: 20px;
  }
```

  

## div三角形

```css
*{margin: 0; padding: 0}
.content{
	width:0;
	height:0;
	margin: 0 auto;
	border-width:20px;
	border-style:solid;
	border-color:transparent transparent pink transparent;
}

.content2{
	width: 0;
  height:0;
  margin:0 auto;
  border:50px solid transparent;
  border-top: 50px solid pink;
	
}
```

