- ### Iterator

  > 处理集合中的每个项是很常见的操作。JavaScript 提供了许多迭代集合的方法，从简单的for循环到map()和filter()。迭代器和生成器将迭代的概念直接带入核心语言，并提供了一种机制来自定义for...of循环的行为。

如果对 MDN 这个描述理解不是很到位的话，可以看下接下来这个小示例：

```
let authors = {
    allAuthors: {
        fiction: [
            'Agatha Christie',
            'J. K. Rowling',
            'Dr. Seuss'
        ],
        scienceFiction: [
            'Neal Stephenson',
            'Arthur Clarke',
            'Isaac Asimov',
            'Robert Heinlein'
        ],
        fantasy: [
            'J. R. R. Tolkien',
            'J. K. Rowling',
            'Terry Pratchett'
        ]
    }
}
```

这个数据结构是汇总了所有作者，每个作者按创作性质进行了分类。如果我们想获取所有作者的名单，该怎么做呢？

```
for (let author of authors) {
    console.log(author)
}
```

你发现这个遍历遇到了报错：Uncaught TypeError: authors is not iterable

**1、基本语法**

Iterator 就是 ES6 中用来实现自定义遍历的接口，按照上述的示例，我们来实现下这个接口：

```
authors[Symbol.iterator] = function() {
    let allAuthors = this.allAuthors
    let keys = Reflect.ownKeys(allAuthors)
    let values = []
    return {
        next() {
            if (!values.length) {
                if (keys.length) {
                    values = allAuthors[keys[0]]
                    keys.shift()
                }
            }
            return {
                done: !values.length,
                value: values.shift()
            }
        }
    }
}
```

这个代码在数据结构上部署了 Iterator 接口，我们就可以用 for...of 来遍历代码了：

```
for (let value of authors) {
    console.log( `${value}` )
}
```

**2、可迭代协议和迭代器协议。**

- 1. 迭代器协议

这是两个概念：可迭代协议、迭代器协议。通俗的讲，迭代器协议要求符合以下条件：

- 首先，它是一个对象
- 其次，这个对象包含一个无参函数 next
- 最后，next 返回一个对象，对象包含 done 和 value 属性。其中 done 表示遍历是否结束，value 返回当前遍历的值。
- 1. 可迭代协议
     可迭代协议允许 JavaScript 对象去定义或定制它们的迭代行为, 例如（定义）在一个 for..of 结构中什么值可以被循环（得到）。一些内置类型都是内置的可迭代类型并且有默认的迭代行为, 比如 Array or Map, 另一些类型则不是 (比如Object) 。

为了变成可迭代对象， 一个对象必须实现 @@iterator 方法, 意思是这个对象（或者它原型链 prototype chain 上的某个对象）必须有一个名字是 Symbol.iterator 的属性。

如果让一个对象是可遍历的，就要遵守可迭代协议，该协议要求对象要部署一个以 Symbol.iterator 为 key 的键值对，而 value 就是一个无参函数，这个函数返回的对象要遵守迭代器协议。

**3、Generator**

熟悉了 Generator 之后，发现它是天然满足可迭代协议的。上述的代码我们可以用 Generator 来实现：

```
authors[Symbol.iterator] = function*() {
    let allAuthors = this.allAuthors
    let keys = Reflect.ownKeys(allAuthors)
    let values = []
    while (1) {
        if (!values.length) {
            if (keys.length) {
                values = allAuthors[keys[0]]
                keys.shift()
                yield values.shift()
            } else {
                return false
            }
        } else {
            yield values.shift()
        }
    }
}
```

同一个场景，同一个数据结构，写法确实不同的，利用 Generator 就不再需要显示的写迭代协议了（next方法和包含 done、value 属性的返回对象）。