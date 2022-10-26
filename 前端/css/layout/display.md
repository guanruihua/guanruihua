# display

| 值                   | 作用                                                                         |
| :------------------- | :--------------------------------------------------------------------------- |
| `none`               | 使用后元素将不会显示                                                         |
| `grid`               | 定义一个容器属性为网格布局                                                   |
| `flex`               | 定义一个弹性布局                                                             |
| `block`              | 使用后元素将变为块级元素显示，元素前后带有换行符                             |
| `inline`             | display默认值。使用后原色变为行内元素显示，前后无换行符                      |
| `list-item`          | 使用后元素作为列表显示run-in使用后元素会根据上下文作为块级元素或行内元素显示 |
| `table`              | 使用后将作为块级表格来显示（类似`<table>`），前后带有换行符                  |
| `inline-table`       | 使用后元素将作为内联表格显示（类似`<table>`），前后没有换行符                |
| `table-row-group`    | 元素将作为一个或多个行的分组来显示（类似`<tbody>`）                          |
| `table-hewder-group` | 元素将作为一个或多个行的分组来表示（类似`<thead>`）                          |
| `table-footer-group` | 元素将作为一个或多个行分组显示（类似`<tfoot>`）                              |
| `table-row`          | 元素将作为一个表格行显示（类似`<tr>`）                                       |
| `table-column-group` | 元素将作为一个或多个列的分组显示（类似`<colgroup>`）                         |
| `table-column`       | 元素将作为一个单元格列显示（类似`<col>`）                                    |
| `table-cell`         | 元素将作为一个表格单元格显示（类似`<td>`和`<th>`）                           |
| `table-caption`      | 元素将作为一个表格标题显示（类似`<caption>`）                                |
| `inherit`            | 规定应该从父元素集成display属性的值                                          |

```css
/* precomposed values */
display: block;
display: inline;
display: inline-block;
display: flex;
display: inline-flex;
display: grid;
display: inline-grid;
display: flow-root;

/* box generation */
display: none;
display: contents;

/* two-value syntax */
display: block flow;
display: inline flow;
display: inline flow-root;
display: block flex;
display: inline flex;
display: block grid;
display: inline grid;
display: block flow-root;

/* other values */
display: table;
display: table-row; /* all table elements have an equivalent CSS display value */
display: list-item;

/* Global values */
display: inherit;
display: initial;
display: revert;
display: revert-layer;
display: unset;

```
