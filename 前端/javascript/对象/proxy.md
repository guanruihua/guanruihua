# Proxy

> 在 ES6 标准中新增的一个非常强大的功能是 Proxy，它可以自定义一些常用行为如查找、赋值、枚举、函数调用等。通过 Proxy 这个名称也可以看出来它包含了“代理”的含义，只要有“代理”的诉求都可以考虑使用 Proxy 来实现。

**1.基本语法**

```js
let p = new Proxy(target, handler)
```

| 参数    | 含义                                                         | 必选 |
| ------- | ------------------------------------------------------------ | ---- |
| target  | 用 Proxy 包装的目标对象（可以是任何类型的对象，包括原生数组，函数，甚至另一个代理） | y    |
| handler | 一个对象，其属性是当执行一个操作时定义代理的行为的函数       | y    |

**2.常用拦截操作**

- get
  拦截对象属性的读取，比如proxy.foo和proxy['foo']。

  ```js
  let arr = [7, 8, 9]
  arr = new Proxy(arr, {
    get(target, prop) {
        // console.log(target, prop)
        return prop in target ? target[prop] : 'error'
    }
  })
  console.log(arr[1])
  console.log(arr[10])
  ```

  ```js
  let dict = {
    'hello': '你好',
    'world': '世界'
  }
  dict = new Proxy(dict, {
    get(target, prop) {
        return prop in target ? target[prop] : prop
    }
  })
  console.log(dict['world'])
  console.log(dict['imooc'])
  ```

- set
  拦截对象属性的设置，比如proxy.foo = v或proxy['foo'] = v，返回一个布尔值。

  ```js
  let arr = []
  arr = new Proxy(arr, {
    set(target, prop, val) {
        if (typeof val === 'number') {
            target[prop] = val
            return true
        } else {
            return false
        }
    }
  })
  arr.push(5)
  arr.push(6)
  console.log(arr[0], arr[1], arr.length)
  ```

- has
  拦截propKey in proxy的操作，返回一个布尔值。

  ```js
  let range = {
    start: 1,
    end: 5
  }
  
  range = new Proxy(range, {
    has(target, prop) {
        return prop >= target.start && prop <= target.end
    }
  })
  console.log(2 in range)
  console.log(9 in range)
  ```

- ownKeys
  拦截Object.getOwnPropertyNames(proxy)、Object.getOwnPropertySymbols(proxy)、Object.keys(proxy)、for...in循环，返回一个数组。该方法返回目标对象所有自身的属性的属性名，而Object.keys()的返回结果仅包括目标对象自身的可遍历属性。

  ```js
  let userinfo = {
    username: 'xxx',
    age: 18,
    _password: '***'
  }
  userinfo = new Proxy(userinfo, {
    ownKeys(target) {
        return Object.keys(target).filter(key => !key.startsWith('_'))
    }
  })
  
  // for (let key in userinfo) {
  //     console.log(key)
  // }
  console.log(Object.keys(userinfo))
  ```

- deleteProperty
  拦截delete proxy[propKey]的操作，返回一个布尔值。

  ```js
  let user = {
    name: 'xxx',
    age: 18,
    _password: '***'
  }
  user = new Proxy(user, {
    get(target, prop) {
        if (prop.startsWith('_')) {
            throw new Error('不可访问')
        } else {
            return target[prop]
        }
    },
    set(target, prop, val) {
        if (prop.startsWith('_')) {
            throw new Error('不可访问')
        } else {
            target[prop] = val
            return true
        }
    },
    deleteProperty(target, prop) { // 拦截删除
        if (prop.startsWith('_')) {
            throw new Error('不可删除')
        } else {
            delete target[prop]
            return true
        }
    },
    ownKeys(target) {
        return Object.keys(target).filter(key => !key.startsWith('_'))
    }
  })
  console.log(user.age)
  console.log(user._password)
  user.age = 18
  console.log(user.age)
  try {
    user._password = 'xxx'
  } catch (e) {
    console.log(e.message)
  }
  
  try {
    // delete user.age
    delete user._password
  } catch (e) {
    console.log(e.message)
  }
  console.log(user.age)
  
  for (let key in user) {
    console.log(key)
  }
  ```

- apply
  拦截 Proxy 实例作为函数调用的操作，比如proxy(...args)、proxy.call(object, ...args)、proxy.apply(...)。

  ```js
  let sum = (...args) => {
    let num = 0
    args.forEach(item => {
        num += item
    })
    return num
  }
  
  sum = new Proxy(sum, {
    apply(target, ctx, args) {
        return target(...args) * 2
    }
  })
  console.log(sum(1, 2))
  console.log(sum.call(null, 1, 2, 3))
  console.log(sum.apply(null, [1, 2, 3]))
  ```

- construct
  拦截 Proxy 实例作为构造函数调用的操作，比如new proxy(...args)。

  ```js
  let User = class {
    constructor(name) {
        this.name = name
    }
  }
  User = new Proxy(User, {
    construct(target, args, newTarget) {
        console.log('construct')
        return new target(...args)
    }
  })
  console.log(new User('imooc'))
  ```