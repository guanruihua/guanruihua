---
title: mock
date: 2020-09-12 00:20:03
tags:
	- mock
	- javascript

---

# mock

> [官网](http://mockjs.com/)
>
> [示例](http://mockjs.com/examples.html)
>
> [文档](https://github.com/nuysoft/Mock/wiki)
>
> 用于生成随机数据, 帮助前端可以提前工作

## 安装

```bash
npm install mockjs
npm install mockjs -g # 全局安装

$ random url 
# => http://rmcpx.org/funzwc
$ random -h #帮助
```



```js
// 使用 Mock
var Mock = require('mockjs')
var data = Mock.mock({
    // 属性 list 的值是一个数组，其中含有 1 到 10 个元素
    'list|1-10': [{
        // 属性 id 是一个自增数，起始值为 1，每次增 1
        'id|+1': 1
    }]
})
// 输出结果
console.log(JSON.stringify(data, null, 4))
```



## 语法规范

> 1. 数据模板定义规范
> 2. 数据占位定义规范

### 数据模板定义规范

```bash
// 属性名   name
// 生成规则 rule
// 属性值   value
'name|rule': value
```

> 七种格式
>
> 1. 'name|min-max': value
> 2. 'name|count': value
> 3. 'name|min-max.dmin-dmax': value
> 4. 'name|min-max.dcount': value
> 5. 'name|count.dmin-dmax': value
> 6. 'name|count.dcount': value
> 7. 'name|+step': value





## Mock.mock()

### Mock.mock( rurl?, rtype?, template|function( options ) )

> 根据数据模板生成模拟数据。

#### Mock.mock( template )

> 根据数据模板生成模拟数据。

#### Mock.mock( rurl, template )

> 记录数据模板。当拦截到匹配 `rurl` 的 Ajax 请求时，将根据数据模板 `template` 生成模拟数据，并作为响应数据返回。

#### Mock.mock( rurl, function( options ) )

> 记录用于生成响应数据的函数。当拦截到匹配 `rurl` 的 Ajax 请求时，函数 `function(options)` 将被执行，并把执行结果作为响应数据返回。



#### Mock.mock( rurl, rtype, template )

> 记录数据模板。当拦截到匹配 `rurl` 和 `rtype` 的 Ajax 请求时，将根据数据模板 `template` 生成模拟数据，并作为响应数据返回。

#### Mock.mock( rurl, rtype, function( options ) )

> 记录用于生成响应数据的函数。当拦截到匹配 `rurl` 和 `rtype` 的 Ajax 请求时，函数 `function(options)` 将被执行，并把执行结果作为响应数据返回。

#### rurl

> 可选。
>
> 表示需要拦截的 URL，可以是 URL 字符串或 URL 正则。例如 `/\/domain\/list\.json/`、`'/domian/list.json'`。



#### rtype

>  可选。
>
> 表示需要拦截的 Ajax 请求类型。例如 `GET`、`POST`、`PUT`、`DELETE` 等。



#### template

>  可选。
> 表示数据模板，可以是对象或字符串。例如 `{ 'data|1-10':[{}] }`、`'@EMAIL'`。

#### function(options)

> 可选。
>
> 表示用于生成响应数据的函数。

##### options

指向本次请求的 Ajax 选项集，含有 `url`、`type` 和 `body` 三个属性，参见 [XMLHttpRequest 规范](https://xhr.spec.whatwg.org/)。

> 从 1.0 开始，Mock.js 通过覆盖和模拟原生 XMLHttpRequest 的行为来拦截 Ajax 请求，不再依赖于第三方 Ajax 工具库（例如 jQuery、Zepto 等）。



## Mock.setup( settings )

- Mock.setup( settings )

配置拦截 Ajax 请求时的行为。支持的配置项有：`timeout`。

### settings

必选。

配置项集合。

#### timeout

可选。

指定被拦截的 Ajax 请求的响应时间，单位是毫秒。值可以是正整数，例如 `400`，表示 400 毫秒 后才会返回响应内容；也可以是横杠 `'-'` 风格的字符串，例如 `'200-600'`，表示响应时间介于 200 和 600 毫秒之间。默认值是`'10-100'`。

```
Mock.setup({
    timeout: 400
})
Mock.setup({
    timeout: '200-600'
})
```

> 目前，接口 `Mock.setup( settings )` 仅用于配置 Ajax 请求，将来可能用于配置 Mock 的其他行为。



## Mock.Random

Mock.Random 是一个工具类，用于生成各种随机数据。

**Mock.Random 的方法在数据模板中称为『占位符』，书写格式为 `@占位符(参数 [, 参数])` 。**

```js
var Random = Mock.Random
Random.email()
// => "n.clark@miller.io"
Mock.mock('@email')
// => "y.lee@lewis.org"
Mock.mock( { email: '@email' } )
// => { email: "v.lewis@hall.gov" }
```

**注意**

### 方法

Mock.Random 提供的完整方法（占位符）如下：

| Type          | Method                                                       |
| ------------- | ------------------------------------------------------------ |
| Basic         | boolean, natural, integer, float, character, string, range, date, time, datetime, now |
| Image         | image, dataImage                                             |
| Color         | color                                                        |
| Text          | paragraph, sentence, word, title, cparagraph, csentence, cword, ctitle |
| Name          | first, last, name, cfirst, clast, cname                      |
| Web           | url, domain, email, ip, tld                                  |
| Address       | region, province, city, county, zip                          |
| Helper        | capitalize, upper, lower, pick, shuffle                      |
| Miscellaneous | guid, id                                                     |



### 扩展

Mock.Random 中的方法与数据模板的 `@占位符` 一一对应，在需要时还可以为 Mock.Random 扩展方法，然后在数据模板中通过 `@扩展方法` 引用。例如：

```js
Random.extend({
    constellation: function(date) {
        var constellations = ['白羊座', '金牛座', '双子座', '巨蟹座', '狮子座', '处女座', '天秤座', '天蝎座', '射手座', '摩羯座', '水瓶座', '双鱼座']
        return this.pick(constellations)
    }
})
Random.constellation()
// => "水瓶座"
Mock.mock('@CONSTELLATION')
// => "天蝎座"
Mock.mock({
    constellation: '@CONSTELLATION'
})
// => { constellation: "射手座" }
```

## Mock.valid()

### Mock.valid( template, data )

- Mock.valid( template, data )

校验真实数据 `data` 是否与数据模板 `template` 匹配。

#### template

必选。

表示数据模板，可以是对象或字符串。例如 `{ 'list|1-10':[{}] }`、`'@EMAIL'`。

#### data

必选。

表示真实数据。

```js
var template = {
    name: 'value1'
}
var data = {
    name: 'value2'
}
Mock.valid(template, data)
// =>
[
    {
        "path": [
            "data",
            "name"
        ],
        "type": "value",
        "actual": "value2",
        "expected": "value1",
        "action": "equal to",
        "message": "[VALUE] Expect ROOT.name'value is equal to value1, but is value2"
    }
]
```

## Mock.toJSONSchema()

### Mock.toJSONSchema( template )

- Mock.toJSONSchema( template )

把 Mock.js 风格的数据模板 `template` 转换成 [JSON Schema](http://json-schema.org/)。

#### template

必选。

表示数据模板，可以是对象或字符串。例如 `{ 'list|1-10':[{}] }`、`'@EMAIL'`。

```js
var template = {
    'key|1-10': '★'
}
Mock.toJSONSchema(template)
// =>
{
    "name": undefined,
    "path": [
        "ROOT"
    ],
    "type": "object",
    "template": {
        "key|1-10": "★"
    },
    "rule": {},
    "properties": [{
        "name": "key",
        "path": [
            "ROOT",
            "key"
        ],
        "type": "string",
        "template": "★",
        "rule": {
            "parameters": ["key|1-10", "key", null, "1-10", null],
            "range": ["1-10", "1", "10"],
            "min": 1,
            "max": 10,
            "count": 3,
            "decimal": undefined,
            "dmin": undefined,
            "dmax": undefined,
            "dcount": undefined
        }
    }]
}
var template = {
    'list|1-10': [{}]
}
Mock.toJSONSchema(template)
// =>
{
    "name": undefined,
    "path": ["ROOT"],
    "type": "object",
    "template": {
        "list|1-10": [{}]
    },
    "rule": {},
    "properties": [{
        "name": "list",
        "path": ["ROOT", "list"],
        "type": "array",
        "template": [{}],
        "rule": {
            "parameters": ["list|1-10", "list", null, "1-10", null],
            "range": ["1-10", "1", "10"],
            "min": 1,
            "max": 10,
            "count": 6,
            "decimal": undefined,
            "dmin": undefined,
            "dmax": undefined,
            "dcount": undefined
        },
        "items": [{
            "name": 0,
            "path": ["data", "list", 0],
            "type": "object",
            "template": {},
            "rule": {},
            "properties": []
        }]
    }]
}
```