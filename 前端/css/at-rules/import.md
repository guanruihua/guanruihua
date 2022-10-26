# @import

## [概述](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@import#概述)

**`@import`**[CSS](https://developer.mozilla.org/en-US/docs/Web/CSS)[@规则](https://developer.mozilla.org/en-US/docs/Web/CSS/At-rule)，用于从其他样式表导入样式规则。这些规则必须先于所有其他类型的规则，[@charset](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@charset) 规则除外; 因为它不是一个[嵌套语句](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Syntax#nested_statements)，@import不能在[条件组的规则](https://developer.mozilla.org/zh-CN/docs/Web/CSS/At-rule#conditional_group_rules)中使用。

因此，用户代理可以避免为不支持的媒体类型检索资源，作者可以指定依赖媒体的@import规则。这些条件导入在URI之后指定逗号分隔的[媒体查询](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries)。在没有任何媒体查询的情况下，导入是无条件的。指定所有的媒体具有相同的效果。

## [语法](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@import#语法)

```css
@import url;
@import url list-of-media-queries;
```

其中:

- *url*

  是一个表示要引入资源位置的 [](https://developer.mozilla.org/zh-CN/docs/Web/CSS/string) 或者 [(en-US)](https://developer.mozilla.org/en-US/docs/Web/CSS/url()) 。 这个 URL 可以是绝对路径或者相对路径。 要注意的是这个 URL 不需要指明一个文件； 可以只指明包名，然后合适的文件会被自动选择 (e.g. **chrome://communicator/skin/**). [See here](https://developer.mozilla.org/en-US/docs/Mozilla/Tech/XUL/Tutorial/The_Chrome_URL) 了解更多。

- *list-of-media-queries*

  是一个逗号分隔的 [媒体查询](https://developer.mozilla.org/en-US/docs/Web/CSS/Media_Queries/Using_media_queries) 条件列表，决定通过URL引入的 CSS 规则 在什么条件下应用。如果浏览器不支持列表中的任何一条媒体查询条件，就不会引入URL指明的CSS文件。

## [正规语法](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@import#正规语法)

```css
@import [ <string> | <url> ]
        [ layer | layer(<layer-name>) ]?
        [ supports( [ <supports-condition> | <declaration> ] ) ]?
        <media-query-list>? ;

where <supports-condition> = not <supports-in-parens> | <supports-in-parens> [ and <supports-in-parens> ]* | <supports-in-parens> [ or <supports-in-parens> ]*<media-query-list> = <media-query>#

where <supports-in-parens> = ( <supports-condition> ) | <supports-feature> | <general-enclosed><media-query> = <media-condition> | [ not | only ]? <media-type> [ and <media-condition-without-or> ]?

where <supports-feature> = <supports-decl> | <supports-selector-fn><general-enclosed> = [ <function-token> <any-value> ) ] | ( <ident> (en-US) <any-value> )<media-condition> = <media-not> | <media-and> | <media-or> | <media-in-parens><media-type> = <ident> (en-US)<media-condition-without-or> = <media-not> | <media-and> | <media-in-parens>

where <supports-decl> = ( <declaration> )<supports-selector-fn> = selector( <complex-selector> )<media-not> = not <media-in-parens><media-and> = <media-in-parens> [ and <media-in-parens> ]+<media-or> = <media-in-parens> [ or <media-in-parens> ]+<media-in-parens> = ( <media-condition> ) | <media-feature> | <general-enclosed>

where <complex-selector> = <compound-selector> [ <combinator>? <compound-selector> ]*<media-feature> = ( [ <mf-plain> | <mf-boolean> | <mf-range> ] )

where <compound-selector> = [ <type-selector>? <subclass-selector>* [ <pseudo-element-selector> <pseudo-class-selector>* ]* ]!<combinator> = '>' | '+' | '~' | [ '||' ]<mf-plain> = <mf-name> : <mf-value><mf-boolean> = <mf-name><mf-range> = <mf-name> [ '<' | '>' ]? '='? <mf-value> | <mf-value> [ '<' | '>' ]? '='? <mf-name> | <mf-value> '<' '='? <mf-name> '<' '='? <mf-value> | <mf-value> '>' '='? <mf-name> '>' '='? <mf-value>

where <type-selector> = <wq-name> | <ns-prefix>? '*'<subclass-selector> = <id-selector> | <class-selector> | <attribute-selector> | <pseudo-class-selector><pseudo-element-selector> = ':' <pseudo-class-selector><pseudo-class-selector> = ':' <ident-token> | ':' <function-token> <any-value> ')'<mf-name> = <ident> (en-US)<mf-value> = <number> | <dimension> | <ident> (en-US) | <ratio>

where <wq-name> = <ns-prefix>? <ident-token><ns-prefix> = [ <ident-token> | '*' ]?  | <id-selector> = <hash-token><class-selector> = '.' <ident-token><attribute-selector> = '[' <wq-name> ']' | '[' <wq-name> <attr-matcher> [ <string-token> | <ident-token> ] <attr-modifier>? ']'

where <attr-matcher> = [ '~' |  |  | '^' | '$' | '*' ]? '='<attr-modifier> = i | s
```

## [示例](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@import#示例)

```css
@import url("fineprint.css") print;
@import url("bluish.css") projection, tv;
@import 'custom.css';
@import url("chrome://communicator/skin/");
@import "common.css" screen, projection;
@import url('landscape.css') screen and (orientation:landscape);
```
