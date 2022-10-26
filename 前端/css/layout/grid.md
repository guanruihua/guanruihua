# grid布局

## 网格布局

```css
display: grid | inline-grid; //行列布局

grid-template-columns: 200px 200px 200px; | repeat(3, 200px) | repeat(auto-fill, 200px)[表示列宽200px, 只要能容纳得下就可以放置] | 200px 2fr 1fr [后面两项分别占有剩下的2/3,1/3] | 1fr 1fr minmax(300px, 2fr)[第三列:300px<=x<=2fr]
grid-template-rows: 200px 200px 200px; | repeat(3, 200px) | 和上个类似

//行列间隔
grid-row-gap:10px;
grid-column-gap:20px;
grid-gap:10px 20px;[等同于上两行]
row-gap: 10px;
column-gap: 20px;

//定义区域
grid-template-areas:
". header  header"[.表示空单元格]
"sidebar content content";[这里定义的六个区域]
 //子元素
.header{
  grid-area: header;
}

//控制自动布局
grid-auto-flow: row | column | row dense | column dense;[dense:自动填补空白;row: 表示先行后列]

//控制单元格位置
justify-items:start | end | center | stretch[拉伸];[水平方向]
align-items:start | end | center | stretch;[垂直方向]
place-items:[jusity-item] [align-items];//同时设置水平和垂直方向

//这个是对于单个单元格
justify-self:start | end | center | stretch[拉伸];[水平方向]
align-self:start | end | center | stretch;[垂直方向]
place-self:[jusity-item] [align-items];//同时设置水平和垂直方向

//控制内容区域在容器的位置
justify-content: start | end | center | stretch | 
   space-around[每个项目两侧距离相等,项目间间隔比到容器间隙大一倍] | 
   space-between[项目间隔相等,与容器边框没有间隙] | 
   space-evenly[项目与项目的间隙相等, 项目间与容器边框间间隙相等];
align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
place-content: [justify-content] [align-content]

//定义隐式网格属性[当超出grid-template-xxx的定义就会触发]
grid-auto-columns: 50px;
grid-auto-rows: 50px;

//网格项目所在的四个边框
grid-column-start 属性：左边框所在的垂直网格线
grid-column-end 属性：右边框所在的垂直网格线
grid-row-start 属性：上边框所在的水平网格线
grid-row-end 属性：下边框所在的水平网格线

//  合并单元格(可以使用 grid-template-areas 来定义重复的名字可以达到效果)
// 'span 3': 合并往下三个单元格; auto: 表示再原有位置进行合并, 也可以指定其他行
grid-row: auto / span 3;
// 'span 2': 合并往右边2个单元格; auto: 表示再原有位置进行合并, 也可以指定其他列
grid-column: auto / span 2;

```

## display 属性

> - `display：grid`  
>
> - `display：inline-grid`

## grid-template-columns / rows

> - `grid-template-columns` 属性设置每列宽
>
> - `grid-template-rows` 属性设置每行高

**固定的列宽和行高**

```css
.wrapper {
  display: grid;
  /*  声明了三列，宽度分别为 200px 100px 200px */
  grid-template-columns: 200px 100px 200px;
  grid-gap: 5px;
  /*  声明了两行，行高分别为 50px 50px  */
  grid-template-rows: 50px 50px;
}
```

> **repeat() 函数**：可以简化重复的值。
>
> - 第一个参数是重复的次数
> - 第二个参数是所要重复的值

```scss
.wrapper-1 {
  display: grid;
  grid-template-columns: 200px 100px 200px;
  grid-gap: 5px;
  /*  2行，而且行高都为 50px  */
  grid-template-rows: repeat(2, 50px);
}
```

> **auto-fill 关键字**：表示自动填充，让一行（或者一列）中尽可能的容纳更多的单元格。`grid-template-columns: repeat(auto-fill, 200px)` 表示列宽是 200 px，但列的数量是不固定的，只要浏览器能够容纳得下，就可以放置元素

```css
.wrapper-2 {
  display: grid;
  grid-template-columns: repeat(auto-fill, 200px);
  grid-gap: 5px;
  grid-auto-rows: 50px;
}
```

> **fr 关键字**：`Grid` 布局还引入了一个另外的长度单位来帮助我们创建灵活的网格轨道。`fr` 单位代表网格容器中可用空间的一等份。`grid-template-columns: 200px 1fr 2fr` 表示第一个列宽设置为 200px，后面剩余的宽度分为两部分，宽度分别为剩余宽度的 1/3 和 2/3。

```css
.wrapper-3 {
  display: grid;
  grid-template-columns: 200px 1fr 2fr;
  grid-gap: 5px;
  grid-auto-rows: 50px;
}
```

