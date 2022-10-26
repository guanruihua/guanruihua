# @media

`@media` [CSS](https://developer.mozilla.org/en/CSS) [@规则](https://developer.mozilla.org/zh-CN/docs/Web/CSS/At-rule) 可用于基于一个或多个 [媒体查询](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries)的结果来应用样式表的一部分。 使用它，您可以指定一个媒体查询和一个CSS块，当且仅当该媒体查询与正在使用其内容的设备匹配时，该CSS块才能应用于该文档。

**注：** 在JavaScript中，可以使用[CSSMediaRule](https://developer.mozilla.org/zh-CN/docs/Web/API/CSSMediaRule)CSS对象模型接口访问使用@media创建的规则。

## [语法](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media#语法)

`@media` 规则可置于您代码的顶层或位于其它任何[@条件规则组](https://developer.mozilla.org/zh-CN/docs/Web/CSS/At-rule#条件规则组)内。

```css
/* At the top level of your code */
@media screen and (min-width: 900px) {
  article {
    padding: 1rem 3rem;
  }
}

/* Nested within another conditional at-rule */
@supports (display: flex) {
  @media screen and (min-width: 900px) {
    article {
      display: flex;
    }
  }
}
```

### [正式语法](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media#正式语法)

```css
@media <media-query-list> {
  <group-rule-body>
}
where <media-query-list> = <media-query>#

where <media-query> = <media-condition> | [ not | only ]? <media-type> [ and 
<media-condition-without-or> ]?

where <media-condition> = <media-not> | <media-and> | <media-or> | <media-in-parens><media-type> = <ident> (en-US)<media-condition-without-or> = <media-not> | <media-and> | <media-in-parens>

where <media-not> = not <media-in-parens><media-and> = <media-in-parens> [ and <media-in-parens> ]+<media-or> = <media-in-parens> [ or <media-in-parens> ]+<media-in-parens> = ( <media-condition> ) | <media-feature> | <general-enclosed>

where <media-feature> = ( [ <mf-plain> | <mf-boolean> | <mf-range> ] )<general-enclosed> = [ <function-token> <any-value> ) ] | ( <ident> (en-US) <any-value> )

where <mf-plain> = <mf-name> : <mf-value><mf-boolean> = <mf-name><mf-range> = <mf-name> [ '<' | '>' ]? '='? <mf-value> | <mf-value> [ '<' | '>' ]? '='? <mf-name> | <mf-value> '<' '='? <mf-name> '<' '='? <mf-value> | <mf-value> '>' '='? <mf-name> '>' '='? <mf-value>

where <mf-name> = <ident> (en-US)<mf-value> = <number> | <dimension> | <ident> (en-US) | <ratio>
```

## [媒体特性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media#媒体特性)

*媒体特性*（*Media features*）描述了 [user agent](https://developer.mozilla.org/zh-CN/docs/Glossary/User_agent)、输出设备，或是浏览环境的具体特征。媒体特性表达式是完全可选的，它负责测试这些特性或特征是否存在、值为多少。每条媒体特性表达式都必须用括号括起来。

| 名称                                                                                                       | 简介                                                                                                                           | 备注                                                                                             |
| :--------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------- |
| [any-hover](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media/any-hover)                             | 是否有任何可用的输入机制允许用户（将鼠标等）悬停在元素上？                                                                     | 在 Media Queries Level 4 中被添加。                                                              |
| [any-pointer](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media/any-pointer)                         | 可用的输入机制中是否有任何指针设备，如果有，它的精度如何？                                                                     | 在 Media Queries Level 4 中被添加。                                                              |
| [aspect-ratio](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media/aspect-ratio)                       | 视窗（viewport）的宽高比                                                                                                       |                                                                                                  |
| [color (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/color)                             | 输出设备每个像素的比特值，常见的有 8、16、32 位。如果设备不支持输出彩色，则该值为 0                                            |                                                                                                  |
| [color-gamut (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/color-gamut)                 | 用户代理和输出设备大致程度上支持的色域                                                                                         | 在 Media Queries Level 4 中被添加。                                                              |
| [color-index (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/color-index)                 | 输出设备的颜色查询表（color lookup table）中的条目数量，如果设备不使用颜色查询表，则该值为 0                                   |                                                                                                  |
| [device-aspect-ratio (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/device-aspect-ratio) | 输出设备的宽高比                                                                                                               | 已在 Media Queries Level 4 中被弃用。                                                            |
| [device-height](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media/device-height)                     | 输出设备渲染表面（如屏幕）的高度                                                                                               | 已在 Media Queries Level 4 中被弃用。                                                            |
| [device-width (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/device-width)               | 输出设备渲染表面（如屏幕）的宽度                                                                                               | 已在 Media Queries Level 4 中被弃用。                                                            |
| [display-mode (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/display-mode)               | 应用程序的显示模式，如web app的manifest中的[display](https://developer.mozilla.org/zh-CN/docs/Web/Manifest#display) 成员所指定 | 在 [Web App Manifest spec](http://w3c.github.io/manifest/#the-display-mode-media-feature)被定义. |
| [forced-colors (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/forced-colors)             | 检测是user agent否限制调色板                                                                                                   | 在 Media Queries Level 5 中被添加。                                                              |
| [grid (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/grid)                               | 输出设备使用网格屏幕还是点阵屏幕？                                                                                             |                                                                                                  |
| [height (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/height)                           | 视窗（viewport）的高度                                                                                                         |                                                                                                  |
| [hover](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media/hover)                                     | 主要输入模式是否允许用户在元素上悬停                                                                                           | 在 Media Queries Level 4 中被添加。                                                              |
| [inverted-colors (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/inverted-colors)         | user agent或者底层操作系统是否反转了颜色                                                                                       | 在 Media Queries Level 5 中被添加。                                                              |
| `light-level`                                                                                              | 环境光亮度                                                                                                                     | 在 Media Queries Level 5 中被添加。                                                              |
| [monochrome (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/monochrome)                   | 输出设备单色帧缓冲区中每个像素的位深度。如果设备并非黑白屏幕，则该值为 0                                                       |                                                                                                  |
| [orientation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media/orientation)                         | 视窗（viewport）的旋转方向                                                                                                     |                                                                                                  |
| [overflow-block (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/overflow-block)           | 输出设备如何处理沿块轴溢出视窗(viewport)的内容                                                                                 | 在 Media Queries Level 4 中被添加。                                                              |
| [overflow-inline (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/overflow-inline)         | 沿内联轴溢出视窗(viewport)的内容是否可以滚动？                                                                                 | 在 Media Queries Level 4 中被添加。                                                              |
| [pointer (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/pointer)                         | 主要输入机制是一个指针设备吗？如果是，它的精度如何？                                                                           | 在 Media Queries Level 4 中被添加。                                                              |
| [prefers-color-scheme](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media/prefers-color-scheme)       | 探测用户倾向于选择亮色还是暗色的配色方案                                                                                       | 在 Media Queries Level 5 中被添加。                                                              |
| [prefers-contrast (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-contrast)       | 探测用户是否有向系统要求提高或降低相近颜色之间的对比度                                                                         | 在 Media Queries Level 5 中被添加。                                                              |
| [prefers-reduced-motion](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media/prefers-reduced-motion)   | 用户是否希望页面上出现更少的动态效果                                                                                           | 在 Media Queries Level 5 中被添加。                                                              |
| `prefers-reduced-transparency`                                                                             | 用户是否倾向于选择更低的透明度                                                                                                 | 在 Media Queries Level 5 中被添加。                                                              |
| [resolution (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/resolution)                   | 输出设备的像素密度（分辨率）                                                                                                   |                                                                                                  |
| `scan`                                                                                                     | 输出设备的扫描过程（适用于电视等）                                                                                             |                                                                                                  |  | [scripting (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/scripting) | 探测脚本（例如 JavaScript）是否可用 | 在 Media Queries Level 5 中被添加。 |  | [update (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/update-frequency) | 输出设备更新内容的渲染结果的频率 | 在 Media Queries Level 4 中被添加。 |
| [width](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media/width)                                     | 视窗（viewport）的宽度，包括纵向滚动条的宽度                                                                                   |                                                                                                  |

## [示例](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media#示例)

```css
@media print {
  body { font-size: 10pt; }
}

@media screen {
  body { font-size: 13px; }
}

@media screen, print {
  body { line-height: 1.2; }
}

@media only screen
  and (min-width: 320px)
  and (max-width: 480px)
  and (resolution: 150dpi) {
    body { line-height: 1.4; }
}
```

媒体查询第4级引入了一种新的范围语法，在测试接受范围的任何特性时允许更简洁的媒体查询，如下面的示例所示：

```css
@media (height > 600px) {
    body { line-height: 1.4; }
}

@media (400px <= width <= 700px) {
    body { line-height: 1.4; }
}
```

更多示例，请查看[使用媒体查询](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries#syntax)。

## [可访问性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media#可访问性)

为了最好地调整网站文本大小，当您需要[](https://developer.mozilla.org/zh-CN/docs/Web/CSS/length) 进行[媒体查询](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries)时，请使用`em`。

`em`和`px`都是有效单位，但如果用户更改浏览器文本大小，`em`的效果会更好。

考虑使用4级媒体查询来改善用户体验。 例如，使用`prefers-reduced-motion`以[检测用户是否已请求系统最小化其使用的动画](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion)或动作量。
