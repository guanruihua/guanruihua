# 函数

## **定义方法**

> 静态方法	function 函数名([参数]){}
>
> 动态匿名	var 函数名 = new Function(["虚参列表"], "函数体");
>
> **直接变量** 函数名 = function([虚参列表]){函数体;}

### 函数声明

```js
// ES5
function [name](){}
function (){} //匿名函数
// ES6
()=>{}// 函数体只有一行{}和return 可以省略
```

### 函数表达式

```js
// ES5
var sum = function(){};
// ES6
let sum = () => {};// 函数体只有一行{}和return 可以省略
```

### 构造函数

```js
const sum = new Function('a', 'b', 'return a + b');
```

### 三种方法对比

> 1. 函数声明有预解析,而且函数声明的优先级高于变量; 
> 2. 使用Function构造函数定义函数的方式是一个函数表达式,这种方式会导致解析两次代码，影响性能。
>    1. 第一次解析常规的JavaScript代码
>    2. 第二次解析传入构造函数的字符串

------

## Rest 参数

> rest : 剩余部分

```js
function sum(...nums) {
    let num = 0
    nums.forEach(function(item) {
        num += item * 1
    })
    return num
}

console.log(sum(1, 2, 3)) // 6
console.log(sum(1, 2, 3, 4)) // 10
```

## name属性

```js
function foo(){}

foo.name // "foo"s
```



## 调用

> - 直接调用 :	函数名(实参列表)
> - 在连接中调用: `<a href = "javascript: 函数名()">tap</a>`
> - 在事件中调用 : 事件类型 = "函数名()"
> - 递归调用

### ES5

> ES5函数内容的this指向和调用方法相关

#### 调用模式

> 函数名()和匿名函数调用, this 指向 window

```js
 function getSum() {
    console.log(this) //这个属于函数名调用，this指向window
 }
 getSum()
 
 (function() {
    console.log(this) //匿名函数调用，this指向window
 })()
 
 var getSum=function() {
    console.log(this) //实际上也是函数名调用，window
 }
 getSum()
```

#### 方法调用

> `对象.方法名()`, this 指向对象

```js
var obj = {
   name: 'methods',
   getSum: function() {
     console.log(this) //objList对象
   }
}
obj.getSum()
```



#### 构造器调用

> new关键词 , this 指向实例化的对象;

```js
function Person() {
  console.log(this); //是构造函数调用，指向实例化的对象personOne
}
var personOne = new Person();
```



#### 间接调用

> 利用call和apply来实现,this就是call和apply对应的第一个参数,如果不传值或者第一个值为null,undefined时this指向window 通过call/apply如果第一个参数是string、number、boolean，call内部会调用其相应的构造器String、Numer、Boolean将其转换为相应的实例对象

```js
function foo() {
   console.log(this);
}
foo.apply('我是apply改变的this值');//我是apply改变的this值
foo.call('我是call改变的this值');//我是call改变的this值
```



### ES6箭头函数

> - ==箭头函数==不可以作为构造函数使用
> - 不可以使用arguments 对象
> - this的指向定义时所在的对象, 而不是调用的对象
> - 不可以通过new实例化对象
> - 调用就是调用模式

```js
(() => {
   console.log(this)//window
})()

let arrowFun = () => {
  console.log(this)//window
}
arrowFun()

let arrowObj = {
  arrFun: function() {
   (() => {
     console.log(this)//this指向的是arrowObj对象
   })()
   }
 }
 arrowObj.arrFun();
```

## 方法

> - IE5之前不支持call和apply,bind是ES5出来的; 
>   
> - call和apply可以调用函数,改变this,实现继承和借用别的对象的方法;
>   
> - apply  : 
>   
>   - 将函数作为对象的方法来调用
>   - 将参数以数组形式传递给改方法(多个参)
>   
> - ##### call : 
>   
>   - 将函数作为对象的方法来调用
>   - 将指定参数传递给该方法(一个参)
>   
> - toString