**minmax() 函数**：我们有时候想给网格元素一个最小和最大的尺寸，`minmax()` 函数产生一个长度范围，表示长度就在这个范围之中都可以应用到网格项目中。它接受两个参数，分别为最小值和最大值。`grid-template-columns: 1fr 1fr minmax(300px, 2fr)` 的意思是，第三个列宽最少也是要 300px，但是最大不能大于第一第二列宽的两倍。代码以及效果如下：

```css
.wrapper-4 {
  display: grid;
  grid-template-columns: 1fr 1fr minmax(300px, 2fr);
  grid-gap: 5px;
  grid-auto-rows: 50px;
}
```

**auto 关键字**：由浏览器决定长度。通过 `auto` 关键字，我们可以轻易实现三列或者两列布局。`grid-template-columns: 100px auto 100px` 表示第一第三列为 100px，中间由浏览器决定长度，代码以及效果如下：

```css
.wrapper-5 {
  display: grid;
  grid-template-columns: 100px auto 100px;
  grid-gap: 5px;
  grid-auto-rows: 50px;
}
```

## gap(间隙)

> `grid-row-gap` 属性、`grid-column-gap` 属性分别设置行间距和列间距。
>
> `grid-gap` 属性是两者的简写形式。(已经弃用)
> `column-gap` 和 `row-gay` 替代

```css
.wrapper {
  display: grid;
  grid-template-columns: 200px 100px 100px;
  grid-gap: 10px 20px;
  grid-auto-rows: 50px;
}
```

```
.wrapper-1 {
  display: grid;
  grid-template-columns: 200px 100px 100px;
  grid-auto-rows: 50px;
  grid-row-gap: 10px;
  grid-column-gap: 20px;
}
```

## grid-template-areas/grid-area

> `grid-template-areas` 属性用于定义区域，一个区域由一个或者多个单元格组成
>
> `grid-area` : 指定放在哪个区域

```css
.wrapper {
  display: grid;
  grid-gap: 10px;
  grid-template-columns: 120px  120px  120px;
  grid-template-areas:
    ". header  header"
    "sidebar content content";
  background-color: #fff;
  color: #444;
}
```

> 上面代码表示划分出 6 个单元格，其中值得注意的是 `.` 符号代表空的单元格，也就是没有用到该单元格。

```css
.sidebar {
  grid-area: sidebar;
}

.content {
  grid-area: content;
}

.header {
  grid-area: header;
}
```

以上代码表示将类 `.sidebar` `.content` `.header`所在的元素放在上面 `grid-template-areas` 中定义的 `sidebar` `content` `header` 区域中

## grid-auto-flow

> `grid-auto-flow` 属性控制着自动布局算法怎样运作，精确指定在网格中被自动布局的元素怎样排列。默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二行，即下图英文数字的顺序 `one`,`two`,`three`...。这个顺序由 `grid-auto-flow` 属性决定，默认值是 `row`。

### row

```css
.wrapper {
  display: grid;
  grid-template-columns: 100px 200px 100px;
  grid-auto-flow: row;
  grid-gap: 5px;
  grid-auto-rows: 50px;
}
```

### dense

> `grid-auto-flow: row dense` => dense 会用后面的项目填充前面的空位

```css
.wrapper-2 {
  display: grid;
  grid-template-columns: 100px 200px 100px;
  grid-auto-flow: row dense;
  grid-gap: 5px;
  grid-auto-rows: 50px;
}
```

### column

可以设置 `grid-auto-flow: column`，表示先列后行，代码以及效果如下图所示：

```css
.wrapper-1 {
  display: grid;
  grid-auto-columns: 100px;
  grid-auto-flow: column;
  grid-gap: 5px;
  grid-template-rows:  50px 50px;
}
```

## 项目对齐(item)

> - `justify-items` : 设置单元格内容的水平位置（左中右）
>
> - `align-items`  : 设置单元格的垂直位置（上中下）
> - `place-items : [align-items]  | [justify-items]`  

```css
.container {
  justify-items: start | end | center | stretch;
  align-items: start | end | center | stretch;
}
```

```css
.wrapper, .wrapper-1, .wrapper-2, .wrapper-3 {
  display: grid;
  grid-template-columns: 100px 200px 100px;
  grid-gap: 5px;
  grid-auto-rows: 50px;
  justify-items: start;
}
.wrapper-1 {
  justify-items: end;
}
.wrapper-2 {
  justify-items: center;
}
.wrapper-3 {
  justify-items: stretch;
}
```

### start

- start：对齐单元格的起始边缘

### end

> 对齐单元格的结束边缘

### center

> center：单元格内部居中

### stretch

> 拉伸，占满单元格的整个宽度（默认值）

## justify-content ,align-content ,place-content

> `justify-content` 属性是整个内容区域在容器里面的水平位置（左中右）
>
> `align-content` 属性是整个内容区域的垂直位置（上中下）。它们都有如下的属性值。

```css
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
  align-content: start | end | center | stretch | space-around | space-between | space-evenly; 
  place-content:  [align-content] [justify-content];
}
```

