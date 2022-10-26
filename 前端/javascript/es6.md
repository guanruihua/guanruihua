# es6

## 数据类型

> 前六种是：Undefined、Null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）symbol(ES6新增)

## let const

### let

> - 块级作用域:类似于局部变量,只在所处的代码块有效
> - 不可以重复声明
> - 不存在变量提升

```js
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```

> var 变量声明 window.variable === variable
>
> ```js
> var age =14;
> console.log(window.age) // 14
> ```
>
> 这样会造成全局变量污染

demo

```js
// 生成十个按钮, 每次点击的时候弹出对应的数字

// 传统方法
var i= 0;
for (i =1; i <= 10; i++) {
  (function(i){
    var btn = document.createElement('button');
    btn.innerText = i;
    btn.onclick = function() {
      alert(i);
    }
    document.body.appendChild(btn);
  })(i);
}

// 使用let 方法
for (let = 1; i<=10; i++){
  var btn = document.createElement('button');
  btn.innerText = i;
  btn.onclick = function() {
  alert(i)
  }
  document.body.appendChild(btn);
}
```

### const

> - 声明一个只读的常量,常量的值不可以改变
>
> - 一定要赋初值
> - 对于符合类型的变量,变量不指向数据,而是指向数据所在的地址

```js
// 使变量不可以更改
var CST = { value: '张三' };
Object.defineProperty(CST, 'value', {
  writable: false
})

Object.seal(CST); // CST也不可以挂载任何变量/或拓展

const PI = 3.1415;
PI // 3.1415

PI = 3;
// TypeError: Assignment to constant variable.
```

> cost变量还是可以赋值的

```js
const foo = {};
foo.prop = 123;

foo.prop
// 123

const a = [];
a.push('Hello'); // 可执行
a.length = 0;    // 可执行
a = ['Dave'];    // 报错
```

## 变量的解构赋值

### 基本用法

```js
var a = 1;
var b = 2;
var c = 3;
//可以写成
var [a, b, c] = [1, 2, 3];
//set结构
let [x, y, z] = new Set(["a", "b", "c"]);
//默认值
[x, y = 'b'] = ['a']; // x='a', y='b'
[x, y = 'b'] = ['a', undefined]; // x='a', y='b'
//对象
var { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"
//字符串
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"
let {length : len} = 'hello';
len // 5
```

### 遍历map结构

```js
var map = new Map();
map.set('first','hello');
map.set('second','world');

for(let [key, value] of map){
  console.log(key+"is"+value);
}

//获取键名
for(let [key] of map){...}
//获取键值
for(let [,value] of map){...}
```

### 输入模块的指定方法

```js
const { SourceMapConsumer, SourceNode } = require("source-map");
```

## 字符串拓展

### 字符的Unicode表示法

> `\u0000`——`\uFFFF`之间的字符。超出这个范围的字符，必须用两个双字节的形式表达

```js
"\uD842\uDFB7"
// "𠮷"
"\u20BB7"
// " 7"(这里js理解为"\u20BB+7"=>空格+7)
//放进化括号可以解决以上问题
'\u{1F680}' === '\uD83D\uDE80'
// true
```

#### 六种方式表示一个字符

```js
'\z' === 'z'  // true
'\172' === 'z' // true
'\x7A' === 'z' // true
'\u007A' === 'z' // true
'\u{7A}' === 'z' // true
```

### codePointAt()

> 普通字符:2个字符(UTF-16格式)
>
> 特殊字符:4个字符(Unicode格式[Unicode码点>0xFFFF])

```js
var s = "𠮷";//0xD842 0xDFB7

s.length // 2
s.charAt(0) // ''
s.charAt(1) // ''
s.charCodeAt(0) // 55362
s.charCodeAt(1) // 57271
```

### at()

```js
'abc'.charAt(0) // "a"
'𠮷'.charAt(0) // "\uD842"

'abc'.at(0) // "a"
'𠮷'.at(0) // "𠮷"
```

### 字符串遍历器

> for ...of可以遍历大于0xFFFF的码点

```js
for (let codePoint of 'foo') {
  console.log(codePoint)
}
// "f"
// "o"
// "o"
```

### includes(), startsWith(), endsWith()

> - indexOf():用来确定一个字符串是否包含在另一个字符串中。
> - **includes()**：返回布尔值，表示是否找到了参数字符串。
>
> - **startsWith()**：返回布尔值，表示参数字符串是否在源字符串的头部。
> - **endsWith()**：返回布尔值，表示参数字符串是否在源字符串的尾部。

