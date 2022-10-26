---
title:`Object`
date:2021-4-27 13:44:32
---



# JavaScript中Object构造函数的方法

## Object

### 简洁属性表示法

```js
let name = 'xx'
  let age = 18
  let obj = {
      name,
      age
  }
```



### 属性名表达式

```js
  let s = 'school'
  let obj = {
      foo: 'bar',
      [s]: 'xx'
  }
```



## Object构造函数的方法

### Object.assign()

> 通过复制一个或多个对象来创建一个新的对象
>
> 用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };
const returnedTarget = Object.assign(target, source);
console.log(source); // { b: 4, c: 5 }; 原数据不变
console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }
console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
```



### Object.create()

> 使用指定的原型对象和属性创建一个新对象。 
>
> 创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。 

```js
const person = {
  isHuman: false,
  printIntroduction: function () {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};

const me = Object.create(person);

me.name = "Matthew"; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // inherited properties can be overwritten

me.printIntroduction();
// expected output: "My name is Matthew. Am I human? true"
```



### Object.defineProperty()

> 给对象添加一个属性并指定该属性的配置。
>
> 方法直接在一个对象上定义新的属性或修改现有属性，并返回该对象。
>
> 语法   Object.defineProperties(obj, props)   obj在其上定义或修改属性的对象。props要定义其可枚举属性或修改的属性描述符的对象。对象中存在的属性描述符主要有两种：数据描述符和访问器描述符（更多详情，请参阅Object.defineProperty()）。描述符具有以下键：configurabletrue 当且仅当该属性描述符的类型可以被改变并且该属性可以从对应对象中删除。 默认为 falseenumerabletrue 当且仅当在枚举相应对象上的属性时该属性显现。 默认为 falsevalue与属性关联的值。可以是任何有效的JavaScript值（数字，对象，函数等）。 默认为 undefined.writabletrue当且仅当与该属性相关联的值可以用assignment operator改变时。 默认为 falseget作为该属性的 getter 函数，如果没有 getter 则为undefined。函数返回值将被用作属性的值。 默认为 undefinedset作为属性的 setter 函数，如果没有 setter 则为undefined。函数将仅接受参数赋值给该属性的新值。 默认为 undefined返回值节 传递给函数的对象。
>
> 语法 Object.defineProperty(obj, prop, descriptor)      obj要在其上定义属性的对象。prop要定义或修改的属性的名称。descriptor将被定义或修改的属性描述符。返回值节    被传递给函数的对象。在ES6中，由于 Symbol类型的特殊性，用Symbol类型的值来做对象的key与常规的定义或修改不同，而Object.defineProperty 是定义key为Symbol的属性的方法之一。
>
> Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。

```js

```



### Object.defineProperties()

> 给对象添加多个属性并分别指定它们的配置。

```js
var obj = {};
Object.defineProperties(obj, {
  'property1': {
    value: true,
    writable: true
  },
  'property2': {
    value: 'Hello',
    writable: false
  }
  // etc. etc.
});
```



### Object.entries()

> 返回给定对象自身可枚举属性的[key, value]数组。
>
> 返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 for...in 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环也枚举原型链中的属性）

```js
const object1 = { foo: 'bar', baz: 42 };
console.log(Object.entries(object1)[1]);
// expected output: Array ["baz", 42]

const object2 = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.entries(object2)[2]);
// expected output: Array ["2", "c"]

const result = Object.entries(object2).sort((a, b) => a - b);
console.log(Object.entries(result)[1]);
// expected output: Array ["1", Array ["1", "b"]]
```





### Object.freeze()

> 冻结对象：其他代码不能删除或更改任何属性。
>
> 可以冻结一个对象。一个被冻结的对象再也不能被修改；冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。此外，冻结一个对象后该对象的原型也不能被修改。freeze() 返回和传入的参数相同的对象。

```js
const object1 = {
  property1: 42
};

const object2 = Object.freeze(object1);

object2.property1 = 33;
// Throws an error in strict mode

console.log(object2.property1);
// expected output: 42
```



### Object.is()

> 比较两个值是否相同。所有 NaN 值都相等（这与==和===不同）。
>
> 判断两个值是否是相同的值

```js
Object.is('foo', 'foo');     // true
Object.is(window, window);   // true

