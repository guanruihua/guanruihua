# Reflect

> <https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Reflect>
>
> - 拦截js操作的方法
> - 与proxy 相同
> - 不是函数对象, 不可以构造
> - 不可通过new 运算符来调用
> - 方法与Object相同

## apply

> `Reflect.apply(target, thisArgument, argumentsList)`
>
> - target: 目标函数
> - thisArgument: target函数调用同事绑定的this对象
> - argumentsList: 实参列表
> 与`Function.prototype.apply()`方法类似

```js
console.log(Reflect.apply(Math.floor, undefined, [1.75]))
// expected output: 1​

console.log(Reflect.apply(String.fromCharCode, undefined, [104, 101, 108, 108, 111]))
// expected output: "hello"

console.log(Reflect.apply(RegExp.prototype.exec, /ab/, ['confabulation']).index)
// expected output: 4

console.log(Reflect.apply(''.charAt, 'ponies', [3]))
// expected output: "i"
```

## construct

> 行为有点像 new 操作符 构造函数 ， 相当于运行 new target(...args).
> `Reflect.construct(target, argumentsList[, newTarget])`
> target 被运行的目标构造函数
> argumentsList 类数组，目标构造函数调用时的参数。
> newTarget ? 作为新创建对象的原型对象的constructor属性， 参考 new.target 操作符，默认值为target。
> return 以target（如果newTarget存在，则为newTarget）函数为构造函数，argumentList为其初始化参数的对象实例

```js
var d = Reflect.construct(Date, [1776, 6, 4]);
d instanceof Date; // true
d.getFullYear(); // 1776
```

## defineProperty

> `Reflect.defineProperty(target, propertyKey, attributes)`
>
> - target 目标对象
> - propertyKey 要定义或修改的属性的名称
> - attributes 要定义或修改的属性的描述
> return Boolean 属性是否被成功定义。

```js
let obj = {}
Reflect.defineProperty(obj, 'x', {value: 7})  // true
obj.x
// 检查属性是否被重新定义
if (Reflect.defineProperty(target, property, attributes)) {
  // 成功
} else {
  // 失败
}
```

## deleteProperty
>
> - 静态方法 Reflect.deleteProperty() 允许用于删除属性
> - 很像 delete operator ，但它是一个函数
>
> `Reflect.deleteProperty(target, propertyKey)`
>
> - 参数 target 删除属性的目标对象
> - propertyKey 需要删除的属性的名称
> return Boolean 值表明该属性是否被成功删除

```js
var obj = { x: 1, y: 2 };
Reflect.deleteProperty(obj, "x"); // true
obj; // { y: 2 }

var arr = [1, 2, 3, 4, 5];
Reflect.deleteProperty(arr, "3"); // true
arr; // [1, 2, 3, , 5]

// 如果属性不存在，返回 true
Reflect.deleteProperty({}, "foo"); // true

// 如果属性不可配置，返回 false
Reflect.deleteProperty(Object.freeze({foo: 1}), "foo"); // false
```

## get

> 对象 (target[propertyKey]) 中读取属性类似，但它是通过一个函数执行来操作的
> `Reflect.get(target, propertyKey[, receiver])`
>
> - target 需要取值的目标对象
> - propertyKey 需要获取的值的键值
> - receiver 如果target对象中指定了getter，receiver则为getter调用时的this值。
> return any 属性的值
>

```js
// Object
var obj = { x: 1, y: 2 };
Reflect.get(obj, "x"); // 1

// Array
Reflect.get(["zero", "one"], 1); // "one"

// Proxy with a get handler
var x = {p: 1};
var obj = new Proxy(x, {
  get(t, k, r) { return k + "bar"; }
});
Reflect.get(obj, "foo"); // "foobar"
```

## getOwnPropertyDescriptor