```js
var s = 'Hello world!';

s.startsWith('Hello') // true
s.endsWith('!') // true
s.includes('o') // true

s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false
```

repest()

> 返回一个字符重复n次的新字符

```js
 'x'.repeat(3) // "xxx"
```

### 模板字符串

```js
// 字符串中嵌入变量
var name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`


//标签模板
alert`123`
// 等同于
alert(123)
```

## symbol

> - 通过Symbol函数生成
>   - 原来就有的字符串
>   - 新增的Symbol类型
> - 每一个symbol都是不相等的
>
> -

```js
let s = Symbol();

typeof s
// "symbol"

var mySymbol = Symbol();

var a = {};
a[mySymbol] = 'Hello!';
```

## set和Map数据结构

### Set

> - 每一个成员都是唯一,没有重复的值
> - 遍历操作
>   - keys():返回键名的遍历器
>   - values():返回键值的遍历器
>   - entries():返回键值对的遍历器
>   - forEach():使用回调函数遍历每一个成员

```js
var s = new Set();

[2, 3, 5, 4, 5, 2, 2].map(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4

// 例一
var set = new Set([1, 2, 3, 4, 4]);
[...set]
// [1, 2, 3, 4]

// 例二
var items = new Set([1, 2, 3, 4, 5, 5, 5, 5]);
items.size // 5

// 例三
function divs () {
  return [...document.querySelectorAll('div')];
}

var set = new Set(divs());
set.size // 56

// 类似于
divs().forEach(div => set.add(div));
set.size // 56
```

### Map

> - 只能使用字符串当做键
> - set(key,value):赋值
> - get(key):获取

```js
var data = {};
var element = document.getElementById('myDiv');

data[element] = 'metadata';
data['[object HTMLDivElement]'] // "metadata"

```

```javascript
var m = new Map();
var o = {p: 'Hello World'};

m.set(o, 'content')
m.get(o) // "content"

m.has(o) // true
m.delete(o) // true
m.has(o) // false
```

## Proxy和Reflect

### Proxy

> Proxy 用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。
>
> Proxy 可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。
>
> ```javascript
> var proxy = new Proxy(target, handler);
> target:拦截目标
> handler:拦截行为
> ```

```js
var obj = new Proxy({}, {
  get: function (target, key, receiver) {
    console.log(`getting ${key}!`);
    return Reflect.get(target, key, receiver);
  },
  set: function (target, key, value, receiver) {
    console.log(`setting ${key}!`);
    return Reflect.set(target, key, value, receiver);
  }
});

obj.count = 1
//  setting count!
++obj.count
//  getting count!
//  setting count!
//  2
```

> 对于可以设置、但没有设置拦截的操作，则直接落在目标对象上，按照原先的方式产生结果。
>
> **（1）get(target, propKey, receiver)**
>
> 拦截对象属性的读取，比如`proxy.foo`和`proxy['foo']`。
>
> 最后一个参数`receiver`是一个对象，可选，参见下面`Reflect.get`的部分。
>
> ```js
> var person = {
> name: "张三"
> };
> 
> var proxy = new Proxy(person, {
> get: function(target, property) {
>  if (property in target) {
>    return target[property];
>  } else {
>    throw new ReferenceError("Property \"" + property + "\" does not exist.");
>  }
> }
> });
> 
> proxy.name // "张三"
> proxy.age // 抛出一个错误
> ```
>
>
>
> **（2）set(target, propKey, value, receiver)**
>
> 拦截对象属性的设置，比如`proxy.foo = v`或`proxy['foo'] = v`，返回一个布尔值。
>
> ```js
> let validator = {
> set: function(obj, prop, value) {
>  if (prop === 'age') {
>    if (!Number.isInteger(value)) {
>      throw new TypeError('The age is not an integer');
>    }
>    if (value > 200) {
>      throw new RangeError('The age seems invalid');
>    }
>  }
> 
>  // 对于age以外的属性，直接保存
>  obj[prop] = value;
> }
> };
> 
> let person = new Proxy({}, validator);
> 
> person.age = 100;
> 
> person.age // 100
> person.age = 'young' // 报错
> person.age = 300 // 报错
> ```
>
> **（3）has(target, propKey)**
>
> 拦截`propKey in proxy`的操作，以及对象的`hasOwnProperty`方法，返回一个布尔值。
>
> **（4）deleteProperty(target, propKey)**
>
> 拦截`delete proxy[propKey]`的操作，返回一个布尔值。
>
> **（5）ownKeys(target)**
>
> 拦截`Object.getOwnPropertyNames(proxy)`、`Object.getOwnPropertySymbols(proxy)`、`Object.keys(proxy)`，返回一个数组。该方法返回对象所有自身的属性，而`Object.keys()`仅返回对象可遍历的属性。
>
> **（6）getOwnPropertyDescriptor(target, propKey)**
>
> 拦截`Object.getOwnPropertyDescriptor(proxy, propKey)`，返回属性的描述对象。
>
> **（7）defineProperty(target, propKey, propDesc)**
>
> 拦截`Object.defineProperty(proxy, propKey, propDesc）`、`Object.defineProperties(proxy, propDescs)`，返回一个布尔值。
>
> **（8）preventExtensions(target)**
>
> 拦截`Object.preventExtensions(proxy)`，返回一个布尔值。
>
> **（9）getPrototypeOf(target)**
>
> 拦截`Object.getPrototypeOf(proxy)`，返回一个对象。
>
> **（10）isExtensible(target)**
>
> 拦截`Object.isExtensible(proxy)`，返回一个布尔值。
>
> **（11）setPrototypeOf(target, proto)**
>
> 拦截`Object.setPrototypeOf(proxy, proto)`，返回一个布尔值。
>
> 如果目标对象是函数，那么还有两种额外操作可以拦截。
>
> **（12）apply(target, object, args)**
>
> 拦截 Proxy 实例作为函数调用的操作，比如`proxy(...args)`、`proxy.call(object, ...args)`、`proxy.apply(...)`。
>
> **（13）construct(target, args)**
>
> 拦截 Proxy 实例作为构造函数调用的操作，比如`new proxy(...args)`。

### Reflect

> `Reflect`对象与`Proxy`对象一样，也是ES6为了操作对象而提供的新API。`Reflect`对象的设计目的有这样几个。
>
> （1） 将`Object`对象的一些明显属于语言内部的方法（比如`Object.defineProperty`），放到`Reflect`对象上。现阶段，某些方法同时在`Object`和`Reflect`对象上部署，未来的新方法将只部署在`Reflect`对象上。
>
> （2） 修改某些Object方法的返回结果，让其变得更合理。比如，`Object.defineProperty(obj, name, desc)`在无法定义属性时，会抛出一个错误，而`Reflect.defineProperty(obj, name, desc)`则会返回`false`。
>
> - Reflect.apply(target,thisArg,args)
> - Reflect.construct(target,args)
> - Reflect.get(target,name,receiver)
>
> ```js
> var obj = {
>   get foo() { return this.bar(); },
>   bar: function() { ... }
> };
> 
> // 下面语句会让 this.bar()
> // 变成调用 wrapper.bar()
> Reflect.get(obj, "foo", wrapper);
> ```
>
> - Reflect.set(target,name,value,receiver)
> - Reflect.defineProperty(target,name,desc)
> - Reflect.deleteProperty(target,name)
> - Reflect.has(target,name)
> - Reflect.ownKeys(target)
> - Reflect.isExtensible(target)
> - Reflect.preventExtensions(target)
> - Reflect.getOwnPropertyDescriptor(target, name)
> - Reflect.getPrototypeOf(target)
> - Reflect.setPrototypeOf(target, prototype)

## Generator

> - 提供异步变成解决方案
>
> - 状态机:封装多个内部状态
> - 特征
>   - function:关键词和函数名之间有一个星号(没有规定位置,只要在这之间都可以)
>   - 内部使用yield(产出)语句[定义状态]

```js
function* helloWorldGenerator() {
  yield 'hello';
  yield 'world';
  return 'ending';//结束
}
/*
 建立后并不执行,
 返回也不是函数运行结果
 而是一个指向内部状态指针对象(遍历对象Iterator Object)
*/


var hw = helloWorldGenerator();
//done:false;表示对象遍历没有结束
hw.next()
// { value: 'hello', done: false }

hw.next()
// { value: 'world', done: false }

hw.next()
// { value: 'ending', done: true }

hw.next()
// { value: undefined, done: true }//表示遍历结束
```

## 代理Proxy

### 语法

```js
let p = new Proxy(target, handler);
```

> `target`: 一个目标对象( 可以是任何类型对象, 包括数组函数等, 甚至另一个代理 ) 用Proxy 来封装
>
> `handler`: 一个对象, 其属性是执行一个操作时定义代理的行为函数

### 代理使用

```js
const obj = {
  a: 10
}
let handler = {
  get: function(target, name){
    console.log('test: ', target, name)
    // test:  {"a":10} a
    // test:  {"a":10} b
    return name in target ? target[name] : 37
  }
}
let p = new Proxy(obj, handler)
console.log(p.a, p.b) // 10 37
```

## Object.assign()

## 基本用法

`-Object.assign`方法用于对象的合并，将源对象（source）的所有可枚举属性，复制到目标对象（target）。

```dart
const target = { a: 1 };

const source1 = { b: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

`Object.assign`方法的第一个参数是目标对象，后面的参数都是源对象。

注意，如果目标对象与源对象有同名属性，或多个源对象有**同名属性，则后面的属性会覆盖前面的属性。**

```dart
const target = { a: 1, b: 1 };

const source1 = { b: 2, c: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

如果只有一个参数，`Object.assign`会直接返回该参数。

```dart
const obj = {a: 1};
Object.assign(obj) === obj // true
```

如果该参数不是对象，则会先转成对象，然后返回。

```jsx
typeof Object.assign(2) // "object"
```

由于`undefined`和`null`无法转成对象，所以如果它们作为参数，就会报错。

```jsx
Object.assign(undefined) // 报错
Object.assign(null) // 报错
```

如果非对象参数出现在源对象的位置（即非首参数），那么处理规则有所不同。首先，这些参数都会转成对象，如果无法转成对象，就会跳过。这意味着，如果`undefined`和`null`不在首参数，就不会报错。

```jsx
let obj = {a: 1};
Object.assign(obj, undefined) === obj // true
Object.assign(obj, null) === obj // true
```

其他类型的值（即数值、字符串和布尔值）不在首参数，也不会报错。但是，除了字符串会以数组形式，拷贝入目标对象，其他值都不会产生效果。

```jsx
const v1 = 'abc';
const v2 = true;
const v3 = 10;

const obj = Object.assign({}, v1, v2, v3);
console.log(obj); // { "0": "a", "1": "b", "2": "c" }
```

上面代码中，`v1`、`v2`、`v3`分别是字符串、布尔值和数值，结果只有字符串合入目标对象（以字符数组的形式），数值和布尔值都会被忽略。这是因为只有字符串的包装对象，会产生可枚举属性。

```dart
Object(true) // {[[PrimitiveValue]]: true}
Object(10)  //  {[[PrimitiveValue]]: 10}
Object('abc') // {0: "a", 1: "b", 2: "c", length: 3, [[PrimitiveValue]]: "abc"}
```

上面代码中，布尔值、数值、字符串分别转成对应的包装对象，可以看到它们的原始值都在包装对象的内部属性 `[[PrimitiveValue]]`上面，这个属性是不会被`Object.assign`拷贝的。只有字符串的包装对象，会产生可枚举的实义属性，那些属性则会被拷贝。

`Object.assign`拷贝的属性是有限制的，只拷贝源对象的自身属性（不拷贝继承属性），也不拷贝不可枚举的属性（`enumerable: false`）。

```dart
Object.assign({b: 'c'},
  Object.defineProperty({}, 'invisible', {
    enumerable: false,
    value: 'hello'
  })
)
// { b: 'c' }
```

上面代码中，`Object.assign`要拷贝的对象只有一个不可枚举属性`invisible`，这个属性并没有被拷贝进去。

属性名为 Symbol 值的属性，也会被`Object.assign`拷贝。

```dart
Object.assign({ a: 'b' }, { [Symbol('c')]: 'd' })
// { a: 'b', Symbol(c): 'd' }
```

------

### 注意点

#### （1）浅拷贝

`Object.assign`方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。

```dart
const obj1 = {a: {b: 1}};
const obj2 = Object.assign({}, obj1);

obj1.a.b = 2;
obj2.a.b // 2
```

上面代码中，源对象`obj1`的`a`属性的值是一个对象，`Object.assign`拷贝得到的是这个对象的引用。这个对象的任何变化，都会反映到目标对象上面。

#### （2）同名属性的替换

对于这种嵌套的对象，一旦遇到同名属性，`Object.assign`的处理方法是替换，而不是添加。

```dart
const target = { a: { b: 'c', d: 'e' } }
const source = { a: { b: 'hello' } }
Object.assign(target, source)
// { a: { b: 'hello' } }
```

上面代码中，`target`对象的`a`属性被`source`对象的`a`属性整个替换掉了，而不会得到`{ a: { b: 'hello', d: 'e' } }`的结果。这通常不是开发者想要的，需要特别小心。

一些函数库提供 `Object.assign`的定制版本（比如 Lodash 的`_.defaultsDeep`方法），可以得到深拷贝的合并。

#### （3）数组的处理

`Object.assign`可以用来处理数组，但是会把数组视为对象。

```dart
Object.assign([1, 2, 3], [4, 5])
// [4, 5, 3]
```

上面代码中，`Object.assign`把数组视为属性名为 0、1、2 的对象，因此源数组的 0 号属性`4`覆盖了目标数组的 0 号属性`1`。

#### （4）取值函数的处理

`Object.assign`只能进行值的复制，如果要复制的值是一个取值函数，那么将求值后再复制。

```jsx
const source = {
  get foo() { return 1 }
};
const target = {};

Object.assign(target, source)
// { foo: 1 }
```

上面代码中， `source`对象的`foo`属性是一个取值函数，`Object.assign`不会复制这个取值函数，只会拿到值以后，将这个值复制过去。

## 常见用途

`Object.assign`方法有很多用处。

### （1）为对象添加属性

```jsx
class Point {
  constructor(x, y) {
    Object.assign(this, {x, y});
  }
}
```

上面方法通过`Object.assign`方法，将`x`属性和`y`属性添加到`Point`类的对象实例。

### （2）为对象添加方法

```jsx
Object.assign(SomeClass.prototype, {
  someMethod(arg1, arg2) {
    ···
  },
  anotherMethod() {
    ···
  }
});

// 等同于下面的写法
SomeClass.prototype.someMethod = function (arg1, arg2) {
  ···
};
SomeClass.prototype.anotherMethod = function () {
  ···
};
```

上面代码使用了对象属性的简洁表示法，直接将两个函数放在大括号中，再使用`assign`方法添加到`SomeClass.prototype`之中。

### （3）克隆对象

```jsx
function clone(origin) {
  return Object.assign({}, origin);
}
```

上面代码将原始对象拷贝到一个空对象，就得到了原始对象的克隆。

不过，采用这种方法克隆，只能克隆原始对象自身的值，不能克隆它继承的值。如果想要保持继承链，可以采用下面的代码。

```jsx
function clone(origin) {
  let originProto = Object.getPrototypeOf(origin);
  return Object.assign(Object.create(originProto), origin);
}
```

### （4）合并多个对象

将多个对象合并到某个对象。

```jsx
const merge = (target, ...sources) => Object.assign(target, ...sources);
```

如果希望合并后返回一个新对象，可以改写上面函数，对一个空对象合并。

```jsx
const merge = (...sources) => Object.assign({}, ...sources);
```

### （5）为属性指定默认值

```jsx
const DEFAULTS = {
  logLevel: 0,
  outputFormat: 'html'
};

function processContent(options) {
  options = Object.assign({}, DEFAULTS, options);
  console.log(options);
  // ...
}
```

上面代码中，`DEFAULTS`对象是默认值，`options`对象是用户提供的参数。`Object.assign`方法将`DEFAULTS`和`options`合并成一个新对象，如果两者有同名属性，则`option`的属性值会覆盖`DEFAULTS`的属性值。

注意，由于存在浅拷贝的问题，`DEFAULTS`对象和`options`对象的所有属性的值，最好都是简单类型，不要指向另一个对象。否则，`DEFAULTS`对象的该属性很可能不起作用。

```csharp
const DEFAULTS = {
  url: {
    host: 'example.com',
    port: 7070
  },
};

processContent({ url: {port: 8000} })
// {
//   url: {port: 8000}
// }
```

上面代码的原意是将 `url.port`改成 8000，`url.host`不变。实际结果却是`options.url`覆盖掉`DEFAULTS.url`，所以`url.host`就不存在了。

## function

```js
//传统
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);

//ES6
class Point {
  constructor(x, y) {//类似于java的构造函数
    this.x = x;
    this.y = y;
  }
 // 私有方法
  _bar(baz) {
    return this.snaf = baz;
  }
  toString() {
    return '(' + this.x + ', ' + this.y + ')';
  }
}
```