Object.is('foo', 'bar');     // false
Object.is([], []);           // false

var foo = { a: 1 };
var bar = { a: 1 };
Object.is(foo, foo);         // true
Object.is(foo, bar);         // false

Object.is(null, null);       // true

// 特例
Object.is(0, -0);            // false
Object.is(-0, -0);           // true
Object.is(NaN, 0/0);         // true
```



### Object.isExtensible()

> 判断对象是否可扩展。

```js
// 使用Object.freeze是冻结一个对象最方便的方法.
var frozen = { 1: 81 };
Object.isFrozen(frozen) //=== false
Object.freeze(frozen);
Object.isFrozen(frozen) //=== true

// 一个冻结对象也是一个密封对象.
Object.isSealed(frozen) //=== true

// 当然,更是一个不可扩展的对象.
Object.isExtensible(frozen) //=== false
在 ES5 中，如果参数不是一个对象类型，将抛出一个TypeError异常。在 ES2015 中，非对象参数将被视为一个冻结的普通对象，因此会返回true。

Object.isFrozen(1);
// TypeError: 1 is not an object (ES5 code)

Object.isFrozen(1);
// true                          (ES2015 code)
```



### Object.isFrozen()

> 判断对象是否已经冻结。

```js
// 使用Object.freeze是冻结一个对象最方便的方法.
var frozen = { 1: 81 };
Object.isFrozen(frozen) //=== false
Object.freeze(frozen);
Object.isFrozen(frozen) //=== true

// 一个冻结对象也是一个密封对象.
Object.isSealed(frozen) //=== true

// 当然,更是一个不可扩展的对象.
Object.isExtensible(frozen) //=== false
在 ES5 中，如果参数不是一个对象类型，将抛出一个TypeError异常。在 ES2015 中，非对象参数将被视为一个冻结的普通对象，因此会返回true。

Object.isFrozen(1);
// TypeError: 1 is not an object (ES5 code)

Object.isFrozen(1);
// true                          (ES2015 code)
```



### Object.isSealed()

> 判断对象是否已经密封。

```js
// 使用Object.freeze是冻结一个对象最方便的方法.
var frozen = { 1: 81 };
Object.isFrozen(frozen) //=== false
Object.freeze(frozen);
Object.isFrozen(frozen) //=== true

// 一个冻结对象也是一个密封对象.
Object.isSealed(frozen) //=== true

// 当然,更是一个不可扩展的对象.
Object.isExtensible(frozen) //=== false
在 ES5 中，如果参数不是一个对象类型，将抛出一个TypeError异常。在 ES2015 中，非对象参数将被视为一个冻结的普通对象，因此会返回true。

Object.isFrozen(1);
// TypeError: 1 is not an object (ES5 code)

Object.isFrozen(1);
// true                          (ES2015 code)
```





### Object.keys()

> 返回一个包含所有给定对象自身可枚举属性名称的数组。
>
> 会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和使用 for...in 循环遍历该对象时返回的顺序一致 。

```js
// simple array
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']

// array like object with random key ordering
var anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // console: ['2', '7', '100']

// getFoo is a property which isn't enumerable
var myObj = Object.create({}, {
  getFoo: {
    value: function () { return this.foo; }
  } 
});
myObj.foo = 1;
console.log(Object.keys(myObj)); // console: ['foo']
```



### Object.values()

> 返回给定对象自身可枚举值的数组。
>
> 返回一个给定对象自身的所有可枚举属性值的数组，值的顺序与使用for...in循环的顺序相同 ( 区别在于 for-in 循环枚举原型链中的属性 )。

```js
var obj = { foo: 'bar', baz: 42 };
console.log(Object.values(obj)); // ['bar', 42]

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.values(obj)); // ['a', 'b', 'c']

// array like object with random key ordering
// when we use numeric keys, the value returned in a numerical order according to the keys
var an_obj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.values(an_obj)); // ['b', 'c', 'a']

// getFoo is property which isn't enumerable
var my_obj = Object.create({}, { getFoo: { value: function() { return this.foo; } } });
my_obj.foo = 'bar';
console.log(Object.values(my_obj)); // ['bar']

// non-object argument will be coerced to an object
console.log(Object.values('foo')); // ['f', 'o', 'o']
```



### Object.fromEntries() 

> 把键值对列表转换为一个对象。

```js
 Map 转化为 Object
