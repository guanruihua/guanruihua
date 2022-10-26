# @font-face

> 指定一个用于显示文本的自定义字体；字体能从远程服务器或者用户本地安装的字体加载. 如果提供了local()函数，从用户本地查找指定的字体名称，并且找到了一个匹配项, 本地字体就会被使用. 否则, 字体就会使用url()函数下载的资源。
>
> 通过允许作者提供他们自己的字体，@font-face 让设计内容成为了一种可能，同时并不会被所谓的"网络-安全"字体所限制(字体如此普遍以至于它们能被广泛的使用). 指定查找和使用本地安装的字体名称可以让字体的自定义化程度超过基本字体，同时在不依赖网络情况下实现此功能。
>
> 在同时使用url()和local()功能时，为了用户已经安装的字体副本在需要使用时被使用，如果在用户本地没有找到字体副本就会去使用户下载的副本查找字体。
>
> @font-face 规则不仅仅使用在CSS的顶层，还可以用在任何CSS条件组规则中.

```css
@font-face {
  font-family: "Open Sans";
  src: url("/fonts/OpenSans-Regular-webfont.woff2") format("woff2"),
       url("/fonts/OpenSans-Regular-webfont.woff") format("woff");
}
```

Copy to Clipboard

## [概述](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face#概述)

这是一个叫做`@font-face` 的[CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS) [@规则](https://developer.mozilla.org/zh-CN/docs/Web/CSS/At-rule) ，它允许网页开发者为其网页指定在线字体。 通过这种作者自备字体的方式，`@font-face` 可以消除对用户电脑字体的依赖。 `@font-face` 不仅可以放在在CSS的最顶层, 也可以放在 @规则 的 [条件规则组](https://developer.mozilla.org/zh-CN/docs/Web/CSS/At-rule#conditional_group_rules) 中。

## [语法](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face#语法)

```css
@font-face {
  [ font-family: <family-name>; ] ||
  [ src: <src>; ] ||
  [ unicode-range: <unicode-range>; ] ||
  [ font-variant: <font-variant>; ] ||
  [ font-feature-settings: <font-feature-settings>; ] ||
  [ font-variation-settings: <font-variation-settings>; ] ||
  [ font-stretch: <font-stretch>; ] ||
  [ font-weight: <font-weight>; ] ||
  [ font-style: <font-style>; ] ||
  [ size-adjust: <size-adjust>; ] ||
  [ ascent-override: <ascent-override>; ] ||
  [ descent-override: <descent-override>; ] ||
  [ line-gap-override: <line-gap-override>; ]
}where <family-name> = <string> | <custom-ident>+
```

### [取值](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face#取值)

- [font-family](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face/font-family)

  所指定的字体名字将会被用于font或font-family属性( i.e. font-family: <family-name>; )

- [src](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face/src)

  远程字体文件位置的URL或者用户计算机上的字体名称， 可以使用local语法通过名称指定用户的本地计算机上的字体( i.e. src: local('Arial'); )。 如果找不到该字体，将会尝试其他来源，直到找到它。

- [font-variant (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-variant)

  A [font-variant](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-variant) value.

- [font-stretch (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-stretch)

  A [font-stretch](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-stretch) value.

- [font-weight (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/font-weight)

  A [font-weight](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-weight) value.

- [font-style](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face/font-style)

  对于src所指字体的描述。如果所需字体符合描述，则采用本font-face所定义的字体。

- [unicode-range (en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face/unicode-range)

  在该@font-face中定义的unicode字体范围

## [示例](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face#示例)

下面的例子简单定义了一个可下载的字体，并应用到了文档的整个body标签上。

[View live sample](https://developer.mozilla.org/@api/deki/files/2935/=webfont-sample.html)

```html
<html>
<head>
  <title>Web Font Sample</title>
  <style type="text/css" media="screen, print">
    @font-face {
      font-family: "Bitstream Vera Serif Bold";
      src: url("https://developer.mozilla.org/@api/deki/files/2934/=VeraSeBd.ttf");
    }

    body { font-family: "Bitstream Vera Serif Bold", serif }
  </style>
</head>
<body>
  This is Bitstream Vera Serif Bold.
</body>
</html>
```

在接下来的例子中，用到了用户本地字体"Helvetica Neue Bold"的备份；如果当前用户(浏览器)未安装该字体(两种可能的字体名都已经试过)，就会用下载的字体"MgOpenModernaBold.ttf"来代替：

```css
@font-face {
  font-family: MyHelvetica;
  src: local("Helvetica Neue Bold"),
  local("HelveticaNeue-Bold"),
  url(MgOpenModernaBold.ttf);
  font-weight: bold;
}
```

Copy to Clipboard

接下来的例子在英文原文中已被删除。

这个例子新定义了一个字体，正常粗细的字采用字体Times New Roman，粗体字采用Consolas。

```css
@font-face {
    font-family: myFirstFont;
    src: local("Times New Roman");
    font-weight:normal;
}

@font-face {
    font-family: myFirstFont;
    src: local(Consolas);
    font-weight:bold;
}
```

Copy to Clipboard

## [注意](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face#注意)

- 这里使用的Web fonts 仍然受到同域限制  (字体文件必须和调用它的网页同一域), 但可以使用 [HTTP access controls](https://developer.mozilla.org/en-US/docs/HTTP_access_control) 解除这一限制。

- 因为这里没有为 TrueType(ttf), OpenType(otf) 和 Web Open File Format(WOFF) 字体定义MIME，因此不能为这些字体类型设置特定的MIME（实际上WOFF的MIME将会是application/font-woff，但浏览器对此MIME的识别还不统一，其它字体情况也类似，可暂时使用application/octet-stream）。

- 你不能在一个 CSS 选择器中定义 @font-face 。例如，这样写是无效的：

  ```css
  .className {
    @font-face {
      font-family: MyHelvetica;
      src: local("Helvetica Neue Bold"), local("HelveticaNeue-Bold"),
          url(MgOpenModernaBold.ttf);
      font-weight: bold;
    }
  }
  ```
