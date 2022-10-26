# static 关键词

> 静态属性 和 静态方法都是通过类(构造函数) 直接使用
>
> 引用属性 或 调用方法 的时候, 没有必要首先创建类 ( 构造函数 ) 的对象实例

## 静态属性

> - 给Antzone添加静态属性webName
> - 静态属性直接由类本身引用, 而不是他的对象实例

```js
class Antzone{}
Antzone.webName="grh";
console.log(Antzone.webName);
```

## 静态方法

> - 静态方法是由类或构造函数直接调用, 而不是对象实例
>
> -

```js
function Antzone() {
  Antzone.show = function() {
    console.log('grh')
  }
}
Antzone.show();
```

使用关键词static

```js
class Antzone {
 static show() {
  console.log('grh')
  }
}
let ant = new Antzone();
Antzone.show()
ant.show();// 这里会报ant.show not a function
```

> 1. 通过关键词static定义show()
> 2. 直接通过类调用show(), 而不是对象实例调用
> 3. ant.show() 之所以会报错, 他是调用实例方法show, 然而not 定义

修改

```js
class Antzone {
 static show() {
  console.log('grh')
  }
  show() {
   console.log('grh')
  }
}
let ant = new Antzone();
Antzone.show()
ant.show();
```

> 静态方法与实例化方法可以重名

```js
class Antzone{
  static show(){
    this.done()
  }
  static done(){
    console.log("蚂蚁部落一");
  }
  done(){
    console.log("蚂蚁部落二");
  }
}
let ant=new Antzone();
Antzone.show();
```

> 1. 使用实例对象调用方法, 方法中的this指向此实例对象
> 2. 如果使用类调用静态方法, so 静态function 中的this指向类本身
> 3. show的this指向class Antzone , so this.done() 调用的是方法

## 静态属性 and 静态方法的继承

> - extends 关键字 , 可以实现类之间的继承
>
> - 可以继承父类的实例属性and 实例function, 静态属性and 静态方法也会被继承

```js
class F{
  static func() {
  console.log('grh')
  }
} 
F.address = '珠海'
class C extends F{}
C.func();
console.log(C.address);
```
