

# Promise原理模拟

## 核心逻辑分析

1. Promise 就是一个类 在执行这个类的时候 需要传递一个执行器进去 执行器会立即执行
2. Promise 中有三种状态 分别为 成功 fulfilled 失败 rejected 等待 pending
   - pending -> fulfilled
   - pending -> rejected
   - 一旦状态确定就不可更改`
3. resolve和reject函数是用来更改状态的
   - resolve: fulfilled
   - reject: rejected
4. then方法内部做的事情就判断状态 如果状态是成功 调用成功的回调函数 如果状态是失败 调用失败回调函数 then方法是被定义在原型对象中的
5. then成功回调有一个参数 表示成功之后的值 then失败回调有一个参数 表示失败后的原因
6. 同一个promise对象下面的then方法是可以被调用多次的
7. then方法是可以被链式调用的, 后面then方法的回调函数拿到值的是上一个then方法的回调函数的返回值

```js
const PENDING = 'pending'; // 等待
const FULFILLED = 'fulfilled'; // 成功
const REJECTED = 'rejected'; // 失败

class MyPromise {
    constructor(executor) {
        executor(this.resolve, this.reject); 
    }

    status = PENDING; // 状态
    value = undefined; // then函数成功回调需要的参数
    reason = undefined; // then函数失败回调需要的参数

    resolve = value => {
        // 如果状态不是等待 阻止程序向下执行
        if(this.status !== PENDING) return;
        // 将状态更改为成功
        this.status = FULFILLED;
        // 保存成功之后的值
        this.value = value;
    }

    reject = reason => {
        // 如果状态不是等待 阻止程序向下执行
        if(this.status !== PENDING) return;
        // 将状态更改为失败
        this.status = REJECTED;
        // 保存失败后的原因
        this.reason = reason;
    }

    then = (successCallback, failCallback) => {
        // 判断状态
        if(this.status === FULFILLED) {
            successCallback(this.value);
        } else if(this.status === REJECTED) {
            failCallback(this.reason);
        }
    }
}

module.exports = MyPromise;
```

## 异步调用解决

### 1. 场景

```js
const promise = new MyPromise((resolve, reject) => {
    setTimeout(() => {
        resolve('成功');
    }, 2000);
});
promise.then(value => {
    console.log(value);
}, reason => {
    console.log(reason);
});
```

### 2. 实现步骤分析

- 定时器中回调函数为异步执行代码
- then函数执行时状态还是pending
- 即then函数中需要增加状态为pending时的执行逻辑，将成功和失败的回调存储起来在相应的resolve和reject函数中调用

### 3. 代码实现

```js
class MyPromise {
    ...
    successCallback = undefined; // 成功回调
    failCallback = undefined; // 失败回调

    resolve = value => {
        ...
        // 成功回调存在则调用
        this.successCallback && this.successCallback(value);
    }

    reject = reason => {
        ...
        // 失败回调存在则调用
        this.failCallback && this.failCallback(reason);
    }

    then = (successCallback, failCallback) => {
        // 判断状态
        if(this.status === FULFILLED) {
            ...
        } else {
            // 等待
            // 将成功和失败回调存储起来
            this.successCallback = successCallback;
            this.failCallback = failCallback;
        }
    }
}

```

## then的多次调用

### 1. 场景

```js
promise.then(value => {
    console.log(value);
});
promise.then(value => {
    console.log(value);
});
promise.then(value => {
    console.log(value);
});

```

### 2. 实现步骤分析

- then多次调用同步代码不需要做特殊处理，而异步的是需要特殊处理的
- 即在then多次调用时，将每次调用的回调都存储起来
- 当状态变成成功或失败时，再依次去执行存储起来的回调

### 3. 代码实现

```js
class MyPromise {
    ...
    successCallback = []; // 成功回调
    failCallback = []; // 失败回调

    resolve = value => {
        ...
        // 成功回调存在则调用
        // this.successCallback && this.successCallback(value);
        while (this.successCallback.length) this.successCallback.shift()(this.value)
    }

    reject = reason => {
        ...
        // 失败回调存在则调用
        // this.failCallback && this.failCallback(reason);
        while (this.failCallback.length) this.failCallback.shift()(this.reason)
    }

    then = (successCallback, failCallback) => {
        // 判断状态
        if(this.status === FULFILLED) {
            ...
        } else {
            // 等待
            // 将成功和失败回调存储起来
            this.successCallback.push(successCallback);
            this.failCallback.push(failCallback);
        }
    }
}

```

## then的链式调用

### 1. 场景

```js
promise.then(value => {
    console.log(value);
    return 100;
}).then(value => {
    console.log(value);
}).then(value => {
    console.log(value);
});

