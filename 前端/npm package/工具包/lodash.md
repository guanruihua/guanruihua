---
``title: Lodash
date: 2021-01-14 09:34:35
tags:
- lodash
- front-end
- javascript
---



# Lodash

## 前言

> Lodash 是一个一致性, 模块化, 高性能的javascript使用工具库
>
> [官网](https://www.lodashjs.com/)
>
> [Lodash Documentation](https://lodash.com/docs/4.17.15)

### 安装

```html
<script src = "lodash.js"></script>
```

```shell
npm i loadash --save
```

```js
// Load the full build.
var _ = require('lodash');
// Load the core build.
var _ = require('lodash/core');
// Load the FP build for immutable auto-curried iteratee-first data-last methods.
var fp = require('lodash/fp');
 
// Load method categories.
var array = require('lodash/array');
var object = require('lodash/fp/object');
 
// Cherry-pick methods for smaller browserify/rollup/webpack bundles.
var at = require('lodash/at');
var curryN = require('lodash/fp/curryN');
```

## 数组



## 集合

## 函数

## 对象

### pick

```js
var object = { 'a': 1, 'b': '2', 'c': 3 };
 
_.pick(object, ['a', 'c']);
// => { 'a': 1, 'c': 3 }
```

### pickBy

> 第二参数, 就是过滤出只要获取的值类型

```js
var object = { 'a': 1, 'b': '2', 'c': 3 };
 

_.pickBy(object, _.isNumber);
// => { 'a': 1, 'c': 3 }
```

### omit

```js
var object = { 'a': 1, 'b': '2', 'c': 3 };
 
_.omit(object, ['a', 'c']);
// => { 'b': '2' }
```

## 字符串

## Seq

## number

>- clamp : 限制数据的输出范围
>- inRange : 判断范围
>- random ： 生成随机树

```js
let _ = require('lodash')

let runArray = [
  () => [
      // 后两个参数 分别是上限和下限
    _.clamp( -10, -5, 5),
    _.clamp( 10, -5, 5),
  ],
  () => [
      // 判断第一个值是否在后两个参数定义的范围中
      // 若 总共只有两个参数 第二参数 为上限 下限为 0 , 若第二个参数为负数, 上限为0 , 下限为该参数
      _.inRange(3, 2, 4),
      _.inRange(3, 4),
      _.inRange(3, -4),
      _.inRange(-2, -4),
  ],
  () => [
      // 三个参数分别为: 上限[0], 下限[1], 是否为浮点数
      _.random(5), // 改范围为0 - 5
      _.random(1, 5),
      _.random(1, 5, true),
      _.random(1.1, 5.1),// 前两个参数为浮点型, 就会认为是浮点型
  ]
]

runArray[runArray.length - 1]().map(item => console.log(item)) 
```





## lang

> - castArray : 强制转换为数组 
> - clone :  浅拷贝
> - deepDeep : 深拷贝 

```js
let _ = require('lodash')

let runArray = [
  () => [
    // 强制转换为数组
    _.castArray(1),
    // => [1]
    _.castArray({ 'a': 1 }),
    // => [{ 'a': 1 }]
    _.castArray('abc'),
    // => ['abc']
    _.castArray(null),
    // => [null]
    _.castArray(undefined),
    // => [undefined]
    _.castArray(),
    // => []
  ],
  () => [
    (()=>{
      // 浅拷贝
      let objects = [{ 'a': 1 }, { 'b': 2 }];
      let shallow = _.clone(objects);
      console.log(shallow[0] === objects[0]);
      // => true

    })(),
    (()=>{
      // 深拷贝
      let objects = [{ 'a': 1 }, { 'b': 2 }];
      let deep = _.cloneDeep(objects);
      console.log(objects[0] === deep[0]);
      console.log( deep );
      console.log( objects[0]['a'] === deep[0]['a']);
      // => false
    })(),
  ]

]

runArray[runArray.length - 1]().map(item => console.log(item)) 
```

 