> - 静态方法 `Reflect.getOwnPropertyDescriptor()` 与 `Object.getOwnPropertyDescriptor()` 方法相似
> - 如果在对象中存在，则返回给定的属性的属性描述符。否则返回 undefined。
> `Reflect.getOwnPropertyDescriptor(target, propertyKey)`
>
> - target 需要寻找属性的目标对象
> - propertyKey 获取自己的属性描述符的属性的名称
> return any 如果属性存在于给定的目标对象中，则返回属性描述符；否则，返回 undefined
>

```js
Reflect.getOwnPropertyDescriptor({x: "hello"}, "x");
// {value: "hello", writable: true, enumerable: true, configurable: true}

Reflect.getOwnPropertyDescriptor({x: "hello"}, "y");
// undefined

Reflect.getOwnPropertyDescriptor([], "length");
// {value: 0, writable: true, enumerable: false, configurable: false}

```

### 与 Object.getOwnPropertyDescriptor() 的不同点

> 如果该方法的第一个参数不是一个对象（一个原始值），那么将造成 TypeError 错误
> 而对于 Object.getOwnPropertyDescriptor，非对象的第一个参数将被强制转换为一个对象处理

```js
Reflect.getOwnPropertyDescriptor("foo", 0);
// TypeError: "foo" is not non-null object

Object.getOwnPropertyDescriptor("foo", 0);
// { value: "f", writable: false, enumerable: true, configurable: false }
```

## getPrototypeOf
>
> - 静态方法 Reflect.getPrototypeOf() 与 Object.getPrototypeOf() 方法几乎是一样的
> - 都是返回指定对象的原型（即内部的 [[Prototype]] 属性的值）
> `Reflect.getPrototypeOf(target)`
> - target 获取原型的目标对象
> - return 给定对象的原型。如果给定对象没有继承的属性，则返回 null
>
```js
Reflect.getPrototypeOf({}); // Object.prototype
Reflect.getPrototypeOf(Object.prototype); // null
Reflect.getPrototypeOf(Object.create(null)); // null
```

### 与object.getPrototypeOf比较

```js
// 如果参数为 Object，返回结果相同
Object.getPrototypeOf({})   // Object.prototype
Reflect.getPrototypeOf({})  // Object.prototype

// 在 ES5 规范下，对于非 Object，抛异常
Object.getPrototypeOf('foo')   // Throws TypeError
Reflect.getPrototypeOf('foo')  // Throws TypeError

// 在 ES2015 规范下，Reflect 抛异常, Object 强制转换非 Object
Object.getPrototypeOf('foo')   // String.prototype
Reflect.getPrototypeOf('foo')  // Throws TypeError

// 如果想要模拟 Object 在 ES2015 规范下的表现，需要强制类型转换
Reflect.getPrototypeOf(Object('foo'))  // String.prototype
```

## has

> 静态方法 Reflect.has() 作用与 in 操作符 相同
>
> `Reflect.has(target, propertyKey)`
>
> target 目标对象
> propertyKey 属性名，需要检查目标对象是否存在此属性
> return Boolean 是否存在此属性

```js
Reflect.has({x: 0}, "x"); // true
Reflect.has({x: 0}, "y"); // false

// 如果该属性存在于原型链中，返回true
Reflect.has({x: 0}, "toString");

// Proxy 对象的 .has() 句柄方法
obj = new Proxy({}, {
  has(t, k) { return k.startsWith("door"); }
});
Reflect.has(obj, "doorbell"); // true
Reflect.has(obj, "dormitory"); // false

```

## isExtensible

> 静态方法 Reflect.isExtensible() 判断一个对象是否可扩展 （即是否能够添加新的属性）
> 与它 Object.isExtensible()
> `Reflect.isExtensible(target)`
>
> - target 检查是否可扩展的目标对象。
> return Boolean 是否可扩展。
>
```js
// New objects are extensible.
var empty = {};
Reflect.isExtensible(empty); // === true

// ...but that can be changed.
Reflect.preventExtensions(empty);
Reflect.isExtensible(empty); // === false

// Sealed objects are by definition non-extensible.
var sealed = Object.seal({});
Reflect.isExtensible(sealed); // === false

// Frozen objects are also by definition non-extensible.
var frozen = Object.freeze({});
Reflect.isExtensible(frozen); // === false

```

