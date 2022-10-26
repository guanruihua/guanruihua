# contain

`contain` 属性允许我们指定特定的 DOM 元素和它的子元素，让它们能够独立于整个 DOM 树结构之外。目的是能够让浏览器有能力只对部分元素进行重绘、重排，而不必每次都针对整个页面。

> The contain property allows an author to indicate that an element and its contents are, as much as possible, independent of the rest of the document tree. This allows the browser to recalculate layout, style, paint, size, or any combination of them for a limited area of the DOM and not the entire page.

## `contain` 语法

看看它的语法：

```CSS
{
  /* No layout containment. */
  contain: none;
  /* Turn on size containment for an element. */
  contain: size;
  /* Turn on layout containment for an element. */
  contain: layout;
  /* Turn on style containment for an element. */
  contain: style;
  /* Turn on paint containment for an element. */
  contain: paint;

  /* Turn on containment for layout, paint, and size. */
  contain: strict;
  /* Turn on containment for layout, and paint. */
  contain: content;
}
复制代码
```

除去 `none`，取值还有 6 个，我们一个一个来看看。

## contain: size

contain: size: 设定了 `contain: size` 的元素的渲染不会受到其子元素内容的影响。

> The value turns on size containment for the element. This ensures that the containing box can be laid out without needing to examine its descendants.

我开始看到这个定义也是一头雾水，光看定义很难明白到底是什么意思。还需实践一番：

假设我们有如下简单结构：

```HTML
<div class="container">
   
</div>
复制代码
.container {
    width: 300px;
    padding: 10px;
    border: 1px solid red;
}

p {
    border: 1px solid #333;
    margin: 5px;
    font-size: 14px;
}
复制代码
```

并且，借助 jQuery 实现每次点击容器添加一个 `<p>Coco</p>` 结构：

```javascript
$('.container').on('click', e => {
    $('.container').append('<p>Coco</p>')
})
复制代码
```

那么会得到如下结果：

![containsize](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ad0040e236d245649f52d199cd25ff6b~tplv-k3u1fbpfcp-watermark.awebp)

可以看到，容器 `.container` 的高度是会随着元素的增加而增加的，这是正常的现象。

此刻，我们给容器 `.container` 添加一个 `contain: size`，也就会出现上述说的：**设定了 `contain: size` 的元素的渲染不会受到其子元素内容的影响**。

```CSS
.container {
    width: 300px;
    padding: 10px;
    border: 1px solid red;
+   contain: size
}
复制代码
```

再看看会发生什么：

![containsize2](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/41c8e8578776435591bcc23a096e355b~tplv-k3u1fbpfcp-watermark.awebp)

正常而言，父元素的高度会因为子元素的增多而被撑高，而现在，子元素的变化不再影响父元素的样式布局，这就是 `contain: size` 的作用。

## contain: style

接下来再说说 `contain: style`、`contain: layout` 、`contain: paint`。先看看 contain: style。

截止至本文书写的过程中，`contain: style` 暂时被移除了。

