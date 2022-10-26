# Reflect

Reflect对象与Proxy对象一样，也是 ES6 为了操作对象而提供的新 API。

**1.设计目的**

- 将Object属于语言内部的方法放到Reflect上

  ```js
  let obj = {}
  let newVal = ''
  Reflect.defineProperty(obj, 'name', {
    get() {
        return newVal
    },
    set(val) {
        console.log('set')
        // this.name = val
        newVal = val
    }
  })
  obj.name = 'es'
  console.log(obj.name)
  ```

- 修改某些Object方法的返回结果，让其变得更合理

  ```js
  // 老写法
  try {
    Object.defineProperty(target, property, attributes)
    // success
  } catch (e) {
    // failure
  }
  
  // 新写法
  if (Reflect.defineProperty(target, property, attributes)) {
    // success
  } else {
    // failure
  }
  ```

- 让Object操作变成函数行为

  ```js
  // 老写法
  'assign' in Object // true
  
  // 新写法
  Reflect.has(Object, 'assign') // true
  ```

- Reflect对象的方法与Proxy对象的方法一一对应，只要是Proxy对象的方法，就能在Reflect对象上找到对应的方法。

  ```js
  Proxy(target, {
    set: function(target, name, value, receiver) {
        var success = Reflect.set(target, name, value, receiver)
        if (success) {
            console.log('property ' + name + ' on ' + target + ' set to ' + value)
        }
        return success
    }
  })
  ```

  Reflect 是一个内置的对象，它提供拦截 JavaScript 操作的方法，这些方法与处理器对象的方法相同。Reflect不是一个函数对象，因此它是不可构造的。

**2、常用方法**

- Reflect.apply()

  > Reflect.apply(target, thisArgument, argumentsList),target为目标函数；thisArgument为target函数调用时绑定的this对象；argumentsList为target函数调用时传入的实参列表，该参数应该是一个类数组的对象

```js
Reflect.apply(Math.floor, undefined, [1.75])
// 1

Reflect.apply(String.fromCharCode, undefined, [104, 101, 108, 108, 111])
// "hello"

Reflect.apply(RegExp.prototype.exec, /ab/, ['confabulation']).index
// 4

Reflect.apply(''.charAt, 'ponies', [3])
// "i"
```

- Reflect.construct()
  Reflect.construct() 方法的行为有点像 new 操作符 构造函数 ， 相当于运行 new target(...args)

  ```js
  var d = Reflect.construct(Date, [1776, 6, 4])
  d instanceof Date // true
  d.getFullYear() // 1776
  ```

- Reflect.defineProperty()
  静态方法 Reflect.defineProperty() 基本等同于 Object.defineProperty() 方法，唯一不同是返回 Boolean 值。

  ```js
  const student = {}
  Reflect.defineProperty(student, 'name', {
    value: 'Mike'
  }) // true
  student.name // "Mike"
  ```

- Reflect.deleteProperty()
  Reflect.deleteProperty 允许你删除一个对象上的属性。返回一个 Boolean 值表示该属性是否被成功删除。它几乎与非严格的 delete operator 相同。

  ```js
  var obj = {
    x: 1,
    y: 2
  }
  Reflect.deleteProperty(obj, "x") // true
  obj // { y: 2 }
  
  var arr = [1, 2, 3, 4, 5]
  Reflect.deleteProperty(arr, "3") // true
  arr // [1, 2, 3, , 5]
  
  // 如果属性不存在，返回 true
  Reflect.deleteProperty({}, "foo") // true
  
  // 如果属性不可配置，返回 false
  Reflect.deleteProperty(Object.freeze({
    foo: 1
  }), "foo") // false
  ```

- Reflect.get()
  Reflect.get() 方法的工作方式，就像从 object (target[propertyKey]) 中获取属性，但它是作为一个函数执行的。

  ```js
  // Object
  var obj = {
    x: 1,
    y: 2
  }
  Reflect.get(obj, 'x') // 1
  
  // Array
  Reflect.get(['zero', 'one'], 1) // "one"
  
  // Proxy with a get handler
  var x = {
    p: 1
  }
  var obj = new Proxy(x, {
    get(t, k, r) {
        return k + 'bar'
    }
  })
  Reflect.get(obj, 'foo') // "foobar"
  ```

- Reflect.getOwnPropertyDescriptor()
  静态方法 Reflect.getOwnPropertyDescriptor() 与 Object.getOwnPropertyDescriptor() 方法相似。如果在对象中存在，则返回给定的属性的属性描述符，否则返回 undefined。

  ```js
  Reflect.getOwnPropertyDescriptor({
    x: 'hello'
  }, 'x')
  // {value: "hello", writable: true, enumerable: true, configurable: true}
  
  Reflect.getOwnPropertyDescriptor({
    x: 'hello'
  }, 'y')
  // undefined
  
  Reflect.getOwnPropertyDescriptor([], 'length')
  // {value: 0, writable: true, enumerable: false, configurable: false}
  ```

- 更多方法可以参考[Reflect](https://link.segmentfault.com/?url=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FReference%2FGlobal_Objects%2FReflect)