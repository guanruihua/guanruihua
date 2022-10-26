# Class

## 声明类

```js
class Animal {
    constructor(type) {
        this.type = type
    }
    walk() {
        console.log( `I am walking` )
    }
}
let dog = new Animal('dog')
let monkey = new Animal('monkey')
```



## Setters & Getters

> 可以直接在`constructor`中通过this直接定义, 还可以直接在类的顶层来定义

```js
class Animal {
    constructor(type, age) {
        this.type = type
        this._age = age
    }
    get age() {
        return this._age
    }
    set age(val) {
        this._age = val
    }
}
```



## 静态方法



```js
class Animal {
    constructor(type) {
        this.type = type
    }
    walk() {
        console.log( `I am walking` )
    }
    static eat() {
        console.log( `I am eating` )
    }
}
```



## 继承



```js
class Animal {
    constructor(type) {
        this.type = type
    }
    walk() {
        console.log( `I am walking` )
    }
    static eat() {
        console.log( `I am eating` )
    }
}

class Dog extends Animal {
  constructor () {
    super('dog')
  }
  run () {
    console.log('I can run')
  }
}
```