```

### 2. 实现步骤分析

- then方法的链式调用必须每一个then方法都返回一个Promise对象
- 把上一个then方法的返回值（需要判断这个返回值是不是Promise对象）传递给下一个then方法

### 3. 代码实现

```js
class MyPromise {
    ...
    then = (successCallback, failCallback) => {
        const promise2 = new MyPromise((resolve, reject) => {
            // 判断状态
            if (this.status === FULFILLED) {
                const x = successCallback(this.value);
                // 判断x的值是普通值还是Promise对象
                // 如果是普通值，直接调用resolve传参
                // 如果是Promise对象 则查看promise对象返回的结果
                // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
                resolvePromise(x, resolve, reject);
            } else if (this.status === REJECTED) {
                failCallback(this.reason);
            } else {
                // 等待
                // 将成功和失败回调存储起来
                this.successCallback.push(successCallback);
                this.failCallback.push(failCallback);
            }
        });

        return promise2;
    }
}
function resolvePromise(x, resolve, reject) {
    if(x instanceof MyPromise) {
        // promise 对象
        // x.then(value => resolve(value), reason => reject(reason));
        x.then(resolve, reject);
    } else {
        // 普通值
        resolve(x);
    }
}

```

## 链式调用then不能传当前promise

### 1. 场景

```js
let p1 = promise.then(value => {
    console.log(value);
    return p1;
})

```

### 2. 实现步骤分析

- 在then方法当中不能返回当前这个then方法所返回的Promise对象

### 3. 代码实现

```js
class MyPromise {
    ...
    then = (successCallback, failCallback) => {
        const promise2 = new MyPromise((resolve, reject) => {
            // 判断状态
            if (this.status === FULFILLED) {
                setTimeout(() => {
                    const x = successCallback(this.value);
                    // 判断x的值是普通值还是Promise对象
                    // 如果是普通值，直接调用resolve传参
                    // 如果是Promise对象 则查看promise对象返回的结果
                    // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
                    // 由于此处拿不到当前new出来返回的promise2对象，所以需要改为异步调用，包一层setTimeout
                    resolvePromise(promise2, x, resolve, reject);
                }, 0);
            } else if (this.status === REJECTED) {
                ...
            } else {
                ...
            }
        });

        return promise2;
    }
}
function resolvePromise(promise2, x, resolve, reject) {
    if(promise2 === x) {
        return reject(new TypeError("Chaining cycle detected for promise #<Promise>"));
    }
    ...
}

```

## 异常处理

### 1. 场景

> 为了代码的健壮性，要在相应的位置进行适当的错误处理

### 2. 实现步骤分析

- 构造函数调用执行器时
- 成功或失败回调执行错误时调用下一个promise的reject方法把错误信息传递下去

### 3. 代码实现

```js
class MyPromise {
    constructor(executor) {
        try {
            executor(this.resolve, this.reject);
        } catch (e) {
            this.reject(e);
        }
    }
    ...
    resolve = value => {
        ...
        while (this.successCallback.length) this.successCallback.shift()();
    }

    reject = reason => {
        ...
        while (this.failCallback.length) this.failCallback.shift()();
    }
    then = (successCallback, failCallback) => {
        const promise2 = new MyPromise((resolve, reject) => {
            // 判断状态
            if (this.status === FULFILLED) {
                setTimeout(() => {
                    try {
                        const x = successCallback(this.value);
                        // 判断x的值是普通值还是Promise对象
                        // 如果是普通值，直接调用resolve传参
                        // 如果是Promise对象 则查看promise对象返回的结果
                        // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
                        // 由于此处拿不到当前new出来返回的promise2对象，所以改为异步调用，包一层setTimeout
                        resolvePromise(promise2, x, resolve, reject);
                    } catch (e) {
                        // 如果发生错误手动调用下一个promise的reject方法
                        reject(e);
                    }
                }, 0);
            } else if (this.status === REJECTED) {
                setTimeout(() => {
                    try {
                        const x = failCallback(this.reason);;
                        // 判断x的值是普通值还是Promise对象
                        // 如果是普通值，直接调用resolve传参
                        // 如果是Promise对象 则查看promise对象返回的结果
                        // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
                        // 由于此处拿不到当前new出来返回的promise2对象，所以改为异步调用，包一层setTimeout
                        resolvePromise(promise2, x, resolve, reject);
                    } catch (e) {
                        // 如果发生错误手动调用下一个promise的reject方法
                        reject(e);
                    }
                }, 0);
            } else {
                // 等待
                // 将成功和失败回调存储起来
                this.successCallback.push(() => {
                    setTimeout(() => {
                        try {
                            const x = successCallback(this.value);
                            // 判断x的值是普通值还是Promise对象
                            // 如果是普通值，直接调用resolve传参
                            // 如果是Promise对象 则查看promise对象返回的结果
                            // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
                            // 由于此处拿不到当前new出来返回的promise2对象，所以改为异步调用，包一层setTimeout
                            resolvePromise(promise2, x, resolve, reject);
                        } catch (e) {
                            // 如果发生错误手动调用下一个promise的reject方法
                            reject(e);
                        }
                    }, 0);
                });
                this.failCallback.push(() => {
                    setTimeout(() => {
                        try {
                            const x = failCallback(this.reason);;
                            // 判断x的值是普通值还是Promise对象
                            // 如果是普通值，直接调用resolve传参
                            // 如果是Promise对象 则查看promise对象返回的结果
                            // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
                            // 由于此处拿不到当前new出来返回的promise2对象，所以需要改为异步调用，包一层setTimeout
                            resolvePromise(promise2, x, resolve, reject);
                        } catch (e) {
                            // 如果发生错误手动调用下一个promise的reject方法
                            reject(e);
                        }
                    }, 0);
                });
            }
        });

        return promise2;
    }
}

