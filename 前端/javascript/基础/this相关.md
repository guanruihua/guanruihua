---
title: this相关
date: 
tags: 
- question 
- front-end
---

# Question

## 1.this的优先级

> this的优先级:  new>bind>call(apply)>obj.func()>默认绑定

```js
var obj = {};
obj.log= console.log;
obj.log.call(console,this);
```

> this的优先级:  new>bind>call(apply)>obj.func()>默认绑定
>
> 非严格模式下js语句中"this"默认指向全局对象(window)
>
> 严格模式下, 普通函数内部的this不会指向window
>
> 上面代码可以转换为`console.log.call(console, this)`



```js
"use strict"
function fn(){
  console.log(this)
}
fn();
// undefined
```

```js
function fn(){
  console.log(this)
}
fn();
// window
```





## 2.this诡异问题, 内存地址



```js
var obj ={
    a:1,
    b:function () {alert(this.a)} 
}; 
var fun =obj.b; // 会指向b函数存储的地址
// 调用的时候就直接使用b, 而和obj.b()的使用不相同
fun(); // 弹出undefined 
obj.b(); // 弹出 1
```

> this的行为有时候会显得极其诡异，让人感到困惑，但只需要记住 **this的值要等到代码真正执行时才能确定**
> 同时this的值具体有以下几种情况：
>
> 1. new 调用时指的是被构造的对象
> 2. call、apply调用，指向我们指定的对象
> 3. 对象调用，如执行obj.b()，this指向obj
> 4. 默认的，指向全局变量window(相当于执行window.fun())
>
> 这样看来，当你执行fun()的时候，以上1,2点均不满足。
> 第3点,因为this是运行时确定的，而我们执行fun()，等同于windown.fun()(**与obj没有任何关系**)，自然的this指向window，而window没有定义变量a，结果是undefined。