> [CSS Containment Module Level 1](https://link.juejin.cn?target=https%3A%2F%2Fwww.w3.org%2FTR%2Fcss-contain-1%2F): Drop the at-risk “style containment” feature from this specification, move it Level 2。

嗯，官方说辞是因为存在某些风险，暂时被移除，可能在规范的第二版会重新定义吧，那这个属性也暂且放一放。

## contain: paint

contain: paint：设定了 `contain: paint` 的元素即是设定了布局限制，也就是说告知 User Agent，此元素的子元素不会在此元素的边界之外被展示，因此，如果元素不在屏幕上或以其他方式设定为不可见，则还可以保证其后代不可见不被渲染。

> This value turns on paint containment for the element. This ensures that the descendants of the containing box don’t display outside its bounds, so if an element is off-screen or otherwise not visible, its descendants are also guaranteed to be not visible.

这个稍微好理解一点，先来看第一个特性：

### 设定了 `contain: paint` 的元素的子元素不会在此元素的边界之外被展示

- 设定了 `contain: paint` 的元素的子元素不会在此元素的边界之外被展示

这个特点有点类似与 `overflow: hidden`，也就是明确告知用户代理，子元素的内容不会超出元素的边界，所以超出部分无需渲染。

简单示例，假设元素结构如下：

```HTML
<div class="container">
    <p>Coco</p>
</div>
复制代码
.container {
    contain: paint;
    border: 1px solid red;
}

p{
    left: -100px;
}
复制代码
```

我们来看看，设定了 `contain: paint` 与没设定时会发生什么：

![containsize3](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e1edabb7b354493cb0ed38a5c03b9db6~tplv-k3u1fbpfcp-watermark.awebp)

[CodePen Demo -- contain: paint Demo](https://link.juejin.cn?target=https%3A%2F%2Fcodepen.io%2FChokcoco%2Fpen%2FKKwmgmN)

### 设定了 `contain: paint` 的元素在屏幕之外时不会渲染绘制

通过使用 `contain: paint`， 如果元素处于屏幕外，那么用户代理就会忽略渲染这些元素，从而能更快的渲染其它内容。

## contain: layout

contain: layout：设定了 `contain: layout` 的元素即是设定了布局限制，也就是说告知 User Agent，此元素内部的样式变化不会引起元素外部的样式变化，反之亦然。

> This value turns on layout containment for the element. This ensures that the containing box is totally opaque for layout purposes; nothing outside can affect its internal layout, and vice versa.

启用 `contain: layout` 可以潜在地将每一帧需要渲染的元素数量减少到少数，而不是重新渲染整个文档，从而为浏览器节省了大量不必要的工作，并显着提高了性能。

使用 `contain：layout`，开发人员可以指定对该元素任何后代的任何更改都不会影响任何外部元素的布局，反之亦然。

因此，浏览器仅计算内部元素的位置（如果对其进行了修改），而其余DOM保持不变。因此，这意味着帧渲染管道中的布局过程将加快。

### 存在的问题

描述很美好，但是在实际 Demo 测试的过程中（截止至2021/04/27，Chrome 90.0.4430.85），仅仅单独使用 `contain：layout` 并没有验证得到上述那么美好的结果。

**设定了 `contain: layout` 的指定元素，改元素的任何后代的任何更改还是会影响任何外部元素的布局**，点击红框会增加一条 `<p>Coco<p>` 元素插入到 `container` 中：

简单的代码如下：

```HTML
<div class="container">
    <p>Coco</p>
    ...
</div>
<div class="g-test"></div>
复制代码
html,
body {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    gap: 10px;
}

.container {
    width: 150px;
    padding: 10px;
    contain: layout;
    border: 1px solid red;
}

.g-test {
    width: 150px;
    height: 150px;
    border: 1px solid green;
}
复制代码
```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/02c6ecc821d845e593faa7c8f83e1550~tplv-k3u1fbpfcp-watermark.awebp)

[CodePen Demo -- contain: layout Demo](https://link.juejin.cn?target=https%3A%2F%2Fcodepen.io%2FChokcoco%2Fpen%2FrNjRELL)

目前看来，`contain: layout` 的实际作用不那么明显，更多的关于它的用法，你可以再看看这篇文章：[CSS-tricks - contain](https://link.juejin.cn?target=https%3A%2F%2Fcss-tricks.com%2Falmanac%2Fproperties%2Fc%2Fcontain%2F)

## contain: strict | contain: content

这两个属性稍微有点特殊，效果是上述介绍的几个属性的聚合效果：

- `contain: strict`：同时开启 layout、style、paint 以及 size 的功能，它相当于 `contain: size layout paint`
- `contain: content`：同时开启 layout、style 以及 paint 的功能，它相当于 `contain: layout paint`

所以，这里也提一下，contain 属性是可以同时定义几个的。

## Can i Use -- CSS Contain

截止至 2021-04-27，Can i Use  上的 CSS Contain 兼容性，已经可以开始使用起来：

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c34ec138a05b4cc6916aad01600ed651~tplv-k3u1fbpfcp-watermark.awebp)

## 参考文献

- [CSS Containment Module Level 1](https://link.juejin.cn?target=https%3A%2F%2Fwww.w3.org%2FTR%2Fcss-contain-1%2F)
- [CSS containment](https://link.juejin.cn?target=https%3A%2F%2Fjustmarkup.com%2Farticles%2F2016-04-05-css-containment%2F)
- [CSS Containment in Chrome 52](https://link.juejin.cn?target=https%3A%2F%2Fdevelopers.google.com%2Fweb%2Fupdates%2F2016%2F06%2Fcss-containment)