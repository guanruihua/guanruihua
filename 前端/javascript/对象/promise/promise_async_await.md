# Promise async await

## Promise

> - 特点
>   - 链式调用
>
> （1）对象的状态不受外界影响。
>
> ​ `Promise`对象代表一个异步操作，有三种状态：`Pending`（进行中）、`Resolved`（已完成，又称Fulfilled）和`Rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是`Promise`这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。
>
> （2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。
>
> ​ `Promise`对象的状态改变，只有两种可能：从`Pending`变为`Resolved`和从`Pending`变为`Rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果。就算改变已经发生了，你再对`Promise`对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。
>
> 有了`Promise`对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，`Promise`对象提供统一的接口，使得控制异步操作更加容易。
>
> ​ `Promise`也有一些缺点。首先，无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。第三，当处于`Pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。
>
> 如果某些事件不断地反复发生，一般来说，使用stream模式是比部署`Promise`更好的选择。

```js
var promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```

### Promise.prototype.then()

> - then方法返回的是一个新的Promise实例
> - 第一个回调函数,会将返回结果作为参数,传入第二个回调函数

```js
promise.then(function(value) {
  // success
}, function(error) {
  // failure
});
```

### Promise.prototype.catch()

> `Promise.prototype.catch`方法是`.then(null, rejection)`的别名，用于指定发生错误时的回调函数。

### Promise.all()

> 将多个Promise实例,包装成一个新的Promise实例
>
> ```js
> var p = Promise.all([p1, p2, p3]);
> 
> /*
> （1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，
>   此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
> （2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，
>   此时第一个被reject的实例的返回值，会传递给p的回
> 调函数。
> */
> 
> let p1 = new Promise((resolve, reject) => {
>   resolve(11);
> })
> let p2 = new Promise(((resolve, reject) => {
>   resolve(12);
> }))
> 
> let p3 = new Promise(((resolve, reject) => {
>   resolve(13);
> }))
> 
> let p = Promise.all([p1, p2, p3]);
> p.then(res => {
>   console.log('then', res);
> }).catch(res => {
>   console.log("catch", res)
> });
> // 打印
> then [11, 12, 13]
> ```
>
> ```js
> let p1 = new Promise((resolve, reject) => {
>   resolve(11);
> })
> let p2 = new Promise(((resolve, reject) => {
>   resolve(12);
> }))
> 
> let p3 = new Promise(((resolve, reject) => {
>   reject(33)
> }))
> 
> let p = Promise.all([p1, p2, p3]);
> p.then(res => {
>   console.log('then', res);
> }).catch(res => {
>   console.log("catch", res)
> });
> // 打印
> catch 33
> 
> ```

### Promise.race()

> 就是通过竞赛来比较, 看谁的转态先发生改变
>
> ```js
> var p = Promise.race([p1, p2, p3]);
> /*
>  p1, p2, p3 中有一个实例率先改变状态,p就会跟着改变,先改变的Promise实例的返回值,就返回给p的回调函数
> */
> ```
>
>

### Promise.resolve()

> 将现有对象转换为Promise对象
>
> `var jsPromise = Promise.resolve($.ajax('/whatever.json'));`
> 是一个状态为`fulfilled`的Promise实例
>
> - 不会触发catch， 可以正常出发then
> - 若是该方法触发`throw new Error('error')`, 可以将状态修改为`rejected`

### Promise.reject()

```js
var p = Promise.reject('出错了');
// 等同于
var p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s){
  console.log(s)
});
// 出错了
```

### done()

> Promise对象的回调链，不管以`then`方法或`catch`方法结尾，要是最后一个方法抛出错误，都有可能无法捕捉到（因为Promise内部的错误不会冒泡到全局）。因此，我们可以提供一个`done`方法，总是处于回调链的尾端，保证抛出任何可能出现的错误。

```js
asyncFunc()
  .then(f1)
  .catch(r1)
  .then(f2)
  .done();
```

### finally()

>不管状态,都一定会执行

```js
server.listen(0)
  .then(function () {
    // run test
  })
  .finally(server.stop);
```

### Promise.try()

```js
Promise.try(database.users.get({id: userId}))
  .then(...)
  .catch(...)
```

### 例子

#### promise, then , catch

```js
function demo() {
 let pr = new Promise((resolve, reject) => {
    // 1. 这里使用setTimeout模拟异步, 可以证明promise里面还是同步执行的, 这里还是触发了 reject
    // 2. promise是通过resolve或reject传递数据给下一层
    // 3. then 是通过 return传输给下一层
  let flag = false;
  setTimeout(() => {
   flag = true;
  }, 500);
  if (flag)
   resolve("true")
  else
   reject("false")
 });
 return pr.then(res => {
  return "then return data"
 }).catch(res => {
  console.log('catch data', res);
  return "data"// catch 使用return 也可以返回数据
 });
}

demo().then(res => {
 console.log("show data ", res)
})
// 输出
catch data false
show data data
// flag　改为 true
show data then return data
```

#### Promise对象实现Ajax操作的例子

```js
var getJSON = function(url) {
  var promise = new Promise(function(resolve, reject){
    var client = new XMLHttpRequest();
    client.open("GET", url);
    client.onreadystatechange = handler;
    client.responseType = "json";
    client.setRequestHeader("Accept", "application/json");
    client.send();

    function handler() {
      if (this.readyState !== 4) {
        return;
      }
      if (this.status === 200) {
        resolve(this.response);
      } else {
        reject(new Error(this.statusText));
      }
    };
  });

  return promise;
};