### call 和 apply

> 1. 间接调用函数, 改变作用域的this的值
> 2. 劫持其他对象方法
> 3. 调用方法,用一个对象替换掉另一个对象(this) 
>    1. `对象.call(新this对象,实参1,实参2,实参3.....)  `
>    2. `对象.apply(新this对象,[实参1,实参2,实参3.....]) `

```js
var foo = {
  name:"张三",
  logName:function(){
    console.log(this.name);
  }
}
var bar={
  name:"李四"
};
foo.logName.call(bar);//李四
// 实质是call改变了foo的this指向为bar,并调用该函数
```

#### 两个函数继承

```js
function Animal(name){   
  this.name = name;   
  this.showName = function(){   
    console.log(this.name);   
  }   
}   
function Cat(name){  
  Animal.call(this, name);  
}    
var cat = new Cat("Black Cat");   
cat.showName(); //Black Cat
```



### 其他运用

```js
let arr1 = [1, 2, 3];
let arr2 = [4, 5, 6];
Array.prototype.push.apply(arr1, arr2); //将arr2合并到了arr1中
console.log('max:', Math.max.apply(null, arr1)); // max: 6
console.log('arr1:', arr1); //arr1:  [1,2,3,4,5,6]
console.log('type:', Object.prototype.toString.call({}));// type: [Object Object]
console.log('type:', Object.prototype.toString.call(arr1)); // type: [Object arrary]
```



### bind

> - 是`function`一个函数拓展方法
> - bind以后的代码重新绑定了func内部的this指向, 返回一个函数, 不会调用方法
> - 不兼容`IE8`

```js
let foo = {
  name: "张三",
  logName: function (age) {
    console.log(this.name, age);
  }
}
let fooNewBind = foo.logName.bind(foo);
fooNewBind(11)//张三,11  因为bind改变了fooNewBind里面的this指向
```

### call apply bind 原理

#### call

```js
// call
Function.prototype.newCall = function (context, ...parameter) {
  if (typeof context === 'object' || typeof context === 'function') {
    context = context || window
  } else {
    context = Object.create(null)
  }
  let fn = Symbol()
  context[fn] = this
  const res = context[fn](...parameter)
  delete context.fn;
  return res
}

let person = {
  name: 'Abiel'
}
function sayHi(age, sex) {
  console.log(this.name, age, sex);
}
sayHi.newCall(person, 25, '男'); // Abiel 25 男
```



#### apply

```js
// apply
Function.prototype.newApply = function (context, parameter) {
  if (typeof context === 'object' || typeof context === 'function') {
    context = context || window
  } else {
    context = Object.create(null)
  }
  let fn = Symbol()
  context[fn] = this
  const res = context[fn](...parameter);
  delete context[fn]
  return res
}
let person = {
  name: "Abiel"
};
function sayHi(age, sex) {
  console.log(this.name, age, sex);
}
sayHi.newApply(person, [25, '男']) //Abiel 25 男
```



#### bind

```js
// bind
Function.prototype.bind = function (context, ...innerArgs) {
  var me = this
  return function (...finnalyArgs) {
    return me.call(context, ...innerArgs, ...finnalyArgs)
  }
}
let person = {
  name: 'Abiel'
}
function sayHi(age, sex) {
  console.log(this.name, age, sex);
}
let personSayHi = sayHi.bind(person, 25)
personSayHi('男')
```



## arguments对象

> - 功能: 存放实参的参数列表
> - 特性: 
>   - 仅能在函数体内使用
>   - 带有下标属性, 当并非数组
>   - 函数声明自动初始化
> - 属性: 
>   - length
>   - callee   当前正指向的函数
>   - caler    抵用当前正在执行函数的函数名

## 指针标识

> - this                 指向当前操作对象
> - callee             指向参数集合所处函数
> - prototype      指向函数附带的原型对象
> - constructor   指向创建该对象的构造函数