```

## then方法参数变为可选参数

### 1. 场景

```js
const promise = new MyPromise((resolve, reject) => {
    resolve(100);
});
promise.then().then().then(value => console.log(value), reason => console.log(reason));

```

### 2. 实现步骤分析

- then方法的参数都是可选参数
- 如果什么都传入的话就是将值传递下去，相当于传递一个函数**value=>value**
- 所以在then函数中进行相应的参数判断即可

### 3. 代码实现

```js
class MyPromise {
    ...
    then = (successCallback, failCallback) => {
        successCallback = successCallback ? successCallback : value => value;
        failCallback = failCallback ? failCallback : reason => { throw reason; };
        ...
    }
}

```

## 静态方法all

### 1. 场景

```js
function p1 () {
  return new MyPromise(function (resolve, reject) {
    setTimeout(function () {
      resolve('p1')
    }, 2000)
  })
}
function p2 () {
  return new MyPromise(function (resolve, reject) {
    reject('失败')
    // resolve('成功');  
  })
}
MyPromise.all(['a', 'b', p1(), p2(), 'c']).then(result => {
  // result -> ['a', 'b', 'p1', 'p2', 'c']
})

```

### 2. 实现步骤分析

- 按照传入数组的顺序，执行后返回相应顺序的执行结果
- all方法返回的也是一个Promise对象，可以继续链式调用then
- all方法传入的数组中有一个执行的结果是失败的则执行的结果为失败

### 3. 代码实现

```js
static all (array) {
    let result = [];
    let index = 0;
    return new MyPromise((resolve, reject) => {
      function addData (key, value) {
        result[key] = value;
        index++;
        if (index === array.length) {
          resolve(result);
        }
      }
      for (let i = 0; i < array.length; i++) {
        let current = array[i];
        if (current instanceof MyPromise) {
          // promise 对象
          current.then(value => addData(i, value), reason => reject(reason))
        }else {
          // 普通值
          addData(i, array[i]);
        }
      }
    })
}

```

## 静态方法resolve

### 1. 场景

```js
MyPromise.resolve(100).then(value => console.log(value)); // 100

```

### 2. 实现步骤分析

- 将给定的值转化为一个Promise对象，即返回的就是一个Promise对象

### 3. 代码实现

```js
static resolve (value) {
    if (value instanceof MyPromise) return value;
    return new MyPromise(resolve => resolve(value));
}

```

## 原型方法finally

### 1. 场景

 

### 2. 实现步骤分析

- 无论当前的promise对象最终的状态是成功还是失败，finally方法的回调都会执行
- 在finally方法后面可以继续的链式调用then方法拿到当前promise对象最终返回的结果

### 3. 代码实现

```js
finally (callback) {
    return this.then(value => {
        return MyPromise.resolve(callback()).then(() => value);
    }, reason => {
        return MyPromise.resolve(callback()).then(() => { throw reason })
    })
}
```

## 十、原型方法catch

### 1. 场景

```js
function p1 () {
    return new Promise(function (resolve, reject) {
        reject('hello');
    })
}
p1()
    .then(value => console.log(value))
    .catch(reason => console.log(reason))

```

### 2. 实现步骤分析

- 用来处理当前的promise对象最终的状态为失败的情况的
- 当前的then方法是可以不传递失败回调的，可以在后面链式调用catch即可捕获到失败信息

### 3. 代码实现

```js
catch (failCallback) {
    return this.then(undefined, failCallback)
}
```

```js
// 附上完整代码
const PENDING = 'pending'; // 等待
const FULFILLED = 'fulfilled'; // 成功
const REJECTED = 'rejected'; // 失败

