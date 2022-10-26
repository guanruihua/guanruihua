---
title: call&apply&bind
date: 2020-11-10 20:21:02
tags:
- front-end
- javascript
- api
---

# call&apply&bind

> call, bind, apply第一参数都是this的指向对象
>
> call和bind的写法几乎相同, 但是bind是返回的是函数
>
> apply只能传输一个变量

```js
var name = 'guanruihua';
var age = '23';
var obj = {
  name: 'mawenliang',
  age: 22,
  fn: function() {
    console.log( this.name + this.age )
  },
  fn2: function(fm, ft) {
    console.log( this.name + this.age + " aa "+ fm + " bb " + ft)
  }

}
var obj2 = {
  name: 'haungzelin',
  age: 21,
}

obj.fn.call(obj2);
obj.fn.apply(obj2);
obj.fn.bind(obj2)();

obj.fn2.call(obj2, '成都', '上海');
obj.fn2.apply(obj2, ['成都', '上海']);
obj.fn2.bind(obj2, '成都', '上海')();

```

运行结果

```js
haungzelin21
haungzelin21
haungzelin21
haungzelin21 aa 成都 bb 上海  
haungzelin21 aa 成都 bb 上海  
haungzelin21 aa 成都 bb 上海  
```

