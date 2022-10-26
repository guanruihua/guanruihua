待整理

# Intl

<https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Intl>

> ECMAScript 国际化 API 的一个命名空间，它提供了精确的字符串对比、数字格式化，和日期时间格式化。
> `Collator`，`NumberFormat` 和 `DateTimeFormat` 对象的构造函数是 Intl 对象的属性。本页文档内容包括了这些属性，以及国际化使用的构造器和其他语言的方法等常见的功能

- locales 参数:
locales 参数必须是一个 BCP 47 语言标记的字符串，或者是一个包括多个语言标记的数组。如果 locales 参数未提供或者是 undefined，便会使用运行时默认的 locale。
[BCP 47 语言标记](https://www.iana.org/assignments/language-subtag-registry/language-subtag-registry)

属性
Intl.Collator
collators 的构造函数，用于启用对语言敏感的字符串比较的对象。

Intl.DateTimeFormat
用于启用语言敏感的日期和时间格式的对象的构造函数。

Intl.ListFormat
Constructor for objects that enable language-sensitive list formatting.

Intl.NumberFormat
用于启用语言敏感数字格式的对象的构造函数。

Intl.PluralRules
用于启用多种敏感格式和多种语言语言规则的对象的构造函数。

Intl.RelativeTimeFormat
Constructor for objects that enable language-sensitive relative time formatting.
