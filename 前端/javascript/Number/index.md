# Number

## BigInt

> 在 ES10 增加了新的原始数据类型：BigInt，表示一个任意精度的整数，可以表示超长数据，可以超出2的53次方。

Js 中 Number类型只能安全的表示-(2^53-1)至 2^53-1 范的值

```
console.log(2 ** 53) // es7 幂运算符
console.log(Number.MAX_SAFE_INTEGER) // 最大值-1
```

**使用 BigInt 有两种方式：**

- 方式一：数字后面增加n

  ```
  const bigInt = 9007199254740993n
  console.log(bigInt)
  console.log(typeof bigInt) // bigint
  
  console.log(1n == 1) // true
  console.log(1n === 1) // false
  ```

- 方式二：使用 BigInt 函数

  ```
  const bigIntNum = BigInt(9007199254740993n)
  console.log(bigIntNum)
  ```

  ### Promise.allSettled()

  > 学习了ES新特性，我们都知道 Promise.all() 具有并发执行异步任务的能力。但它的最大问题就是如果其中某个任务出现异常(reject)，所有任务都会挂掉，Promise直接进入reject 状态。而 Promise.allSettled 返回一个在所有给定的promise已被决议或被拒绝后决议的promise，并带有一个对象数组，每个对象表示对应的promise结果。

  ```
  Promise.allSettled([
    Promise.reject({
        code: 500,
        msg: '服务异常'
    }),
    Promise.resolve({
        code: 200,
        data: ['1', '2', '3']
    }),
    Promise.resolve({
        code: 200,
        data: ['4', '5', '6']
    })
  ]).then(res => {
    console.log(res)
    // console.log('成功')
    const data = res.filter(item => item.status === 'fulfilled')
    console.log(data)
  }).catch(err => {
    console.log(err)
    console.log('失败')
  })
  ```

## 幂运算符

求幂运算

```
console.log(2 ** 10) // 1024
```

## **二进制与八进制**

- JS中如何把十进制转化为二进制？

  ```
  const a = 5 // 101
  
  console.log(a.toString(2))
  ```

- 如何把八进制转化为二进制？

  ```
  const b = 101
  
  console.log(parseInt(b, 2))
  ```

  ES6 提供了二进制和八进制数值的新的写法，分别用前缀0b（或0B）和0o（或0O）表示。

  ```
  const a = 0B0101
  console.log(a)
  
  const b = 0O777
  console.log(b)
  ```

  **2.新增方法**

- Number.isFinite()
  用来检查一个数值是否为有限的（finite），即不是Infinity。

  ```
  Number.isFinite(15) // true
  Number.isFinite(0.8) // true
  Number.isFinite(NaN) // false
  Number.isFinite(Infinity) // false
  Number.isFinite(-Infinity) // false
  Number.isFinite('foo') // false
  Number.isFinite('15') // false
  Number.isFinite(true) // false
  ```

- Number.isNaN()
  用来检查一个值是否为NaN。

  ```
  Number.isNaN(NaN) // true
  Number.isNaN(15) // false
  Number.isNaN('15') // false
  Number.isNaN(true) // false
  Number.isNaN(9 / NaN) // true
  Number.isNaN('true' / 0) // true
  Number.isNaN('true' / 'true') // true
  ```

- Number.parseInt()
  ES6 将全局方法parseInt()移植到Number对象上面，行为完全保持不变。 这样做的目的，是逐步减少全局性方法，使得语言逐步模块化。

  ```
  // ES5的写法
  parseInt('12.34') // 12
  
  // ES6的写法
  Number.parseInt('12.34') // 12
  ```

- Number.parseFloat()
  ES6 将全局方法parseFloat()移植到Number对象上面，行为完全保持不变。这样做的目的，是逐步减少全局性方法，使得语言逐步模块化。

  ```
  // ES5的写法
  parseFloat('123.45#') // 123.45
  
  // ES6的写法
  Number.parseFloat('123.45#') // 123.45
  ```

- Number.isInteger()
  用来判断一个数值是否为整数。

  ```
  Number.isInteger(25) // true
  Number.isInteger(25.1) // false
  
  Number.isInteger() // false
  Number.isInteger(null) // false
  Number.isInteger('15') // false
  Number.isInteger(true) // false
  ```

- Number.MAX_SAFE_INTEGER

  ```
  Number.MAX_SAFE_INTEGER === Math.pow(2, 53) - 1 // true
  
  Number.MAX_SAFE_INTEGER === 9007199254740991 // true
  ```

- Number.MIN_SAFE_INTEGER

  ```
  Number.MIN_SAFE_INTEGER === -Number.MAX_SAFE_INTEGER // true
  
  Number.MIN_SAFE_INTEGER === -9007199254740991 // true
  ```

- Number.isSafeInteger()
  JavaScript 能够准确表示的整数范围在-2^53到2^53之间（不含两个端点），超过这个范围，无法精确表示这个值。

  ```
  Math.pow(2, 53) // 9007199254740992
  
  Math.pow(2, 53) === Math.pow(2, 53) + 1 // true
  ```