```css
.wrapper, .wrapper-1, .wrapper-2, .wrapper-3, .wrapper-4, .wrapper-5, .wrapper-6 {
  display: grid;
  grid-template-columns: 100px 200px 100px;
  grid-gap: 5px;
  grid-auto-rows: 50px;
  justify-content: start;
}
.wrapper-1 {
  justify-content: end;
}
.wrapper-2 {
  justify-content: center;
}
```

- space-around - 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍
- space-between - 项目与项目的间隔相等，项目与容器边框之间没有间隔
- space-evenly - 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔
- stretch - 项目大小没有指定时，拉伸占据整个网格容器

```css
.wrapper-3 {
  justify-content: space-around;
}
.wrapper-4 {
  justify-content: space-between;
}
.wrapper-5 {
  justify-content: space-evenly;
}
.wrapper-6 {
  justify-content: stretch;
}
```

## grid-auto-columns, grid-auto-rows

> - 显式网格 : 包含了你在 `grid-template-columns` 和 `grid-template-rows` 属性中定义的行和列。
>
> - 如果你在网格定义之外又放了一些东西，或者因为内容的数量而需要的更多网格轨道的时候，**网格将会在隐式网格中创建行和列**
> - 不指定这隐式网格属性，浏览器完全根据单元格内容的大小，决定新增网格的列宽和行高

```css
.wrapper {
  display: grid;
  grid-template-columns: 200px 100px;
/*  只设置了两行，但实际的数量会超出两行，超出的行高会以 grid-auto-rows 算 */
  grid-template-rows: 100px 100px;
  grid-gap: 10px 20px;
  grid-auto-rows: 50px;
}
```

`grid-template-columns` 属性和 `grid-template-rows` 属性只是指定了两行两列，但实际有九个元素，就会产生隐式网格。通过 `grid-auto-rows` 可以指定隐式网格的行高为 50px

## 项目属性介绍

### grid-column-start /end 、grid-row-start / end

> 可以指定网格项目所在的四个边框，分别定位在哪根网格线，从而指定项目的位置
>
> - grid-column-start 属性：左边框所在的垂直网格线
> - grid-column-end 属性：右边框所在的垂直网格线
> - grid-row-start 属性：上边框所在的水平网格线
> - grid-row-end 属性：下边框所在的水平网格线

```css
.wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 20px;
  grid-auto-rows: minmax(100px, auto);
}
.one {
  grid-column-start: 1;
  grid-column-end: 2;
  background: #19CAAD;
}
.two { 
  grid-column-start: 2;
  grid-column-end: 4;
  grid-row-start: 1;
  grid-row-end: 2;
  /*   如果有重叠，就使用 z-index */
  z-index: 1;
  background: #8CC7B5;
}
.three {
  grid-column-start: 3;
  grid-column-end: 4;
  grid-row-start: 1;
  grid-row-end: 4;
  background: #D1BA74;
}
.four {
  grid-column-start: 1;
  grid-column-end: 2;
  grid-row-start: 2;
  grid-row-end: 5;
  background: #BEE7E9;
}
.five {
  grid-column-start: 2;
  grid-column-end: 2;
  grid-row-start: 2;
  grid-row-end: 5;
  background: #E6CEAC;
}
.six {
  grid-column: 3;
  grid-row: 4;
  background: #ECAD9E;
}
```

上面代码中，类 `.two` 所在的网格项目，垂直网格线是从 2 到 4，水平网格线是从 1 到 2。其中它跟 `.three` （垂直网格线是从3 到 4，水平网格线是从 1 到 4） 是有冲突的。可以设置 `z-index` 去决定它们的层级关系

### justify / align / place -self

> - `justify-self` 属性设置单元格内容的水平位置（左中右），跟 `justify-items` 属性的用法完全一致，但只作用于单个项目
>
> - `align-self` 属性设置单元格内容的垂直位置（上中下），跟``align-items`属性的用法完全一致，也是只作用于单个项目

```css
.item {
  justify-self: start | end | center | stretch;
  align-self: start | end | center | stretch;
}

.item {
  justify-self: start;
}
.item-1 {
  justify-self: end;
}
.item-2 {
  justify-self: center;
}
.item-3 {
  justify-self: stretch;
}
```

- start：对齐单元格的起始边缘

- end：对齐单元格的结束边缘

- center：单元格内部居中

- stretch：拉伸，占满单元格的整个宽度（默认值）

## Grid 布局兼容性

最后，聊聊 `Grid` 布局兼容性问题，在 [caniuse](https://caniuse.com/#search=grid) 中，我们可以看到的结果如下，总体兼容性还不错，但在 IE 10 以下不支持。个人建议在公司的内部系统运用起来是没有问题的，但 TOC 的话，可能目前还是不太合适

|                   属性                   |            描述            |
| :--------------------------------------: | :------------------------: |
|                 display                  | grid      \|   inline-grid |
| grid-template-columns/grid-template-rows |       设置列宽/行高        |