通过 Object.fromEntries， 可以将 Map 转化为 Object:

const map = new Map([ ['foo', 'bar'], ['baz', 42] ]);
const obj = Object.fromEntries(map);
console.log(obj); // { foo: "bar", baz: 42 }

Array 转化为 Object
通过 Object.fromEntries， 可以将 Array 转化为 Object:

const arr = [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ];
const obj = Object.fromEntries(arr);
console.log(obj); // { 0: "a", 1: "b", 2: "c" }
对象转换为Object
Object.fromEntries 是 Object.entries() 的反转函数， 借用 array manipulation methods 可以转换对象，如下：

const object1 = { a: 1, b: 2, c: 3 };

const object2 = Object.fromEntries(
  Object.entries(object1)
  .map(([ key, val ]) => [ key, val * 2 ])
);

console.log(object2);
// { a: 2, b: 4, c: 6 }
```



## 对象

### 对象方法

#### 创建对象的方法

```js
//1.字面量
var obj1 = {name: 'solo obj1'};
obj1// { name: "solo obj1" }

//2.new Object
var obj2 = new Object({name: 'solo obj2'})
obj2// { name: "solo obj2" }

//3.构造函数创建
var M = function(name){
  this.name = name;
}
var obj3 = new M('solo obj3');
obj3// { name: "solo obj3" }
M.prototype.constructor === M // true
M.prototype === obj3.__proto__ // true
//4.Object.create
var p = {name: 'p'};
var obj4 = Object.create(p);
obj4// {}
obj4.name // "p"
```

![](https://images.gitee.com/uploads/images/2020/0920/202923_e68d1728_7984151.png)



#### Object.is()

- 在 Es5 中，比较两个数是否相等，使用的是相等( == )和全等( === ),对两数进行比较。相等会自动转换数据类型(例如，布尔型数据类型在进行比较时，会转换为0或者1，NaN不等于NaN,)。

```js
Object.is(NaN,NaN); //true
Object.is(+0,-0);  //false
```

#### Object.assign(obj1,obj2,obj3,..)

- assign() 方法用于对象的合并
- obj1 属于目标对象，后面的obj对象均为源对象。

```js
var obj = {name:"zhangsan"}
var obj2 = {age:"19"}

Object.assign(obj,obj2); // {name:"zhangsan",age:"19"}
```

- 当目标对象和源对象的属性名称出现冲突时，后面对象的属性会覆盖前面的对象的属性。

```js
var obj = {name:"zhangsan"}
var obj2 = {name:"lisi"}
Object.assign(obj,obj2) // {name: "lisi"}
```

1. 当assign()方法中，只有一个参数，object.assign()方法会返回参数对象。
2. 当assign()方法中的参数不是对象，会先转换为对象，再返回该参数。
3. 当assign()方法中，第一个参数为null、undefined时，因为不能转换为对象，便会报错。（只要null，undefined不是首个参数，就不会报错）

*首个参数处理规则与其他的参数不同，除了首参，不能转换为对象的参数就会跳过。参数为数值、布尔型均会跳过。*



#### Object.assign() -- 深拷贝、浅拷贝

例子1：

```js
var obj = {name:"zhangsan"}
var obj2 = Object.assign({},obj);

console.log(obj) // {name:"zhangsan"}
console.log(obj2) // {name:"zhangsan"}

obj2.name = "lisi"; // {name:"lisi"}
console.log(obj2.name) // "lisi"
console.log(obj.name) // "zhangsan"
```

例子2：

```js
var obj = {name:"zhangsan",info:{age:"18"}}
var obj2 = Object.assign({},obj);

console.log(obj); // {name:"zhangsan",info:{age:"18"}}
console.log(obj2); // {name:"zhangsan",info:{age:"18"}}

obj2.info.age = "19"

