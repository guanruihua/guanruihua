---
title:table布局  
date:2021-5-3 17:07:57  
tags：
- table
- layout
- css
---

# table

> - 基于`display:table`的布局
> - 类似于原生的`<table>`标签的使用

## 原生的table标签的说明

> - 就是使用通过使用CSS Table 来说来实现相关属性的。
> - table：指定对象作为块元素级的表格。类同于html标签<table>（CSS2）
> - inline-table：指定对象作为内联元素级的表格。类同于html标签<table>（CSS2）
> - table-caption：指定对象作为表格标题。类同于html标签<caption>（CSS2）
> - table-cell：指定对象作为表格单元格。类同于html标签<td>（CSS2）
> - table-row：指定对象作为表格行。类同于html标签<tr>（CSS2）
> - table-row-group：指定对象作为表格行组。类同于html标签<tbody>（CSS2）
> - table-column：指定对象作为表格列。类同于html标签<col>（CSS2）
> - table-column-group：指定对象作为表格列组显示。类同于html标签<colgroup>（CSS2）
> - table-header-group：指定对象作为表格标题组。类同于html标签<thead>（CSS2）
> - table-footer-group：指定对象作为表格脚注组。类同于html标签<tfoot>（CSS2）

```css
table    { display: table }
tr       { display: table-row }
thead    { display: table-header-group }
tbody    { display: table-row-group }
tfoot    { display: table-footer-group }
col      { display: table-column }
colgroup { display: table-column-group }
td, th   { display: table-cell }
caption  { display: table-caption }
```

### 使用CSS表格

> - CSS表格能够解决所有那些我们在使用绝对定位和浮动定位进行多列布局时所遇到的问题。例如，“display:table;”的CSS声明能够让一个HTML元素和它的子节点像table元素一样。
> - 使用基于表格的CSS布局，使我们能够轻松定义一个单元格的边界、背景等样式，而不会产生因为使用了table那样的制表标签所导致的语义化问题。
> - 在深入了解这种方法之前，让我们先来写份HTML文档实例：


```html
<div id="wrapper">
    <div id="main">
        <div id="nav">navigation column content…</div>
        <div id="extras">news headlines column content…</div>
        <div id="content">main article content…</div>
    </div>
</div>
```

> 这份HTML源代码满足了内容呈现方面的要求。先是导航栏，然后是附加栏，最后是内容栏。我们同样需要将以下CSS样式应用上去：


```css
#main {
    display: table;
    border-collapse: collapse;
}
#nav {
    display: table-cell;
    width: 180px;
    background-color: #e7dbcd;
}
#extras {
    display: table-cell;
    width: 180px;
    padding-left: 10px;
    border-right: 1px dotted #d7ad7b;
}
#content {
    display: table-cell;
    width: 380px;
    padding-left: 10px;
}
```
> 　　这种基于表格的新CSS布局方式能够正确的在IE8、Firefox、Safari和Opera（译者注：包括FF2/FF3/Google都通过了测试）中显示出来。我们轻松实现了三栏等高布局，而无需使用伪造背景图片之类的技巧，更不用担心定位和清除浮动的问题！


### 它是怎样实现的？

- 你可以给HTML元素指定与表格相关的display属性值，使得它们像表格元素那样渲染。以下是这些可用的display属性值：

- table 使该元素按table样式渲染
- table-row 使该元素按tr样式渲染
- table-cell 使该元素按td样式渲染
- table-row-group 使该元素按tbody样式渲染
- table-header-group 使该元素按thead样式渲染
- table-footer-group 使该元素按tfoot样式渲染
- table-caption 使该元素按caption样式渲染
- table-column 使该元素按col样式渲染
- table-column-group 使该元素按colgroup样式渲染

### 难道用table布局不是错的吗？

> - 可能你会对我们上面给出的布局实例有点不爽——毕竟，正如我自己也是一名WEB标准化的拥护者，我们不都一直坚持不应该使用table来进行布局吗？
> - table元素在HTML当中是一个包含语义的标签：它描述什么是数据。因此，你只能用它来标记那些需要制表的数据，例如一张财务信息表。如果数据能够以电子表格的形式保存在你的电脑中，那它在HTML文档中很可能需要用到table标签进行标记。从另一方面来看，display的table属性值只是声明了某些元素在浏览器中的样式——它不包含语义。如果使用table元素来进行布局，它将会告诉客户端：这些数据是制表的。使用一些display属性被设置为table和table-cell之类的div标签，除了告诉客户端以某种特定的样式来渲染它们以外，不会告诉客户端任何语义，只要客户端能够支持这些属性值。
> - 当然，我们同样还要注意，当我们真的需要制表数据的时候不要使用一大堆被声明为display:table;的div元素。
> - 我们上面的那个例子是一个简单的单行三列布局，无需费尽心思，我们就能够使用这种技术轻松实现复杂的栅格布局。

### 匿名表格元素

　　CSS表格除了包含table布局的普通规则之外，同时还有着CSS table布局的超强特性：缺少的表格元素会被浏览器以匿名方式创建。CSS2.1规范中写道：

> CSS2.1表格模型中的元素，可能不会全部包含在除HTML之外的文档语言中。这时，那些“丢失”的元素会被模拟出来，从而使得表格模型能够正常工作。所有的表格元素将会自动在自身周围生成所需的匿名table对象，使其符合table/inline-table、table-row、table-cell的三层嵌套关系。

