# Math

## **Math扩展**

- Math.trunc()
  方法用于去除一个数的小数部分，返回整数部分。

  ```js
  console.log(Math.trunc(5.5)) // 5
  console.log(Math.trunc(-5.5)) // -5
  console.log(Math.trunc(true)) // 1
  console.log(Math.trunc(false)) // 0
  console.log(Math.trunc(NaN)) // NaN
  console.log(Math.trunc(undefined)) // NaN
  console.log(Math.trunc()) // NaN
  ```

- Math.sign()
  方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。

它会返回五种值。

- 参数为正数，返回+1

- 参数为负数，返回-1

- 参数为 0，返回0

- 参数为-0，返回-0

- 其他值，返回NaN

  ```js
  console.log(Math.sign(5)) // 1
  console.log(Math.sign(-5)) // -1
  console.log(Math.sign(0)) // 0
  console.log(Math.sign(NaN)) // NaN
  console.log(Math.sign(true)) // 1
  console.log(Math.sign(false)) // 0
  ```

- Math.cbrt()
  方法用于计算一个数的立方根。

  ```js
  console.log(Math.cbrt(8)) // 2
  
  console.log(Math.cbrt('xx')) // NaN
  ```