getJSON("/posts.json").then(function(json) {
  console.log('Contents: ' + json);
}, function(error) {
  console.error('出错了', error);
});
```

## async, await

> 异步

```javascript
async function main() {
  try {
    var val1 = await firstStep();
    var val2 = await secondStep(val1);
    var val3 = await thirdStep(val1, val2);

    console.log('Final: ', val3);
  }
  catch (err) {
    console.error(err);
  }
}
```

### Promise

**1、基本语法**

> Promise 就是为了解决“回调地狱”问题的，它可以将异步操作的处理变得很优雅。回调地狱，代码难以维护， 常常第一个的函数的输出是第二个函数的输入这种现象promise可以支持多个并发的请求，获取并发请求中的数据这个promise可以解决异步的问题，本身不能说promise是异步的。

创建Promise实例。

```
const promise = new Promise(function(resolve, reject) {
    // ... some code

    if ( /* 异步操作成功 */ ) {
        resolve(value)
    } else {
        reject(error)
    }
})
```

Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

- 处理结果正常的话，调用resolve(处理结果值)，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去

- 处理结果错误的话，调用reject(Error对象)，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。
  Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。

  ```
  promise.then(function(value) {
    // success
  }, function(error) {
    // failure
  })
  ```

  Promise 内部是有状态的(pending、fulfilled、rejected)，Promise 对象根据状态来确定执行哪个方法。Promise 在实例化的时候状态是默认 pending 的，当异步操作是完成的，状态会被修改为 fulfilled，如果异步操作遇到异常，状态会被修改为 rejected。

**2、Promise.prototype.then()**

```
var promise = new Promise(function(resolve, reject) {
    resolve('传递给then的值')
})
promise.then(function(value) {
    console.log(value)
}, function(error) {
    console.error(error)
})
```

- 当 handler 返回一个正常值的时候，这个值会传递给 Promise 对象的 onFulfilled 方法。
- 定义的 handler 中产生异常的时候，这个值则会传递给 Promise 对象的 onRejected 方法。

**3、Promise.prototype.catch()**

捕获异常是程序质量保障最基本的要求，可以使用 Promise 对象的 catch 方法来捕获异步操作过程中出现的任何异常

```
function test() {
    return new Promise((resolve, reject) => {
        reject(new Error('es'))
    })
}

test().catch((e) => {
    console.log(e.message) // es
})
```

**4、Promise.resolve()**

一般情况下我们都会使用 new Promise() 来创建 Promise 对象，但是除此之外我们也可以使用其他方法。

在这里，我们将会学习如何使用 Promise.resolve 和 Promise.reject 这两个方法。

静态方法 Promise.resolve(value) 可以认为是 new Promise() 方法的快捷方式。

比如 Promise.resolve(42) 可以认为是以下代码的语法糖。

```
new Promise(function(resolve) {
    resolve(42)
})
```

方法 Promise.resolve(value) 的返回值也是一个 Promise 对象，所以我们可以像下面那样接着对其返回值进行 .then 调用。

```
Promise.resolve(42).then(function(value) {
    console.log(value)
})
```

**5、Promise.reject()**

Promise.reject(error) 是和 Promise.resolve(value) 类似的静态方法，是 new Promise() 方法的快捷方式。

比如 Promise.reject(new Error("出错了")) 就是下面代码的语法糖形式。

```
new Promise(function(resolve, reject) {
    reject(new Error('出错了'))
})
```

这段代码的功能是调用该Promise 对象通过then指定的 onRejected 函数，并将错误（Error）对象传递给这个 onRejected 函数。

```
Promise.reject(new Error('BOOM!'))
```

**6、Promise.all()**

```
var p1 = Promise.resolve(1)
var p2 = Promise.resolve(2)
var p3 = Promise.resolve(3)
Promise.all([p1, p2, p3]).then(function(results) {
    console.log(results) // [1, 2, 3]
})
```

Promise.all 生成并返回一个新的 Promise 对象，所以它可以使用 Promise 实例的所有方法。参数传递promise数组中所有的 Promise 对象都变为resolve的时候，该方法才会返回， 新创建的 Promise 则会使用这些 promise 的值。

如果参数中的任何一个promise为reject的话，则整个Promise.all调用会立即终止，并返回一个reject的新的 Promise 对象。

由于参数数组中的每个元素都是由 Promise.resolve 包装（wrap）的，所以Promise.all 可以处理不同类型的 Promise 对象。

**7、Promise.race()**

```
var p1 = Promise.resolve(1)
var p2 = Promise.resolve(2)
var p3 = Promise.resolve(3)
Promise.race([p1, p2, p3]).then(function(value) {
    console.log(value) // 1
})
```

Promise.race 生成并返回一个新的 Promise 对象。

参数 promise 数组中的任何一个 Promise 对象如果变为 resolve 或者 reject 的话， 该函数就会返回，并使用这个 Promise 对象的值进行 resolve 或者 reject。