console.log(obj2.info.age); // "19"
console.log(obj.info.age); // "19"
```

- 有以上两个例子得出。
  - Object.assign()的第一层属性是属于深拷贝，第二层以上的属性则为浅拷贝。
  - 第二层的会改变原数据



# JavaScript中Object构造函数的方法



## Object.assign()

>  Object构造函数的方法节
>
>  Object.assign() 方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。

```js
const target = { a: 1, b: 2 };
const source = { b: 4, c: 5 };
const returnedTarget = Object.assign(target, source);
console.log(target);
// expected output: Object { a: 1, b: 4, c: 5 }
console.log(returnedTarget);
// expected output: Object { a: 1, b: 4, c: 5 }
```

## Object.create()

> 通过复制一个或多个对象来创建一个新的对象。
>
> Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__。 

```js
const person = {
  isHuman: false,
  printIntroduction: function () {
    console.log(`My name is ${this.name}. Am I human? ${this.isHuman}`);
  }
};
const me = Object.create(person);

me.name = "Matthew"; // "name" is a property set on "me", but not on "person"
me.isHuman = true; // inherited properties can be overwritten

me.printIntroduction();
// expected output: "My name is Matthew. Am I human? true"
```



## Object.defineProperty()

> 使用指定的原型对象和属性创建一个新对象。
>
> 语法   Object.defineProperties(obj, props)   obj在其上定义或修改属性的对象。
>
> Object.defineProperties() 方法直接在一个对象上定义新的属性或修改现有属性，并返回该对象。
>
> props要定义其可枚举属性或修改的属性描述符的对象。
>
> 对象中存在的属性描述符主要有两种：
>
> - 数据描述符和访问器描述符（更多详情，请参阅Object.defineProperty()）。
> - 描述符具有以下键：configurabletrue 当且仅当该属性描述符的类型可以被改变并且该属性可以从对应对象中删除。 
> - 默认为 falseenumerabletrue 当且仅当在枚举相应对象上的属性时该属性显现。 
> - 默认为 falsevalue与属性关联的值。可以是任何有效的JavaScript值（数字，对象，函数等）。 
> - 默认为 undefined.writabletrue当且仅当与该属性相关联的值可以用assignment operator改变时。 
> - 默认为 falseget作为该属性的 getter 函数，如果没有 getter 则为undefined。函数返回值将被用作属性的值。 
> - 默认为 undefinedset作为属性的 setter 函数，如果没有 setter 则为undefined。函数将仅接受参数赋值给该属性的新值。 
> - 默认为 undefined返回值节 传递给函数的对象。



```js
var obj = {};
Object.defineProperties(obj, {
  'property1': {
    value: true,
    writable: true
  },
  'property2': {
    value: 'Hello',
    writable: false
  }
  // etc. etc.
});
```

## Object.defineProperties()

> 给对象添加一个属性并指定该属性的配置。
>
> Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性， 并返回这个对象。
>
> 语法 Object.defineProperty(obj, prop, descriptor)      
>
> - obj要在其上定义属性的对象。
>
> - prop要定义或修改的属性的名称。
>
> - descriptor将被定义或修改的属性描述符。
> - 返回值节    被传递给函数的对象。在ES6中，由于 Symbol类型的特殊性，用Symbol类型的值来做对象的key与常规的定义或修改不同，而Object.defineProperty 是定义key为Symbol的属性的方法之一。







## Object.entries()

> 给对象添加多个属性并分别指定它们的配置。
>
> Object.entries()方法返回一个给定对象自身可枚举属性的键值对数组，其排列与使用 for...in 循环遍历该对象时返回的顺序一致（区别在于 for-in 循环也枚举原型链中的属性）

```js
const object1 = { foo: 'bar', baz: 42 };
console.log(Object.entries(object1)[1]);
// expected output: Array ["baz", 42]

const object2 = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.entries(object2)[2]);
// expected output: Array ["2", "c"]

const result = Object.entries(object2).sort((a, b) => a - b);
console.log(Object.entries(result)[1]);
// expected output: Array ["1", Array ["1", "b"]]
```



## Object.freeze()

> 返回给定对象自身可枚举属性的[key, value]数组。
>
> Object.freeze() 方法可以冻结一个对象。一个被冻结的对象再也不能被修改；冻结了一个对象则不能向这个对象添加新的属性，不能删除已有属性，不能修改该对象已有属性的可枚举性、可配置性、可写性，以及不能修改已有属性的值。此外，冻结一个对象后该对象的原型也不能被修改。freeze() 返回和传入的参数相同的对象。

```js
const object1 = {
  property1: 42
};

const object2 = Object.freeze(object1);

object2.property1 = 33;
// Throws an error in strict mode

