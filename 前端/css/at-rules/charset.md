
# @charset

> 指定样式表使用的字符集
>
> 样式表中第一行, 前面不可有任何字符

```css
@charset "UTF-8";
@charset "utf-8"; /*大小写不敏感*/
/* 设置css的编码格式为Unicode UTF-8 */
@charset 'iso-8859-15'; /* 无效的, 使用了错误的引号 */
@charset 'UTF-8';       /* 无效的, 使用了错误的引号 */
@charset  "UTF-8";      /* 无效的, 多于一个空格 */
 @charset "UTF-8";      /* 无效的, 在at-rule之前多了一个空格 */
@charset UTF-8;         /* Invalid, without ' or ", the charset is not a CSS <string> */
```
