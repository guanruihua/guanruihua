# pointer-events

> 指定在什么情况下 (如果有) 某个特定的图形元素可以成为鼠标事件的 target

```css
/* Keyword values */
pointer-events: auto;
pointer-events: none;
pointer-events: visiblePainted; /* SVG only */
pointer-events: visibleFill;    /* SVG only */
pointer-events: visibleStroke;  /* SVG only */
pointer-events: visible;        /* SVG only */
pointer-events: painted;        /* SVG only */
pointer-events: fill;           /* SVG only */
pointer-events: stroke;         /* SVG only */
pointer-events: all;            /* SVG only */

/* Global values */
pointer-events: inherit;
pointer-events: initial;
pointer-events: unset;
```

## auto

> 与`pointer-events`属性未指定时的表现效果相同，对于SVG内容，该值与`visiblePainted`效果相同

## none

> **元素永远不会成为鼠标事件的`target`**。但是，当其后代元素的`pointer-events`属性指定其他值时，鼠标事件可以指向后代元素，在这种情况下，鼠标事件将在捕获或冒泡阶段触发父元素的事件侦听器。

## visiblePainted

> 只适用于SVG。元素只有在以下情况才会成为鼠标事件的目标：
>
> - `visibility`属性值为`visible`，且鼠标指针在元素内部，且fill属性指定了none之外的值
> - `visibility`属性值为`visible`，鼠标指针在元素边界上，且stroke属性指定了none之外的值

## visibleFill

> 只适用于SVG。只有在元素`visibility`属性值为`visible`，且鼠标指针在元素内部时,元素才会成为鼠标事件的目标，`fill`属性的值不影响事件处理。

## visibleStroke

> 只适用于SVG。只有在元素`visibility`属性值为`visible`，且鼠标指针在元素边界时,元素才会成为鼠标事件的目标，stroke属性的值不影响事件处理。

## visible

> 只适用于SVG。只有在元素`visibility`属性值为`visible`，且鼠标指针在元素内部或边界时,元素才会成为鼠标事件的目标，fill和stroke属性的值不影响事件处理。

## 　painted

> 只适用于SVG。元素只有在以下情况才会成为鼠标事件的目标：
> 鼠标指针在元素内部，且`fill`属性指定了none之外的值
> 鼠标指针在元素边界上，且`stroke`属性指定了none之外的值
> `visibility`属性的值不影响事件处理。

## fill

> 只适用于SVG。只有鼠标指针在元素内部时,元素才会成为鼠标事件的目标，`fill`和`visibilit`属性的值不影响事件处理。

## stroke

> 只适用于SVG。只有鼠标指针在元素边界上时,元素才会成为鼠标事件的目标，`stroke`和`visibility`属性的值不影响事件处理。

## all

> 只适用于SVG。只有鼠标指针在元素内部或边界时,元素才会成为鼠标事件的目标，`fill`、`stroke`和`visibility`属性的值不影响事件处理。
