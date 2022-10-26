# ESMAScript

## 变量的解构赋值

ES6允许我们，通过数组或者对象的方式，对一组变量进行赋值，这被称为解构。

解构赋值在实际开发中可以大量减少我们的代码量，并且让程序结构更清晰。

### 数组的解构赋值

**举例：**

通常情况下，我们在为一组变量赋值时，一般是这样写：

```javascript
 let a = 0;
 let b = 1;
 let c = 2;

```

现在我们可以通过数组解构的方式进行赋值：

```javascript
 let [a, b, c] = [1, 2, 3];
```

二者的效果是一样的。

**解构的默认值：**

在解构赋值时，是允许使用默认值的。举例如下：

```javascript
{
    //一个变量时
    let [foo = true] = [];
    console.log(foo); //输出结果：true
}

{
    //两个变量时
    let [a, b] = ['生命壹号']   //a 赋值为：生命壹号。b没有赋值
    console.log(a + ',' + b); //输出结果：生命壹号,undefined
}


{
    //两个变量时
    let [a, b = 'smyhvae'] = ['生命壹号']   //a 赋值为：生命壹号。b 采用默认值 smyhvae
    console.log(a + ',' + b); //输出结果：生命壹号,smyhvae
}

```

`undefined`和`null`的区别：

如果我们在赋值时，采用的是 `undefined`或者`null`，那会有什么区别呢？

```javascript
{
    let [a, b = 'smyhvae'] = ['生命壹号', undefined]; //b 虽然被赋值为 undefined，但是 b 会采用默认值
    console.log(a + ',' + b); //输出结果：生命壹号,smyhvae
}

{
    let [a, b = 'smyhvae'] = ['生命壹号', null];  //b 被赋值为 null
    console.log(a + ',' + b); //输出结果：生命壹号,null
}

```

上方代码分析：

- undefined：相当于什么都没有，此时 b 采用默认值。

- null：相当于有值，但值为 null。

### 对象的解构赋值

通常情况下，我们从接口拿到json数据后，一般这么赋值：

```javascript
var a = json.a;

var b = json.b;

bar c = json.c;
```

上面这样写，过于麻烦了。

现在，我们同样可以针对对象，进行结构赋值。

**举例如下：**

```js
 let { foo, bar } = { bar: '我是 bar 的值', foo: '我是 foo 的值' };
 console.log(foo + ',' + bar); //输出结果：我是键 foo 的值,我是键 bar 的值
```

上方代码可以看出，对象的解构与数组的结构，有一个重要的区别：**数组**的元素是按次序排列的，变量的取值由它的**位置**决定；而**对象的属性没有次序**，是**根据键来取值**的。

**圆括号的使用**：

如果变量 foo 在解构之前就已经定义了，此时你再去解构，就会出现问题。下面是错误的代码，编译会报错：

```javascript
 let foo = 'haha';
 { foo } = { foo: 'smyhvae' };
 console.log(foo);

```

要解决报错，只要在解构的语句外边，加一个圆括号即可：

```javascript
 let foo = 'haha';
 ({ foo } = { foo: 'smyhvae' });
 console.log(foo); //输出结果：smyhvae

```

### 字符串解构

字符串也可以解构，这是因为，此时字符串被转换成了一个类似数组的对象。举例如下：

```javascript
const [a, b, c, d] = 'smyhvae';
console.log(a);
console.log(b);
console.log(c);
console.log(d);

console.log(typeof a);  //输出结果：string
```

输出结果：