　　这段话的意思是，如果我们为元素使用“display:table-cell;”属性，而不将其父容器设置为“display:table-row;”属性，浏览器会默认创建出一个表格行，就好像文档中真的存在一个被声明的表格行一样。

### 创建匿名表格元素的规则

> - 这些匿名的盒对象不是用魔术变出来的，它们也不会自动往你的HTML源码中添加新的标签。为了完全发挥出匿名表格元素的优势，你最好能够对创建它们的规则有所了解。如果布局中调用了匿名元素，浏览器将会根据需要创建一个匿名的盒对象并将它的CSS display属性设置为table、table-row或table-cell中的一个。
> - 如果某个元素已经被设置为“display:table-cell;”，而它的父节点（包含它的容器）没有被设置为“display:table-row;”属性，那么浏览器将会创建一个被设置为“display:table-row;”的匿名盒对象来嵌套它。并且与之相邻的属性为“display: table-cell;”的兄弟节点也都会被这个匿名盒对象所包含，直到碰到一个没有被设置为“display: table-cell;”的元素而结束这一行。以下是相关的代码样例：

```html
<div class=”cell”>CELL A</div>
<div class=”cell”>CELL B</div>
<div class=”cell”>CELL C</div>
<div>Not a cell</div>
```
> 3个`cell`的div元素被设置为`display: table-cell;`：会像一个单行三个单元格一样并排排列这。最后一个div元素则不会被包含在这一表格行当中，因为它没有被设置成`display:table-cell;`
>
> 如果某个元素被设置为“display:table-row;”，而它的父节点没有被设置为“display:table;”（或者“display:table-row-group;”），浏览器将会创建一个被设置为“display:table;”的匿名盒对象来嵌套它，与之相邻的属性为“display: table-row;”的兄弟节点也都会被包含其中。
>
> 如果某个元素被设置为“display:table-row;”，但它的内部却缺少“display:table-cell;”的元素，那么一个匿名的table-cell将会被创建，用来包含该table-row中的所有元素。请看以下代码：


```html
<div class=”row”>ROW A</div>
<div class=”row”>ROW B</div>
<div>Not a row</div>
```

> 上面两排类名为“row”的div元素被设置了“display:table-row;”属性，它们将会像单列表格中的两行一样依次排列。最后一个div元素则不会包含在这个匿名的table中。
> 
> 以此类推，如果某个元素的display属性值被设置为与表格相关的值，如table-row-group、table-header-group、 table-footer-group、table-column、table-column-group以及table-caption，但同时又没有一个被设置为“display:table;”的父元素，那么一个匿名的盒对象将会被创建用来包含该元素和它的某些兄弟节点。

### 其他有用的表格属性

> 当使用CSS表格时，因为这些元素遵从table布局的普通规则，所以你还可以给它们应用其它表格相关的CSS属性。下面是一些派得上用场的属性：
> 
> - table-layout 将table-layout属性设置为fixed可以让浏览器按照固定算法来渲染单元格的宽度。这在固定宽度布局中非常有用，例如我们最上面的那段布局代码。
> 
> - border-collapse 和普通的HTML表格一样，你可以使用border-collapse属性来定义你的table布局元素之间使用何种形式的边框，是共用边框（赋值为collapse）还是使用各自独立的边框（赋值为separate）。
> 
> - border-spacing 如果你声明了“border-collapse:separate;”，那么你就可以使用border-spacing属性来定义相邻两个单元格边框间的距离。

### 制作完美的栅格

> 制作等高栅格对于传统CSS布局技术来说已经成为一个难题，然而使用合适的CSS表格则很容易实现。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>等高栅格</title>
<style>  
.grid {
    display: table;/*表格布局*/
    border-spacing: 4px;/*单元格间距4px*/
}
.row {
    display: table-row;/*表格th布局*/
}
.image {
    display: table-cell;/*表格td布局*/
    width: 240px;
    background-color: #000;
    border: 8px solid #000;
    vertical-align: middle;
    text-align: center;
}
.image p {
    color: #fff;
    font-size: 85%;
    text-align: left;
    padding-top: 8px;
}
</style>
<body>
<div class='grid'>
    <div class='row'>
        <div class='image'><img src='images/11.png' alt='A Lily' />
        <p>A lily in the gardens of The Vyne Country House</p></div>
        <div class='image'><img src='images/11.png' alt='A Fuchsia plant' />
        <p>Fuchsia plant in my garden</p></div>
    </div>
    <div class='row'>
        <div class='image'><img src='images/11.png' alt='A crazy looking Allium flower' />
        <p>A crazy looking flower</p></div>
        <div class='image'><img src='images/11.png' alt='A Robin sitting on a fence' />
        <p>This robin has been visiting our garden over the summer.He is very friendly and doesn’t seem to be too worried about sharing the garden with us.</p></div>
    </div>
</div>
</body>
</html>
```


效果图：![img](C:\RUIHUA\Project\blog-code\source\_posts\front-end\前端基础\css\layout\table\1158910-20171016230813099-1688210396.png)

> 当然，这个我采用的是vertical-align:middle;你如果采用top对齐，那么图片就是完全一致对齐啦。

## 区别
> HTML Table和CSS Table之间的真正区别是：适当地调整CSS属性
> CSS的Table能做到许多HTML Table 不能做的事情，可以从Table中择优选择属性使用。
