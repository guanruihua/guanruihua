# Generator

Generators 是可以用来控制迭代器的函数。它们可以暂停，然后在任何时候恢复。如果这句话不好理解，可以看下接下来的示例。

1. 常规循环

   ```js
   for (let i = 0; i < 5; i += 1) {
    console.log(i)
   }
   // this will return immediately 0 -> 1 -> 2 -> 3 -> 4
   ```

2. 利用 Generator

   ```js
   function* generatorForLoop() {
    for (let i = 0; i < 5; i += 1) {
        yield console.log(i)
    }
   }
   
   const genForLoop = generatorForLoop()
   
   console.log(genForLoop.next()) // first console.log - 0
   console.log(genForLoop.next()) // 1
   console.log(genForLoop.next()) // 2
   console.log(genForLoop.next()) // 3
   console.log(genForLoop.next()) // 4
   ```

   对比下代码，常规的循环只能一次遍历完所有值，Generator 可以通过调用 next 方法拿到依次遍历的值，让遍历的执行变得“可控”。

**1、基本语法**

```js
function* gen() {
    yield 1
    yield 2
    yield 3
}

let g = gen()
// "Generator { }"
```

这个是 Generator 的定义方法，有几个点值得注意：

- 比普通函数多一个 *
- 函数内部用 yield 来控制程序的执行的“暂停”
- 函数的返回值通过调用 next 来“恢复”程序执行

Generator 函数的定义不能使用箭头函数，否则会触发 SyntaxError 错误

```js
let generator = * () => {} // SyntaxError
let generator = () * => {} // SyntaxError
let generator = ( * ) => {} // SyntaxError
```

**2、yield 表达式**
yield 关键字用来暂停和恢复一个生成器函数

- yield 表达式的返回值是 undefined，但是遍历器对象的 next 方法可以修改这个默认值。

- Generator 对象的 next 方法，遇到 yield 就暂停，并返回一个对象，这个对象包括两个属性：value 和 done。

  ```js
   function* gen() {
      let val
      val = yield 1
      console.log( `1:${val}` ) // 1:undefined
      val = yield 2
      console.log( `2:${val}` ) // 2:undefined
      val = yield 3
      console.log( `3:${val}` ) // 3:undefined
  }
  
  var g = gen()
  
  console.log(g.next()) // {value: 1, done: false}
  console.log(g.next()) // {value: 2, done: false}
  console.log(g.next()) // {value: 3, done: false}
  console.log(g.next()) // {value: undefined, done: true}
  ```

  **3、方法**

Generator 对象有几个方法，next、return、throw。

- next([value])
  Generator 对象通过 next 方法来获取每一次遍历的结果，这个方法返回一个对象，这个对象包含两个属性：value 和 done。value 是指当前程序的运行结果，done 表示遍历是否结束。

其实 next 是可以接受参数的，这个参数可以让你在 Generator 外部给内部传递数据，而这个参数就是作为 yield 的返回值。

```js
function* gen() {
      var val = 100
      while (true) {
          console.log( `before ${val}` )
          val = yield val
          console.log( `return ${val}` )
      }
  }

  var g = gen()
  console.log(g.next(20).value)
  // before 100
  // 100
  console.log(g.next(30).value)
  // return 30
  // before 30
  // 30
  console.log(g.next(40).value)
  // return 40
  // before 40
  // 40
```

- return()
  return 方法可以让 Generator 遍历终止，有点类似 for 循环的 break。

  ```js
  function* gen() {
  yield 1
  yield 2
  yield 3
  }
  
  var g = gen()
  
  console.log(g.next()) // {value: 1, done: false}
  console.log(g.return()) // {value: undefined, done: true}
  console.log(g.next()) // {value: undefined, done: true}
  ```

- throw()
  可以通过 throw 方法在 Generator 外部控制内部执行的“终断”。

  ```js
  function* gen() {
    while (true) {
        try {
            yield 42
        } catch (e) {
            console.log(e.message)
        }
    }
  }
  
  let g = gen()
  console.log(g.next()) // { value: 42, done: false }
  console.log(g.next()) // { value: 42, done: false }
  console.log(g.next()) // { value: 42, done: false }
  // 中断操作
  g.throw(new Error('break'))
  
  console.log(g.next()) // {value: undefined, done: true}
  ```