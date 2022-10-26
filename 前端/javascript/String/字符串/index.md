# 字符串

## 模板字符串

```js
`string`
let a = '123'
`string ${a}` // string 123
```

## Unicode表示法

ES6 加强了对 Unicode 的支持，允许采用\uxxxx形式表示一个字符，其中xxxx表示字符的 Unicode 码点。

```js
"\u0061"
// "a"
```

但是，这种表示法只限于码点在\u0000~\uFFFF之间的字符。超出这个范围的字符，必须用两个双字节的形式表示。

```js
"\uD842\uDFB7"
// "𠮷"

"\u20BB7"
// " 7"
```

上面代码表示，如果直接在\u后面跟上超过0xFFFF的数值（比如\u20BB7），JavaScript 会理解成\u20BB+7。由于\u20BB是一个不可打印字符，所以只会显示一个空格，后面跟着一个7。

ES6 对这一点做出了改进，只要将码点放入大括号，就能正确解读该字符。

```js
"\u{20BB7}"
// "𠮷"
```

有了这种表示法之后，JavaScript 共有 6 种方法可以表示一个字符。

```js
'\z' === 'z' // true
'\172' === 'z' // true
'\x7A' === 'z' // true
'\u007A' === 'z' // true
'\u{7A}' === 'z' // true
```

## 拓展方法

### String.prototype.fromCodePoint()

```js
// ES5
console.log(String.fromCharCode(0x20BB7))
// ஷ

// ES6
console.log(String.fromCodePoint(0x20BB7))
// 𠮷
```

### String.prototype.includes()

> `indexOf`:判断一个字符是否存在
>
> `includes`:判断一个字符串是否存在

### String.prototype.startsWith()

> 判断字符串是否在头部

### String.prototype.endsWith()

> 判断字符串是否在尾部

### String.prototype.repeat()

> 返回一个新字符串, 参数表示重复的次数

```js
const str = 'isxxx'
const newStr = str.repeat(10)
console.log(newStr)
// isxxxisxxxisxxxisxxxisxxxisxxxisxxxisxxxisxxxisxxx
```