console.log(object2.property1);
// expected output: 42
```







## Object.is()

> 冻结对象：其他代码不能删除或更改任何属性。
>
> Object.is() 方法判断两个值是否是相同的值

```js
Object.is('foo', 'foo');     // true
Object.is(window, window);   // true

Object.is('foo', 'bar');     // false
Object.is([], []);           // false

var foo = { a: 1 };
var bar = { a: 1 };
Object.is(foo, foo);         // true
Object.is(foo, bar);         // false

Object.is(null, null);       // true

// 特例
Object.is(0, -0);            // false
Object.is(-0, -0);           // true
Object.is(NaN, 0/0);         // true
```



## Object.isExtensible()

> 　　比较两个值是否相同。所有 NaN 值都相等（这与==和===不同）。

## Object.isFrozen()

> 判断对象是否可扩展。
>
> Object.isFrozen()方法判断一个对象是否被冻结

```js
// 使用Object.freeze是冻结一个对象最方便的方法.
var frozen = { 1: 81 };
Object.isFrozen(frozen) //=== false
Object.freeze(frozen);
Object.isFrozen(frozen) //=== true

// 一个冻结对象也是一个密封对象.
Object.isSealed(frozen) //=== true

// 当然,更是一个不可扩展的对象.
Object.isExtensible(frozen) //=== false
在 ES5 中，如果参数不是一个对象类型，将抛出一个TypeError异常。在 ES2015 中，非对象参数将被视为一个冻结的普通对象，因此会返回true。

Object.isFrozen(1);
// TypeError: 1 is not an object (ES5 code)

Object.isFrozen(1);
// true                          (ES2015 code)
```

## Object.isSealed()

> 判断对象是否已经冻结。





## Object.keys()

> 判断对象是否已经密封。
>
> Object.keys() 方法会返回一个由一个给定对象的自身可枚举属性组成的数组，数组中属性名的排列顺序和使用 for...in 循环遍历该对象时返回的顺序一致 。

```js
// simple array
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']

// array like object with random key ordering
var anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // console: ['2', '7', '100']

// getFoo is a property which isn't enumerable
var myObj = Object.create({}, {
  getFoo: {
    value: function () { return this.foo; }
  } 
});
myObj.foo = 1;
console.log(Object.keys(myObj)); // console: ['foo']
```



## Object.values()

> 返回一个包含所有给定对象自身可枚举属性名称的数组。
>
> Object.values()方法返回一个给定对象自身的所有可枚举属性值的数组，值的顺序与使用for...in循环的顺序相同 ( 区别在于 for-in 循环枚举原型链中的属性 )。

```js
var obj = { foo: 'bar', baz: 42 };
console.log(Object.values(obj)); // ['bar', 42]

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.values(obj)); // ['a', 'b', 'c']

// array like object with random key ordering
// when we use numeric keys, the value returned in a numerical order according to the keys
var an_obj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.values(an_obj)); // ['b', 'c', 'a']

// getFoo is property which isn't enumerable
var my_obj = Object.create({}, { getFoo: { value: function() { return this.foo; } } });
my_obj.foo = 'bar';
console.log(Object.values(my_obj)); // ['bar']

// non-object argument will be coerced to an object
console.log(Object.values('foo')); // ['f', 'o', 'o']
```









## Object.fromEntries() 

> 把键值对列表转换为一个对象。
>
> Map 转化为 Object 
> 通过 Object.fromEntries， 可以将 Map 转化为 Object:

```js
const map = new Map([ ['foo', 'bar'], ['baz', 42] ]);
const obj = Object.fromEntries(map);
console.log(obj); // { foo: "bar", baz: 42 }

Array 转化为 Object
通过 Object.fromEntries， 可以将 Array 转化为 Object:

const arr = [ ['0', 'a'], ['1', 'b'], ['2', 'c'] ];
const obj = Object.fromEntries(arr);
console.log(obj); // { 0: "a", 1: "b", 2: "c" }
对象转换为Object
Object.fromEntries 是 Object.entries() 的反转函数， 借用 array manipulation methods 可以转换对象，如下：

const object1 = { a: 1, b: 2, c: 3 };

const object2 = Object.fromEntries(
  Object.entries(object1)
  .map(([ key, val ]) => [ key, val * 2 ])
);

console.log(object2);
// { a: 2, b: 4, c: 6 }


```