## 与 Object.isExtensible() 的不同点

> 如果该方法的第一个参数不是一个对象（原始值），那么将造成一个 TypeError 异常
> 对于 Object.isExtensible()，非对象的第一个参数会被强制转换为一个对象

```js
Reflect.isExtensible(1);
// TypeError: 1 is not an object

Object.isExtensible(1);
// false
```

## ownKeys

> `Reflect.ownKeys(target)`
> 静态方法 `Reflect.ownKeys()` 返回一个由目标对象自身的属性键组成的数组。

```js
Reflect.ownKeys({z: 3, y: 2, x: 1}); // [ "z", "y", "x" ]
Reflect.ownKeys([]); // ["length"]

var sym = Symbol.for("comet");
var sym2 = Symbol.for("meteor");
var obj = {[sym]: 0, "str": 0, "773": 0, "0": 0,
           [sym2]: 0, "-1": 0, "8": 0, "second str": 0};
Reflect.ownKeys(obj);
// [ "0", "8", "773", "str", "-1", "second str", Symbol(comet), Symbol(meteor) ]
// Indexes in numeric order,
// strings in insertion order,
// symbols in insertion order

```

## preventExtensions

> `Reflect.preventExtensions(target)`
>
> - target: 阻止的目标对象
> - return boolean 是否可以拓展
> - 阻止新属性添加到对象 (例如：防止将来对对象的扩展被添加到对象中)
> - 与 `Object.preventExtensions()` 相似
>
```js
// Objects are extensible by default.
var empty = {};
Reflect.isExtensible(empty); // === true

// ...but that can be changed.
Reflect.preventExtensions(empty);
Reflect.isExtensible(empty); // === false


Reflect.preventExtensions(1);
// TypeError: 1 is not an object

Object.preventExtensions(1);
// 1

```

## set

> - 在一个对象上设置一个属性
> - 静态方法
>
> `Reflect.set(target, propertyKey, value[, receiver])`
>
> - target: 设置属性的目标对象
> - propertyKey: 设置属性的名称
> - value: 设置的值
> - receiver ?  如果有setter, receiver则为setter调用时的this值
> return boolean 是否设置成功

```js
// Object
var obj = {};
Reflect.set(obj, "prop", "value"); // true
obj.prop; // "value"

// Array
var arr = ["duck", "duck", "duck"];
Reflect.set(arr, 2, "goose"); // true
arr[2]; // "goose"

// It can truncate an array.
Reflect.set(arr, "length", 1); // true
arr; // ["duck"];

// With just one argument, propertyKey and value are "undefined".
var obj = {};
Reflect.set(obj); // true
Reflect.getOwnPropertyDescriptor(obj, "undefined");
// { value: undefined, writable: true, enumerable: true, configurable: true }

```

## setPrototypeOf

> 除了返回类型以外，静态方法 Reflect.setPrototypeOf() 与 Object.setPrototypeOf() 方法是一样的。
> 它可设置对象的原型（即内部的 [[Prototype]] 属性）为另一个对象或 null，
> 如果操作成功返回 true，否则返回 false。
>
> `Reflect.setPrototypeOf(target, prototype)`
>
> - target: 设置原型的目标对象
> - prototype: 对象的新原型（一个对象或 null）
> return boolean 是否成功设置
>
```js
Reflect.setPrototypeOf({}, Object.prototype); // true

// It can change an object's [[Prototype]] to null.
Reflect.setPrototypeOf({}, null); // true

// Returns false if target is not extensible.
Reflect.setPrototypeOf(Object.freeze({}), null); // false

// Returns false if it cause a prototype chain cycle.
var target = {};
var proto = Object.create(target);
Reflect.setPrototypeOf(target, proto); // false

```
