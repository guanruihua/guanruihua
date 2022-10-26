# Less Function

## if

```less
@some: foo;

div {
    margin: if((2 > 1), 0, 3px);
    color:  if((iscolor(@some)), @some, black);
}
```

```css
div {
    margin: 0;
    color:  black;
}
```

## boolean

```less
@bg: black;
@bg-light: boolean(luma(@bg) > 50%);

div {
  background: @bg; 
  color: if(@bg-light, black, white);
}
```

```css
div {
  background: black;
  color: white;
}
```

## escape

- 将URL编码应用于在输入字符串中找到的特殊字符。
- 未编码字符：，`/`，`？`，`@`，`&amp;`，`+`，`'`，`~`,`！`,`$`。
- 最常见的编码字符：`\<space\>`，`\`，`^`，`（`，`）`，`{`，`}`，`，`，`：`，`>`，`<`，`；`，`]`，`[`,`=`。
- 参数：string：要转义的字符串。
- 返回：不带引号的转义字符串内容。

```less
escape('a=1')
```

```css
a%3D1
```

## e

- 字符串转义。
- 它需要字符串作为参数，并按原样返回其内容，但不带引号。它可以用于输出无效CSS语法的CSS值，或者使用Less无法识别的专有语法。
- 参数：string-要转义的字符串。
- 返回：string-转义字符串，不带引号。

```less
@mscode: "ms:alwaysHasItsOwnSyntax.For.Stuff()" 
filter: e(@mscode);
```

```css
filter: ms:alwaysHasItsOwnSyntax.For.Stuff();
```

## `% format`

- 函数%（字符串，参数…）设置字符串格式。
- 第一个参数是带占位符的字符串。所有占位符都以百分比符号%开头，后跟字母s、s、d、d、a或a。其余参数包含替换占位符的表达式。如果需要打印百分比符号，请按另一个百分比%%将其转义。
- 如果需要将特殊字符转义到其utf-8转义码中，请使用大写占位符。函数将转义除（）'~！以外的所有特殊字符！。空间编码为%20。小写占位符保留特殊字符。
- 占位符：
  - d、 d、a、a-可以用任何类型的参数（颜色、数字、转义值、表达式等）替换。如果将它们与字符串结合使用，则将使用整个字符串，包括其引号。但是，引号按原样放在字符串中，它们不会以“/”或任何类似的方式转义。
  - s、 s-可替换为任何表达式。如果将其与字符串一起使用，则只使用字符串值-省略引号。
- 参数：
  - `string`：使用占位符格式化字符串
  - `anything*`：替换占位符的值
- 返回：格式化`string`

```less
format-a-d: %("repetitions: %a file: %d", 1 + 2, "directory/file.less");
format-a-d-upper: %('repetitions: %A file: %D', 1 + 2, "directory/file.less");
format-s: %("repetitions: %s file: %s", 1 + 2, "directory/file.less");
format-s-upper: %('repetitions: %S file: %S', 1 + 2, "directory/file.less");
```

```css
format-a-d: "repetitions: 3 file: "directory/file.less"";
format-a-d-upper: "repetitions: 3 file: %22directory%2Ffile.less%22";
format-s: "repetitions: 3 file: directory/file.less";
format-s-upper: "repetitions: 3 file: directory%2Ffile.less";
```

## replace

- 参数:
  - `string`: 待处理子符串
  - `pattern`: 要搜索的字符串/正则
  - `replacement`: 替换的子串
  - `flags?`: 正则标志

- 返回: 替换过字符串的字符串.

```less
replace("Hello, Mars?", "Mars\?", "Earth!");
replace("One + one = 4", "one", "2", "gi");
replace('This is a string.', "(string)\.$", "new $1.");
replace(~"bar-1", '1', '2');
```

```css
"Hello, Earth!";
"2 + 2 = 4";
'This is a new string.';
bar-2;
```