![](http://img.smyhvae.com/20180304_1626.png)

## for ... of 循环

ES6 中，如果我们要遍历一个数组，可以这样做：

```
 let arr1 = [1, 2, 3, 4, 5];

 for (let value of arr1) {
     console.log(value);
 }
```

输出结果：

![](http://img.smyhvae.com/20180304_2016.png)

for…of 的循环可以避免我们开拓内存空间，增加代码运行效率，所以建议大家在以后的工作中使用for…of循环。

注意，上面的数组中，`for ... of`获取的是数组里的值；`for ... in`获取的是index索引值。

### Map对象的遍历

`for ... of`既可以遍历数组，也可以遍历Map对象。

## 模板字符串

我们以前让字符串进行拼接的时候，是这样做的：（传统写法的字符串拼接）

```javascript
    var name = 'smyhvae';
    var age = '26';
    console.log('name:'+name+',age:'+age);   //传统写法
```

这种写法，比较繁琐，而且容易出错。

现在有了 ES6 语法，字符串拼接可以这样写：

```javascript
    var name = 'smyhvae';
    var age = '26';

    console.log('name:'+name+',age:'+age);   //传统写法

    console.log(`name:${name},age:${age}`);  //ES6 写法

```

**注意**，上方代码中，倒数第二行用的符号是单引号，最后一行用的符号是反引号（在tab键的上方）。

## 函数拓展

ES6在**函数扩展**方面，新增了很多特性。例如：

- 箭头函数

- 参数默认值

- 参数结构赋值

- 扩展运算符

- rest参数

- this绑定

- 尾调用

### 箭头函数

定义和调用函数：（传统写法）

```javascript
function fn1(a, b) {
    return a + b;
}

console.log(fn1(1, 2));  //输出结果：3
```

定义和调用函数：（ES6中的写法）

```javascript
 var fn2 = (a, b) => a + b;
 console.log(fn2(1, 2));  //输出结果：3
```

二者的效果是一样的。

在箭头函数中，如果方法体内有两句话，那就需要在方法体外边加上{}括号。如下：

```javascript
 var fn2 = (a, b) => {
     console.log('haha');
     return a + b;
 };
 console.log(fn2(1, 2));  //输出结果：3

```

从上面的箭头函数中，我们可以很清晰地找到函数名、参数名、方法体。

上方代码中：

- 如果有且仅有1个参数，则`()`可以省略

- 如果方法体内有且仅有1条语句，则`{}`可以省略，但前提是，这条语句必须是 return。

### this的指向

> 箭头函数只是为了让函数写起来更优雅吗？当然不是，还有一个很大的作用是与this的指向有关。

ES5 中，this指向的是函数被调用的对象；而 ES6 的箭头函数中，this指向的是函数被定义时。

简单来说，箭头函数中的this，是不会变的，是永远绑定在当前的环境下。

### 参数默认值

**传统写法**：

```javascript
    function fn(param) {
        let p = param || 'hello';
        console.log(p);
    }
```

上方代码中，函数体内的写法是：如果 param 不存在，就用 `hello`字符串做兜底。这样写比较啰嗦。

**ES6 写法**：（参数默认值的写法，很简洁）

```javascript
    function fn(param = 'hello') {
        console.log(param);
    }
```

在 ES6 中定义方法时，我们可以给方法里的参数加一个**默认值**（缺省值）：

- 方法被调用时，如果没有给参数赋值，那就是用默认值；

- 方法被调用时，如果给参数赋值了新的值，那就用新的值。

如下：

```javascript
 var fn2 = (a, b = 5) => {
     console.log('haha');
     return a + b;
 };
 console.log(fn2(1));     //第二个参数使用默认值 5。输出结果：6

 console.log(fn2(1, 8));  //输出结果：9

```

**提醒1**：默认值的后面，不能再有**没有默认值的变量**。比如`(a,b,c)`这三个参数，如果我给b设置了默认值，那么就一定要给c设置默认值。

**提醒2**：

我们来看下面这段代码：

```javascript
    let x = 'smyh';
    function fn(x, y = x) {
        console.log(x, y);
    }
    fn('vae');
```

注意第二行代码，我们给y赋值为`x`，这里的`x`是括号里的第一个参数，并不是第一行代码里定义的`x`。打印结果：`vae vae`。

如果我把第一个参数改一下，改成：

```javascript
    let x = "smyh";
    function fn(z, y = x) {
        console.log(z, y);
    }
    fn("vae");
```

此时打印结果是：`vae smyh`。

### 扩展运算符

注意区分：

- 扩展运算符的格式为`...`

- rest运算符的格式为`...变量名`

有了ES6，当我们在定义一个方法，但是不确定其参数的个数时，我们就可以用**扩展运算符**作为参数。

以前，我们在定义方法时，参数要确定个数，如下：（程序会报错）

```javascript
    function fn(a, b, c) {
        console.log(a);
        console.log(b);
        console.log(c);
        console.log(d);
    }

    fn(1, 2, 3);
```

上方代码中，因为方法的参数是三个，但使用时是用到了四个参数，所以会报错：

![](http://img.smyhvae.com/20180304_1638.png)

现在，我们有了扩展运算符，就不用担心报错的问题了。代码可以这样写：

```javascript
function fn(...arg) {   //当不确定方法的参数时，可以使用扩展运算符
    console.log(arg[0]);
    console.log(arg[1]);
    console.log(arg[2]);
    console.log(arg[3]);
}

fn(1, 2, 3); //方法中定义了四个参数，但只引用了三个参数，ES6 中并不会报错。

```

![](http://img.smyhvae.com/20180304_1650.png)

上方代码中注意，arg参数之后，不能再加别的参数，否则编译报错。

**举例：**数组赋值的问题

我们来分析一段代码：（将数组 arr1 赋值给 arr2）

```javascript
 let arr1 = ['www', 'smyhvae', 'com'];
 let arr2 = arr1;          // 将 arr1 赋值给 arr2，其实是让 arr2 指向 arr1 的内存地址
 console.log('arr1:' + arr1);
 console.log('arr2:' + arr2);
 console.log('---------------------');

 arr2.push('你懂得');  //往arr2 里添加一部分内容
 console.log('arr1:' + arr1);
 console.log('arr2:' + arr2);
```

运行结果：

![](http://img.smyhvae.com/20180304_1950.png)

上方代码中，我们往往 arr2 里添加了`你懂的`，却发现，arr1 里也有这个内容。原因是：`let arr2 = arr1;`其实是让 arr2 指向 arr1 的地址。也就是说，二者指向的是同一个内存地址。

如果不想让 arr1 和 arr2 指向同一个内存地址，我们可以借助扩展运算符来做：

```javascript
 let arr1 = ['www', 'smyhvae', 'com'];
 let arr2 = [...arr1];  //arr2 会重新开辟内存地址
 console.log('arr1:' + arr1);
 console.log('arr2:' + arr2);
 console.log('---------------------');

 arr2.push('你懂得');  //往arr2 里添加一部分内容
 console.log('arr1:' + arr1);
 console.log('arr2:' + arr2);
```

运行结果：

![](http://img.smyhvae.com/20180304_1951.png)

我们明白了这个例子，就可以避免开发中的很多业务逻辑上的 bug。

### `rest` 运算符

`rest` 在英文中指的是**剩余部分**（不是指休息）。我们来举个例子，理解剩余部分的含义：

```javascript
 function fn(first, second, ...arg) {
     console.log(arg.length);
 }

 fn(0, 1, 2, 3, 4, 5, 6);  //调用函数后，输出结果为 5
```

上方代码的输出结果为 5。 调用`fn()`时，里面有七个参数，而`arg`指的是剩下的部分（因为除去了`first`和`second`）。

从上方例子中可以看出，`rest`运算符适用于：知道前面的一部分参数的数量，但对于后面剩余的参数数量未知的情况。

## promise,async,Symbol

### Promise

#### 概述

Promise对象: 代表了未来某个将要发生的事件(通常是一个异步操作)。

ES6中的promise对象, 可以**将异步操作以同步的流程表达出来，**很好地解决了**回调地狱**的问题（避免了层层嵌套的回调函数）。在使用ES5的时候，在多层嵌套回调时，写完的代码层次过多，很难进行维护和二次开发。

### 回调地狱的举例

假设买菜、做饭、洗碗都是异步的。

现在的流程是：买菜成功之后，才能开始做饭。做饭成功后，才能开始洗碗。这里面就涉及到了回调的嵌套。

ES6的Promise是一个构造函数, 用来生成promise实例。

### promise对象的3个状态

- 初始化状态（等待状态）：pending

- 成功状态：fullfilled

- 失败状态：rejected

### promise 方法

  Promise.prototype.then()

  Promise.prototype.catch(): 捕获错误

  Promise.all(): 循环处理数组, 对象, 集合等

  Promise.race(): 输出最先出的结果, 不用等到所有操作都得出结果

  Promise.resolve()

  Promise.reject()

### 使用promise的基本步骤

（1）创建promise对象

（2）调用promise的**回调函数**then()

代码格式如下：

```javascript
let promise = new Promise((resolve, reject) => {
  //进来之后，状态为pending
  console.log('111');  //这一行代码是同步的
  //开始执行异步操作（这里开始，写异步的代码，比如ajax请求 or 开启定时器）
  if (异步的ajax请求成功) {
    console.log('333');
    resolve();//如果请求成功了，请写resolve()，此时，promise的状态会被自动修改为fullfilled
  } else {
    reject();//如果请求失败了，请写reject()，此时，promise的状态会被自动修改为rejected
  }
})
console.log('222');

//调用promise的then()
promise
  .then(() => {
    //如果promise的状态为fullfilled，则执行这里的代码
    console.log('成功了');
  }, () => {
    //如果promise的状态为rejected，则执行这里的代码
    console.log('失败了');
  })
```

代码解释：

（1）当new Promise()执行之后，promise对象的状态会被初始化为`pending`，这个状态是初始化状态。`new Promise()`这行代码，括号里的内容是同步执行的。括号里定义一个function，function有两个参数：resolve和reject。如下：

- 如果请求成功了，请写resolve()，此时，promise的状态会被自动修改为fullfilled。

- 如果请求失败了，请写reject()，此时，promise的状态会被自动修改为rejected

（2）promise.then()方法，括号里面有两个参数，分别代表两个函数 function1 和 function2：

- 如果promise的状态为fullfilled（意思是：如果请求成功），则执行function1里的内容

- 如果promise的状态为rejected（意思是，如果请求失败），则执行function2里的内容

另外，resolve()和reject()这两个方法，是可以给promise.then()传递参数的。如下：

```javascript
    let promise = new Promise((resolve, reject) => {
        //进来之后，状态为pending
        console.log('111');  //这行代码是同步的
        //开始执行异步操作（这里开始，写异步的代码，比如ajax请求 or 开启定时器）
        if (异步的ajax请求成功) {
            console.log('333');
            resolve('haha');//如果请求成功了，请写resolve()，此时，promise的状态会被自动修改为fullfilled
        } else {
            reject('555');//如果请求失败了，请写reject()，此时，promise的状态会被自动修改为rejected
        }
    })
    console.log('222');

    //调用promise的then()
    promise.then((successMsg) => {
            //如果promise的状态为fullfilled，则执行这里的代码
            console.log(successMsg, '成功了');
        }
        , (errorMsg) => {
            //如果promise的状态为rejected，则执行这里的代码
            console.log(errorMsg, '失败了');

        }
    )
```

P

### ajax请求的举例（涉及到嵌套的回调）

```javascript
    //定义一个请求news的方法
    function getNews(url) {
        //创建一个promise对象
        let promise = new Promise((resolve, reject) => {
            //初始化promise状态为pending
            //启动异步任务
            let request = new XMLHttpRequest();
            request.onreadystatechange = function () {
                if (request.readyState === 4) {
                    if (request.status === 200) {
                        let news = request.response;
                        resolve(news);
                    } else {
                        reject('请求失败了。。。');
                    }
                }
            };
            request.responseType = 'json';//设置返回的数据类型
            request.open("GET", url);//规定请求的方法，创建链接
            request.send();//发送
        })
        return promise;
    }

    getNews('http://localhost:3000/news?id=2')
        .then((news) => {
            console.log(news);
            document.write(JSON.stringify(news));
            console.log('http://localhost:3000' + news.commentsUrl);
            return getNews('http://localhost:3000' + news.commentsUrl);
        }, (error) => {
            alert(error);
        })
        .then((comments) => {
            console.log(comments);
            document.write('<br><br><br><br><br>' + JSON.stringify(comments));
        }, (error) => {
            alert(error);
        })

```

### Symbol

#### 概述

背景：ES5中对象的属性名都是字符串，容易造成重名，污染环境。

**概念**：ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

**特点：**

- Symbol属性对应的值是唯一的，解决**命名冲突问题**

- Symbol值不能与其他数据进行计算，包括同字符串拼串

- for in、for of 遍历时不会遍历Symbol属性。

#### 创建Symbol属性值

Symbol是函数，但并不是构造函数。创建一个Symbol数据类型：

```javascript
    let mySymbol = Symbol();

    console.log(typeof mySymbol);  //打印结果：symbol
    console.log(mySymbol);         //打印结果：Symbol()
```

打印结果：

![](http://img.smyhvae.com/20180317_1134.png)

下面来讲一下Symbol的使用。

#### 将Symbol作为对象的属性值

```javascript
    let mySymbol = Symbol();

    let obj = {
        name: 'smyhvae',
        age: 26
    };

    //obj.mySymbol = 'male'; //错误：不能用 . 这个符号给对象添加 Symbol 属性。
    obj[mySymbol] = 'hello';    //正确：通过**属性选择器**给对象添加 Symbol 属性。后面的属性值随便写。

    console.log(obj);
```

上面的代码中，我们尝试给obj添加一个Symbol类型的属性值，但是添加的时候，不能采用`.`这个符号，而是应该用`属性选择器`的方式。打印结果：

![](http://img.smyhvae.com/20180317_1134.png)

现在我们用for in尝试对上面的obj进行遍历：

```javascript
    let mySymbol = Symbol();

    let obj = {
        name: 'smyhvae',
        age: 26
    };

    obj[mySymbol] = 'hello';

    console.log(obj);

    //遍历obj
    for (let i in obj) {
        console.log(i);
    }
```

打印结果：

![](http://img.smyhvae.com/20180317_1134.png)

从打印结果中可以看到：for in、for of 遍历时不会遍历Symbol属性。

#### 创建Symbol属性值时，传参作为标识

如果我通过 Symbol()函数创建了两个值，这两个值是不一样的：

```javascript
    let mySymbol1 = Symbol();
    let mySymbol2 = Symbol();

    console.log(mySymbol1 == mySymbol2); //打印结果：false
    console.log(mySymbol1);         //打印结果：Symbol()
    console.log(mySymbol2);         //打印结果：Symbol()
```

![](http://img.smyhvae.com/20180317_1134.png)

上面代码中，倒数第三行的打印结果也就表明了，二者的值确实是不相等的。

最后两行的打印结果却发现，二者的打印输出，肉眼看到的却相同。那该怎么区分它们呢？

既然Symbol()是函数，函数就可以传入参数，我们可以通过参数的不同来作为**标识**。比如：

```javascript
    //在括号里加入参数，来标识不同的Symbol
    let mySymbol1 = Symbol('one');
    let mySymbol2 = Symbol('two');

    console.log(mySymbol1 == mySymbol2); //打印结果：false
    console.log(mySymbol1);         //打印结果：Symbol(one)
    console.log(mySymbol2);         //打印结果：Symbol(two)。颜色为红色。
    console.log(mySymbol2.toString());//打印结果：Symbol(two)。颜色为黑色。
```

打印结果：

![](http://img.smyhvae.com/20180317_1134.png)

#### 定义常量

Symbol 可以用来定义常量：

```javascript
    const MY_NAME = Symbol('my_name');
```

### async函数（异步函数）

概念：真正意义上去解决异步回调的问题，同步流程表达异步操作。

本质： Generator 的语法糖。

async比之前的 Promise、Generator要好用一些。

语法：

```javascript
    async function foo() {
        await 异步操作;
        await 异步操作；
    }
```

我们在普通的函数前面加上 async 关键字，就成了 async 函数。

### async、Promise、Generator的对比（async的特点）

1、不需要像Generator去调用next方法，遇到await等待，当前的异步操作完成就往下执行。

2、async返回的总是Promise对象，可以用then方法进行下一步操作。

3、async取代Generator函数的星号*，await取代Generator的yield。

4、语意上更为明确，使用简单，经临床验证，暂时没有任何副作用。

## 字符串的扩展

ES6中的字符串扩展，用得少，而且逻辑相对简单。如下：

- `includes(str)`：判断是否包含指定的字符串

- `startsWith(str)`：判断是否以指定字符串开头

- `endsWith(str)`：判断是否以指定字符串结尾

- `repeat(count)`：重复指定次数

举例如下：

```javascript
    let str = 'abcdefg';

    console.log(str.includes('a'));//true
    console.log(str.includes('h'));//false

    //startsWith(str) : 判断是否以指定字符串开头
    console.log(str.startsWith('a'));//true
    console.log(str.startsWith('d'));//false

    //endsWith(str) : 判断是否以指定字符串结尾
    console.log(str.endsWith('g'));//true
    console.log(str.endsWith('d'));//false

    //repeat(count) : 重复指定次数a
    console.log(str.repeat(5));
```

打印结果：

![](http://img.smyhvae.com/20180402_1050.png)

## Number 的扩展

- 二进制与八进制数值表示法: 二进制用`0b`, 八进制用`0o`。

举例：

```javascript
    console.log(0b1010);//10
    console.log(0o56);//46
```

- `Number.isFinite(i)`：判断是否为有限大的数。比如`Infinity`这种无穷大的数，返回的就是false。

- `Number.isNaN(i)`：判断是否为NaN。

- `Number.isInteger(i)`：判断是否为整数。

- `Number.parseInt(str)`：将字符串转换为对应的数值。

- `Math.trunc(i)`：去除小数部分。

举例：

```javascript
    //Number.isFinite(i) : 判断是否是有限大的数
    console.log(Number.isFinite(NaN)); //false
    console.log(Number.isFinite(5)); //true
    console.log(Number.isFinite(Infinity)); //false

    //Number.isNaN(i) : 判断是否是NaN
    console.log(Number.isNaN(NaN));//true
    console.log(Number.isNaN(5));//falsse

    //Number.isInteger(i) : 判断是否是整数
    console.log(Number.isInteger(5.23));//false
    console.log(Number.isInteger(5.0));//true
    console.log(Number.isInteger(5));//true

    //Number.parseInt(str) : 将字符串转换为对应的数值
    console.log(Number.parseInt('123abc'));//123
    console.log(Number.parseInt('a123abc'));//NaN

    // Math.trunc(i) : 直接去除小数部分
    console.log(Math.trunc(13.123));//13
```

## 数组的扩展

> 下面提到的数组的几个方法，更详细的内容，可以看《04-JavaScript基础/17-数组的常见方法.md》。

### 扩展1：Array.from()

```javascript
 Array.from(伪数组/可遍历的对象)
```

**作用**：将**伪数组**或可遍历对象转换为**真数组**。

### 扩展2：Array.of()

```javascript
 Array.of(value1, value2, value3)
```

**作用**：将一系列值转换成数组。

### 扩展3：find() 和 findIndex()

**方法1**：

```javascript
 find(function(item, index, arr){return true})
```

**作用**：找出**第一个**满足「指定条件返回true」的元素。

**方法2**：

```javascript
 findIndex(function(item, index, arr){return true})
```

**作用**：找出第一个满足「指定条件返回true」的元素的index。

## 对象的扩展

### 扩展1

```javascript
 Object.is(v1, v2)
```

**作用：**判断两个数据是否完全相等。底层是通过**字符串**来判断的。

我们先来看下面这两行代码的打印结果：

```javascript
        console.log(0 == -0);
        console.log(NaN == NaN);
```

打印结果：

```
 true
 false
```

上方代码中，第一行代码的打印结果为true，这个很好理解。第二行代码的打印结果为false，因为NaN和任何值都不相等。

但是，如果换成下面这种方式来比较：

```javascript
        console.log(Object.is(0, -0));
        console.log(Object.is(NaN, NaN));
```

打印结果却是：

```
 false
 true
```

代码解释：还是刚刚说的那样，`Object.is(v1, v2)`比较的是字符串是否相等。

### 扩展2（重要）

```javascript
 Object.assign(目标对象, 源对象1, 源对象2...)
```

**作用：** 将源对象的属性追加到目标对象上。如果对象里属性名相同，会被覆盖。

其实可以理解成：将多个对象**合并**为一个新的对象。

举例：

```javascript
        let obj1 = { name: 'smyhvae', age: 26 };
        let obj2 = { city: 'shenzhen' };
        let obj3 = {};

        Object.assign(obj3, obj1, obj2);
        console.log(obj3);
```

打印结果：

![](http://img.smyhvae.com/20180404_2240.png)

上图显示，成功将obj1和obj2的属性复制给了obj3。

### 扩展3：`__proto__`属性

举例：

```javascript
       let obj1 = {name:'smyhvae'};
       let obj2 = {};

       obj2.__proto__ = obj1;

       console.log(obj1);
       console.log(obj2);
       console.log(obj2.name);
```

打印结果：

![](http://img.smyhvae.com/20180404_2251.png)

上方代码中，obj2本身是没有属性的，但是通过`__proto__`属性和obj1产生关联，于是就可以获得obj1里的属性。

## 迭代器（Iterator）

#### ES5实现迭代器

迭代器是什么？遇到这种新的概念，莫慌张。

**迭代器是一种特殊对象，每一个迭代器对象都有一个next()，该方法返回一个对象，包括value和done属性。**

**ES5实现迭代器的代码如下：**

```js
    //实现一个返回迭代器对象的函数，注意该函数不是迭代器，返回结果才叫做迭代器。
    function createIterator(items) {
      var i = 0;
      return {
        next() {
          var done = (i >= items.length); // 判断i是否小于遍历的对象长度。
          var value = !done ? items[i++] : undefined; //如果done为false，设置value为当前遍历的值。
          return {
            done,
            value
          }
        }
      }
    }
    const a = createIterator([1, 2, 3]);
    
    //该方法返回的最终是一个对象，包含value、done属性。
    console.log(a.next()); //{value: 1, done: false}
    console.log(a.next()); //{value: 2, done: false}
    console.log(a.next()); //{value: 3, done: false}
    console.log(a.next()); //{value: undefined, done: true}
```

### 生成器（Generator）

**生成器是函数：用来返回迭代器。**

这个概念有2个关键点，一个是函数、一个是返回迭代器。这个函数不是上面ES5中创建迭代器的函数，而是ES6中特有的，一个带有*（星号）的函数，同时你也需要使用到yield。

```js
//生成器函数，ES6内部实现了迭代器功能，你要做的只是使用yield来迭代输出。
function *createIterator() {
  yield 1;
  yield 2;
  yield 3;
}
const a = createIterator();
console.log(a.next()); //{value: 1, done: false}
console.log(a.next()); //{value: 2, done: false}
console.log(a.next()); //{value: 3, done: false}
console.log(a.next()); //{value: undefined, done: true}
```

生成器的yield关键字有个神奇的功能，就是当你执行一次next()，那么只会执行一个yield后面的内容，然后语句终止运行。

#### 在for循环中使用迭代器

即使你是在for循环中使用yield关键字，也会暂停循环。

```js
    function *createIterator(items) {
      for(let i = 0; i < items.length;  i++) {
        yield items[i]
      }
    }
    const a = createIterator([1, 2, 3]);
    console.log(a.next()); //{value: 1, done: false}
```

#### yield使用限制

yield只可以在生成器函数内部使用，如果在非生成器函数内部使用，则会报错。

```js
function *createIterator(items) {
  //你应该在这里使用yield
  items.map((value, key) => {
    yield value //语法错误，在map的回调函数里面使用了yield
  })
}
const a = createIterator([1, 2, 3]);
console.log(a.next()); //无输出
```

#### 生成器函数表达式

函数表达式很简单，就是下面这种写法，也叫匿名函数，不用纠结。

```js
const createIterator = function *() {
  yield 1;
  yield 2;
}
const a = createIterator();
console.log(a.next());
```

#### 在对象中添加生成器函数

一个对象长这样：

```
const obj = {}
```

我们可以在obj中添加一个生成器，也就是添加一个带星号的方法：

```js
const obj = {
  a: 1,
  *createIterator() {
    yield this.a
  }
}
const a = obj.createIterator();
console.log(a.next());  //{value: 1, done: false}
```

#### 可迭代对象和for of循环

再次默读一遍，迭代器是对象，生成器是返回迭代器的函数。

凡是通过生成器生成的迭代器，都是可以迭代的对象(可迭代对象具有Symbol.iterator属性)，也就是可以通过for of将value遍历出来。

```js
function *createIterator() {
  yield 1;
  yield 2;
  yield 3;
}
const a = createIterator();
for(let value of a) {
  console.log(value)
}
// 1 2 3
```

上面的例子告诉我们生成器函数返回的迭代器是一个可以迭代的对象。其实我们这里要研究的是Symbol.iterator的用法。

```js
function *createIterator() {
  yield 1;
  yield 2;
  yield 3;
}
const a = createIterator(); //a是一个迭代器
const s = a[Symbol.iterator]();//使用Symbol.iterator访问迭代器
console.log(s.next()) //{value: 1, done: false}
```

Symbol.iterator还可以用来检测一个对象是否可迭代：

```
typeof obj[Symbol.iterator] === "function"
```

#### 创建可迭代对象

**在ES6中，数组、Set、Map、字符串都是可迭代对象。**

**默认情况下定义的对象（object）是不可迭代的，但是可以通过Symbol.iterator创建迭代器。**

```js
    const obj = {
      items: []
    }
    obj.items.push(1);//这样子虽然向数组添加了新元素，但是obj不可迭代
    for (let x of obj) {
      console.log(x) // _iterator[Symbol.iterator] is not a function
    }

    //接下来给obj添加一个生成器，使obj成为一个可以迭代的对象。
    const obj = {
      items: [],
      *[Symbol.iterator]() {
        for (let item of this.items) {
          yield item;
        }
      }
    }
    obj.items.push(1)
    //现在可以通过for of迭代obj了。
    for (let x of obj) {
      console.log(x)
    }
```

#### 内建迭代器

上面提到了，数组、Set、Map都是可迭代对象，即它们内部实现了迭代器，并且提供了3种迭代器函数调用。

**1、entries() 返回迭代器**：返回键值对

```js
    //数组
    const arr = ['a', 'b', 'c'];
    for(let v of arr.entries()) {
      console.log(v)
    }
    // [0, 'a'] [1, 'b'] [2, 'c']
    
    //Set
    const arr = new Set(['a', 'b', 'c']);
    for(let v of arr.entries()) {
      console.log(v)
    }
    // ['a', 'a'] ['b', 'b'] ['c', 'c']

    //Map
    const arr = new Map();
    arr.set('a', 'a');
    arr.set('b', 'b');
    for(let v of arr.entries()) {
      console.log(v)
    }
    // ['a', 'a'] ['b', 'b']
```

**2、values() 返回迭代器**：返回键值对的value

```js
    //数组
    const arr = ['a', 'b', 'c'];
    for(let v of arr.values()) {
      console.log(v)
    }
    //'a' 'b' 'c'

    //Set
    const arr = new Set(['a', 'b', 'c']);
    for(let v of arr.values()) {
      console.log(v)
    }
    // 'a' 'b' 'c'

    //Map
    const arr = new Map();
    arr.set('a', 'a');
    arr.set('b', 'b');
    for(let v of arr.values()) {
      console.log(v)
    }
    // 'a' 'b'
```

**3、keys() 返回迭代器**：返回键值对的key

```js
    //数组
    const arr = ['a', 'b', 'c'];
    for(let v of arr.keys()) {
      console.log(v)
    }
    // 0 1 2
    
    //Set
    const arr = new Set(['a', 'b', 'c']);
    for(let v of arr.keys()) {
      console.log(v)
    }
    // 'a' 'b' 'c'

    //Map
    const arr = new Map();
    arr.set('a', 'a');
    arr.set('b', 'b');
    for(let v of arr.keys()) {
      console.log(v)
    }
    // 'a' 'b'
```

虽然上面列举了3种内建的迭代器方法，但是不同集合的类型还有自己默认的迭代器，在for of中，数组和Set的默认迭代器是values()，Map的默认迭代器是entries()。

#### for of循环解构

对象本身不支持迭代，但是我们可以自己添加一个生成器，返回一个key，value的迭代器，然后使用for of循环解构key和value。

```js
    const obj = {
      a: 1,
      b: 2,
      *[Symbol.iterator]() {
        for(let i in obj) {
          yield [i, obj[i]]
        }
      }
    }
    for(let [key, value] of obj) {
      console.log(key, value)
    }
    // 'a' 1, 'b' 2
```

#### 字符串迭代器

```
    const str = 'abc';
    for(let v of str) {
      console.log(v)
    }
    // 'a' 'b' 'c'
```

#### NodeList迭代器

迭代器真是无处不在啊，dom节点的迭代器你应该已经用过了。

```js
const divs = document.getElementByTagName('div');
for(let d of divs) {
  console.log(d)
}
```

#### 展开运算符和迭代器

```js
    const a = [1, 2, 3];
    const b = [4, 5, 6];
    const c = [...a, ...b]
    console.log(c) // [1, 2, 3, 4, 5, 6]
```

#### 高级迭代器功能

你说什么？上面讲了一堆废话都是基础功能？还有高级功能没讲？

高级功能不复杂，就是传参、抛出异常、生成器返回语句、委托生成器。

1、传参

生成器里面有2个yield，当执行第一个next()的时候，返回value为1，然后给第二个next()传入参数10，传递的参数会替代掉上一个next()的yield返回值。在下面的例子中就是first。

```js
function *createIterator() {
  let first = yield 1;
  yield first + 2;
}
let i = createIterator();
console.log(i.next()); // {value: 1, done: false}
console.log(i.next(10)); // {value: 12, done: false}
```

2、在迭代器中抛出错误

```js
function *createIterator() {
  let first = yield 1;
  yield first + 2;
}
let i = createIterator();
console.log(i.next()); // {value: 1, done: false}
console.log(i.throw(new Error('error'))); // error
console.log(i.next()); //不再执行
```

3、生成器返回语句

生成器中添加return表示退出操作。

```js
function *createIterator() {
  let first = yield 1;
  return;
  yield first + 2;
}
let i = createIterator();
console.log(i.next()); // {value: 1, done: false}
console.log(i.next()); // {value: undefined, done: true}
```

4、委托生成器

生成器嵌套生成器

```js
function *aIterator() {
yield 1;
}
function *bIterator() {
yield 2;
}
function *cIterator() {
yield *aIterator()
yield *bIterator()
}

let i = cIterator();
console.log(i.next()); // {value: 1, done: false}
console.log(i.next()); // {value: 2, done: false}
```

### 异步任务执行器

ES6之前，我们使用异步的操作方式是调用函数并执行回调函数。

书上举的例子挺好的，在nodejs中，有一个读取文件的操作，使用的就是回调函数的方式。

```js
var fs = require("fs");
fs.readFile("xx.json", function(err, contents) {
//在回调函数中做一些事情
})
```

那么任务执行器是什么呢？

**任务执行器是一个函数，用来循环执行生成器，因为我们知道生成器需要执行N次next()方法，才能运行完，所以我们需要一个自动任务执行器帮我们做这些事情，这就是任务执行器的作用。**

下面我们编写一个异步任务执行器。

```js
//taskDef是一个生成器函数，run是异步任务执行器
function run(taskDef) {
  let task = taskDef(); //调用生成器
  let result = task.next(); //执行生成器的第一个next()，返回result
  function step() {
    if(!result.done) {
      //如果done为false，则继续执行next()，并且循环step，直到done为true退出。
      result = task.next(result.value);
      step();
    }
  }
  step(); //开始执行step()
}
```

测试一下我们编写的run方法，我们不再需要console.log N个next了，因为run执行器已经帮我们做了循环执行操作：

```js
    run(function *() {
      let value = yield 1;
      value = yield value + 20;
      console.log(value) // 21
    })
```

## 代理 Proxy

![clipboard.png](https://segmentfault.com/img/bVR5W5?w=710&h=182)

#### 语法

```
    let p = new Proxy(target, handler);
```

target：一个目标对象(可以是任何类型的对象，包括本机数组，函数，甚至另一个代理)用Proxy来包装。 handler：一个对象，其属性是当执行一个操作时定义代理的行为的函数。

#### 代理的使用

**基础demo：**Proxy的demo有很多，我们只分析基础demo，主要看new Proxy({}, handler)的操作，指定目标obj对象，然后handler对象执行get()操作，get()返回值的判断是，如果name是target目标对象的属性，则返回target[name]的值，否则返回37，最后测试的时候，p.a是对象p的key，所以返回a的value，而p.b不存在，返回37。

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

这个例子的作用是拦截目标对象obj，当执行obj的读写操作时，进入handler函数进行判断，如果读取的key不存在，则返回默认值。

## 模块的定义

模块是自动运行在严格模式下并且没有办法退出运行的JavaScript代码。

模块可以是函数、数据、类，需要指定导出的模块名，才能被其他模块访问。

```js
//数据模块
const obj = {a: 1}
//函数模块
const sum = (a, b) => {
  return a + b
}
//类模块
class My extends React.Components {

}
```

### 模块的导出

给数据、函数、类添加一个export，就能导出模块。一个配置型的JavaScript文件中，你可能会封装多种函数，然后给每个函数加上一个export关键字，就能在其他文件访问到。

```js
//数据模块
export const obj = {a: 1}
//函数模块
export const sum = (a, b) => {
  return a + b
}
//类模块
export class My extends React.Components {

}
```

```json
// module.js
let variable = []

// 选择导出的变量
module.exports = {
  variable: variable
  // 可以写多个
}

//使用
let module = require('./module')
console.log(module.varibale) 
```

### 模块的引用

在另外的js文件中，我们可以引用上面定义的模块。使用import关键字，导入分2种情况，一种是导入指定的模块，另外一种是导入全部模块。

1、导入指定的模块。

```js
//导入obj数据，My类
import {obj, My} from './xx.js'

//使用
console.log(obj, My)
```

2、导入全部模块

```
    //导入全部模块
    import * as all from './xx.js'
    
    //使用
    console.log(all.obj, all.sun(1, 2), all.My)
```

### 默认模块的使用

如果给我们的模块加上default关键字，那么该js文件默认只导出该模块，你还需要把大括号去掉。

```js
//默认模块的定义
function sum(a, b) {
  return a + b
}
export default sum

//导入默认模块
import sum from './xx.js'
```

### 模块的使用限制

不能在语句和函数之内使用export关键字，只能在模块顶部使用，作为react和vue开发者的你，这个限制你应该很熟悉了。

**在react中，模块顶部导入其他模块。**

```js
import react from 'react'
```

**在vue中，模块顶部导入其他模块。**

```js
<script>
import sum from './xx.js'
</script>
```

### 修改模块导入和导出名

有2种修改方式，一种是模块导出时修改，一种是导入模块时修改。

1、导出时修改：

```js
    function sum(a, b) {
        return a + b
    }
    export {sum as add}

    import { add } from './xx.js'
    add(1, 2)
```

2、导入时修改：

```
    function sum(a, b) {
        return a + b
    }
    export sum

    import { sum as add } from './xx.js'
    add(1, 2)
```

### 无绑定导入

当你的模块没有可导出模块，全都是定义的全局变量的时候，你可以使用无绑定导入。

模块：

```js
let a = 1
const PI = 3.1314
```

无绑定导入：

```js
import './xx.js'
console.log(a, PI)
```

### 浏览器加载模块

有用过webpack打包js模块的同学可能有经验，使用webpack打包了多个js文件，然后放到HTML使用script加载时，如果加载顺序不对，就会出现找不到模块的错误。

这是因为模块之间是有依赖关系的，就像你使用jQuery的时候，必须先加载jQuery的代码，才能使用jQuery提供的方法。

**加载模块的方法，总是先加载模块1，再加载模块2，因为module类型默认使用defer属性。**

```html
<script type="module" src="module1.js"></script>
<script type="module" src="module2.js"></script>
```

### 总结

模块还有很多有意思的特性，对react和vue开发有一定经验的人对这些基本知识应该了如指掌，新手不了解也不用太心急，写几个module.js做一下尝试。如果浏览器报错，不能识别export模块，你可能需要先加载babel的js插件来编译它。