class MyPromise {
  constructor (executor) {
    try {
      executor(this.resolve, this.reject)
    } catch (e) {
      this.reject(e);
    }
  }
  // promsie 状态 
  status = PENDING;
  // 成功之后的值
  value = undefined;
  // 失败后的原因
  reason = undefined;
  // 成功回调
  successCallback = [];
  // 失败回调
  failCallback = [];

  resolve = value => {
    // 如果状态不是等待 阻止程序向下执行
    if (this.status !== PENDING) return;
    // 将状态更改为成功
    this.status = FULFILLED;
    // 保存成功之后的值
    this.value = value;
    // 判断成功回调是否存在 如果存在 调用
    // this.successCallback && this.successCallback(this.value);
    while(this.successCallback.length) this.successCallback.shift()()
  }
  reject = reason => {
    // 如果状态不是等待 阻止程序向下执行
    if (this.status !== PENDING) return;
    // 将状态更改为失败
    this.status = REJECTED;
    // 保存失败后的原因
    this.reason = reason;
    // 判断失败回调是否存在 如果存在 调用
    // this.failCallback && this.failCallback(this.reason);
    while(this.failCallback.length) this.failCallback.shift()()
  }
  then (successCallback, failCallback) {
    // 参数可选
    successCallback = successCallback ? successCallback : value => value;
    // 参数可选
    failCallback = failCallback ? failCallback: reason => { throw reason };
    let promsie2 = new MyPromise((resolve, reject) => {
      // 判断状态
      if (this.status === FULFILLED) {
        setTimeout(() => {
          try {
            let x = successCallback(this.value);
            // 判断 x 的值是普通值还是promise对象
            // 如果是普通值 直接调用resolve 
            // 如果是promise对象 查看promsie对象返回的结果 
            // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
            resolvePromise(promsie2, x, resolve, reject)
          }catch (e) {
            reject(e);
          }
        }, 0)
      }else if (this.status === REJECTED) {
        setTimeout(() => {
          try {
            let x = failCallback(this.reason);
            // 判断 x 的值是普通值还是promise对象
            // 如果是普通值 直接调用resolve 
            // 如果是promise对象 查看promsie对象返回的结果 
            // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
            resolvePromise(promsie2, x, resolve, reject)
          }catch (e) {
            reject(e);
          }
        }, 0)
      } else {
        // 等待
        // 将成功回调和失败回调存储起来
        this.successCallback.push(() => {
          setTimeout(() => {
            try {
              let x = successCallback(this.value);
              // 判断 x 的值是普通值还是promise对象
              // 如果是普通值 直接调用resolve 
              // 如果是promise对象 查看promsie对象返回的结果 
              // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
              resolvePromise(promsie2, x, resolve, reject)
            }catch (e) {
              reject(e);
            }
          }, 0)
        });
        this.failCallback.push(() => {
          setTimeout(() => {
            try {
              let x = failCallback(this.reason);
              // 判断 x 的值是普通值还是promise对象
              // 如果是普通值 直接调用resolve 
              // 如果是promise对象 查看promsie对象返回的结果 
              // 再根据promise对象返回的结果 决定调用resolve 还是调用reject
              resolvePromise(promsie2, x, resolve, reject)
            }catch (e) {
              reject(e);
            }
          }, 0)
        });
      }
    });
    return promsie2;
  }
  finally (callback) {
    return this.then(value => {
      return MyPromise.resolve(callback()).then(() => value);
    }, reason => {
      return MyPromise.resolve(callback()).then(() => { throw reason })
    })
  }
  catch (failCallback) {
    return this.then(undefined, failCallback)
  }
  static all (array) {
    let result = [];
    let index = 0;
    return new MyPromise((resolve, reject) => {
      function addData (key, value) {
        result[key] = value;
        index++;
        if (index === array.length) {
          resolve(result);
        }
      }
      for (let i = 0; i < array.length; i++) {
        let current = array[i];
        if (current instanceof MyPromise) {
          // promise 对象
          current.then(value => addData(i, value), reason => reject(reason))
        }else {
          // 普通值
          addData(i, array[i]);
        }
      }
    })
  }
  static resolve (value) {
    if (value instanceof MyPromise) return value;
    return new MyPromise(resolve => resolve(value));
  }
}

function resolvePromise (promsie2, x, resolve, reject) {
  if (promsie2 === x) {
    return reject(new TypeError('Chaining cycle detected for promise #<Promise>'))
  }
  if (x instanceof MyPromise) {
    // promise 对象
    // x.then(value => resolve(value), reason => reject(reason));
    x.then(resolve, reject);
  } else {
    // 普通值
    resolve(x);
  }
}

module.exports = MyPromise;
```