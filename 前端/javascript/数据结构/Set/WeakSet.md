# WeakSet

> - 和Set类似
> - WeakSet成员只能是对象,不能是其他类型的值
> - 没有size属性
> - 不可以遍历它的成员
> - WeakSet 的 对象是弱引用
> - 垃圾回收机制不用考虑WeakSet对该对象的引用



```js
const ws = new WeakSet()
ws.add(1)
// TypeError: Invalid value used in weak set
ws.add(Symbol())
// TypeError: invalid value used in weak set
let ws = new WeakSet()
const obj1 = {
    name: 'imooc'
}
const obj2 = {
    age: 5
}
ws.add(obj1)
ws.add(obj2)
ws.delete(obj1)
console.log(ws)
console.log(ws.has(obj2))
```

