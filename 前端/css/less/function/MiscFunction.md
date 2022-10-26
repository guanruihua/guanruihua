# Misc Functions

- 杂项方法

## color

- 解析颜色，使表示颜色的字符串成为颜色

- Parameters: 颜色字符串

- Returns: color

- Example: `color("#aaa")`

- Output: `#aaa`

## image-size

- 获取图片文件尺寸

- Parameters: 要获取图片尺寸的图片文件

- Returns: 尺寸

- Example: `image-size("file.png")`

- Output: `10px 10px`

## image-width

- 获取图片文件尺寸宽度

- Example: `image-width("file.png")`

- Output: `10px`

## image-height

- 获取图片文件尺寸高度

- `Example`: `image-height("file.png")`

- `Output`: `10px`

## convert

- 转换单位
- 第一个参数包含一个带单位的数字，第二个参数包含单位。如果单位兼容，则转换数字。如果它们不兼容，则返回第一个参数，不作修改。

- 可共用单位:

- lengths: m, cm, mm, in, pt and pc
- time: s and ms
- angle: rad, deg, grad and turn

- Parameters:

  - number：带单位的浮点数。
  - 标识符: 字符串或转义值：单位
- Returns: number

Example:

```less
convert(9s, "ms")
convert(14cm, mm)
convert(8, mm) // incompatible  types
```

Output:

```css
9000ms
140mm
8
```

## data-uri

- 如果启用了ieCompat选项且资源太大，或者您在浏览器中使用了该函数，则内联资源并返回url（）。如果未给定MIME类型，则节点使用MIME包确定正确的MIME类型。

Parameters:

- mimetype：（可选）MIME类型字符串。
- url：要内联的文件的url。
若并没有mimetype，数据uri函数将根据文件名后缀猜测它。文本和svg文件编码为utf-8，其他文件编码为base64。
- 如果用户提供了mimetype，如果mimetype参数以结尾，则函数使用base64；base64。例如，图像/jpeg；base64编码为base64，而文本/html编码为utf-8。

- `Example`:`data-uri('../data/image.jpg')`

- `Output`:`url('data:image/jpeg;base64,bm90IGFjdHVhbGx5IGEganBlZyBmaWxlCg==')`

- `Output`: in browser: `url('../data/image.jpg')`

- `Example`:`data-uri('image/jpeg;base64', '../data/image.jpg')`

- `Output`:`url('data:image/jpeg;base64,bm90IGFjdHVhbGx5IGEganBlZyBmaWxlCg==')`

- `Example`:`data-uri('image/svg+xml;charset=UTF-8', 'image.svg')`

- `Output`: `url("data:image/svg+xml;charset=UTF-8,%3Csvg%3E%3Ccircle%20r%3D%229%22%2F%3E%3C%2Fsvg%3E")`

## default

- 仅在保护条件内可用，仅当没有其他mixin匹配时才返回true，否则返回false。

Example:

```less
.mixin(1)                   {x: 11}
.mixin(2)                   {y: 22}
.mixin(@x) when (default()) {z: @x}

div {
  .mixin(3);
}

div.special {
  .mixin(1);
}
```

Output:

```css
div {
  z: 3;
}
div.special {
  x: 11;
}
```

- 可以将默认返回的值与guard运算符一起使用。例如只有当至少有一个以上的mixin定义匹配时，mixin（）when not（default（））{}才会匹配。mixin（）调用:

```less

.mixin(@value) when (ispixel(@value)) {width: @value}
.mixin(@value) when not(default())    {padding: (@value / 5)}

div-1 {
  .mixin(100px);
}

div-2 {
  /*...*/
  .mixin(100%);
}

```

result:

```css

div-1 {
  width: 100px;
  padding: 20px;
}
div-2 {
  /*...*/
}
```

- 允许在相同的保护条件下或在具有相同名称的mixin的不同条件下进行多个default（）调用:

```less

div {
  .m(@x) when (default()), not(default())    {always: @x}
  .m(@x) when (default()) and not(default()) {never:  @x}

  .m(1); // OK
}
```

```less

div {
  .m(@x) when (default())    {}
  .m(@x) when not(default()) {}

  .m(1); // Error
}
```

In above example it is impossible to determine what value each default() call should return since they recursively depend on each other.

Advanced multiple default() usage:

```less
.x {
  .m(red)                                    {case-1: darkred}
  .m(blue)                                   {case-2: darkblue}
  .m(@x) when (iscolor(@x)) and (default())  {default-color: @x}
  .m('foo')                                  {case-1: I am 'foo'}
  .m('bar')                                  {case-2: I am 'bar'}
  .m(@x) when (isstring(@x)) and (default()) {default-string: and I am the default}

  &-blue  {.m(blue)}
  &-green {.m(green)}
  &-foo   {.m('foo')}
  &-baz   {.m('baz')}
}
```

Result:

```css

.x-blue {
  case-2: #00008b;
}
.x-green {
  default-color: #008000;
}
.x-foo {
  case-1: I am 'foo';
}
.x-baz {
  default-string: and I am the default;
}
```

- 默认函数仅在表达式中作为较少的内置函数提供。如果在mixin保护条件之外使用，则将其解释为常规CSS值：

Example:

```less

div {
  foo: default();
  bar: default(42);
}
```

Result:

```css

div {
  foo: default();
  bar: default(42);
}
```

## unit

- 删除或更改标注的

- Example: `unit(5, px)`

- Output: `5px`

- Example: `unit(5em)`

- Output: `5`

## get-unit

- 返回带有单位的数字的单位

- Example: `get-unit(5px)`

- Output: `px`

- Example: `get-unit(5)`

- Output: `//nothing`

## svg-gradient

```less

div {
  @list: red, green 30%, blue;
  background-image: svg-gradient(to right, @list);
}
equivalent - color stops in arguments:

div {
  background-image: svg-gradient(to right, red, green 30%, blue);
}
both result in:

div {
  background-image: url('data:image/svg+xml,%3C%3Fxml%20version%3D%221.0%22%20%3F%3E%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20version%3D%221.1%22%20width%3D%22100%25%22%20height%3D%22100%25%22%20viewBox%3D%220%200%201%201%22%20preserveAspectRatio%3D%22none%22%3E%3ClinearGradient%20id%3D%22gradient%22%20gradients%3D%22userSpaceOnUse%22%20x1%3D%220%25%22%20y1%3D%220%25%22%20x2%3D%22100%25%22%20y2%3D%220%25%22%3E%3Cstop%20offset%3D%220%25%22%20stop-color%3D%22%23ff0000%22%2F%3E%3Cstop%20offset%3D%2230%25%22%20stop-color%3D%22%23008000%22%2F%3E%3Cstop%20offset%3D%22100%25%22%20stop-color%3D%22%230000ff%22%2F%3E%3C%2FlinearGradient%3E%3Crect%20x%3D%220%22%20y%3D%220%22%20width%3D%221%22%20height%3D%221%22%20fill%3D%22url(%23gradient)%22%20%2F%3E%3C%2Fsvg%3E');
}
```

注意：在2.2.0之前的版本中，结果是base64编码的。
生成的背景图像左侧为红色，宽度的30%为绿色，最后为蓝色。Base64编码部分包含以下svg渐变：

```xml

<?xml version="1.0" ?>
<svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="100%" height="100%" viewBox="0 0 1 1" preserveAspectRatio="none">
    <linearGradient id="gradient" gradients="userSpaceOnUse" x1="0%" y1="0%" x2="100%" y2="0%">
        <stop offset="0%" stop-color="#ff0000"/>
        <stop offset="30%" stop-color="#008000"/>
        <stop offset="100%" stop-color="#0000ff"/>
    </linearGradient>
    <rect x="0" y="0" width="1" height="1" fill="url(#gradient)" />
</svg>
```
