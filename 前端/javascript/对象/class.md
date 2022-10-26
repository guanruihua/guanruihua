# Class

- class 相对 function 是后出来的，既然 class 出来了，显然是为了解决 function 在处理面向对象设计中的缺陷而来
- 下面通过对比，来看看 class 作为 ES6 中的重大升级之一的优势在哪里：为了更好的对比，请参见我的另外一篇博文： js面向对象设计之 function 类
  - class 写法更加简洁、含义更加明确、代码结构更加清晰。
  - class 尽管也是函数，却无法直接调用（不存在防御性代码了）。
  - class 不存在变量提升。
  - class 为污染 window 等全局变量
  - class 函数体中的代码始终以严格模式执行
  - 可直接使用 set 和 get 函数, 而function 中需要通过`Object.defineProperty`方法来设置get和set
  - class 中的 this 永远都不会指向 window。
  - class 可以从 javascript 中的几大类中进行继承：Array、number、string...
  - class 中有一个对象super，这个对象可以取到父类的方法、构造函数等。
  - class 中不存在实例方法，class 中定义所有方法都是原型方法。这些方法也都是不可枚举的，使用 for in 这种方式无法遍历到它们。
  - class 不能使用 return 来返回一个实例

```js
/* ReferenceError: Class01 is not defined */
try { var ins01 = new Class01(); } catch ( e ) { console.error( e ); }
class Class01 { }
console.log( typeof Class01 ); /* function */
/* Class constructor Class01 cannot be invoked without 'new' */
try { Class01() } catch ( e ) { console.error( e ); }
console.log( window.Class01 ); /* undefined */
```

## set & get

```js
// 可直接使用 set 和 get 函数, 而function 中需要通过`Object.defineProperty`方法来设置get和set
class Class01{
    constructor() { }
    get name(){
        console.log( 'getter' );
        return this._name;
    }
    set name( v ){
        this._name = v;
        console.log( 'setter' );
        return this;
    }
}
var ins01 =  new Class01();
ins01.name; /* getter */
ins01.name = 2; /* setter */
```

## class this 不会指向 window

```js
// class 中的 this 永远都不会指向 window。
class Class01 {
  
    constructor() {
        this.a = 'a';
    }
    geta(){
        return this.a;
    }
}
let ins01 = new Class01();
console.log( ins01.geta() ); /* a */
let obj = {};
obj.a = 'objA';
obj.geta = ins01.geta;
console.log( obj.geta() ); /* 'objA' */
window.a = 'windowA';
window.geta = ins01.geta;
/* Cannot read property 'a' of undefined */
/* 若是 function 类此处会返回 'windowA' */
try { geta() } catch ( e ) { console.error( e ); }
```

## class 继承

```js
// class 可以从 javascript 中几大类中进行继承：Array、number、string....
class Class01 extends Array { }
let ins01 = new Class01( 1, 2, 3 ); /* [1,2,3] */
let arr = ins01.shift(); /* [2,3] */
arr instanceof Class01; /* false */
ins01 instanceof Class01; /* true */小tips：在 mozilla 的开发者指南中看到一种比较高端的东西（关于从原生类继承肯定还有话题，会继续学习）：
static get [Symbol.species]() { return Array; }参见 https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes
```

## super对象

```js
// class 中有一个对象super，这个对象可以取到父类的方法、构造函数等。
class Class01 {
    constructor( name ){
        this.name = name;
    }
    geta(){
        console.log( this.name + ' 父类方法.' );
    }
}
class Class02 extends Class01 {
    geta(){
        super.geta();
        console.log( this.name + ' 子类方法.' );
    }
}
var l = new Class02( 'Class02' );
l.geta();
```

```js
// 11、class 不能使用 return 来返回一个实例（等等，我还没有试验过，不好意思，我会马上试验一下）。　　
// 编辑验证，class 的 constructor 可以使用 return，因此 class 与 function 一样可以返回任意的内容。若不写 return 语句，或返回是数值、字符串等非引用类型的值，则 constructor 任然会返回 this（实例）。return [] 或 return {} 都会使得 new 关键字并不会返回 class 的实例说了这么多的不同点，再来说说 class 和 function 的相同之处：

// 1、静态方法  在这点上，两者还是有相似之处的。尽管相似，显然 class 要更加优雅，可读性也更强，class 使用 static 关键词指定静态方法。并且class 可以在函数体内定义静态函数，而 function 不能，这无疑也让 function 写出来的代码更加的复杂。

class Class01{
    static geta(){
        console.log( '01的静态方法' );
    }
}
function Class02 () { }
Class02.geta = function () { console.log( '02的静态方法' ); }
Class01.geta();
Class02.geta();

// 2、私有属性和方法两者都必须采用闭包的方式才能实现。下面给出 class 的私有属性和方法。
var Class02 = ( function () {
    let pVal ;
    function sayHello(){
        console.log( this ); /* this 指向 window */
        console.log( '欢迎访问nDos的博客' );
    }
    return class Class01{
        constructor( v = '初始值' ){
            pVal = v;
        }
        get val(){
            sayHello();
            return pVal;
        }
        set val( v ){
            pVal = v;
        }
    };
} )();
let ins01 = new Class02();
console.log( ins01.val );
ins01.val = 'hello';
console.log( ins01.val );
ins01 instanceof Class02; /* true */
ins01.constructor.name; /* "Class01" */
// 小tips：上面的代码显示 ins01 是 Class02 的实例，但 ins01 的构造函数 name 属性却是 Class01。显然这在项目中不可行，会给类的使用者造成困惑。下例可解决这个问题：
var Class01 = ( function () {
    return class Class01 { };
} )();
let ins01 = new Class01();
ins01 instanceof Class01; /* true */
ins01.constructor.name; /* "Class01" */


```
