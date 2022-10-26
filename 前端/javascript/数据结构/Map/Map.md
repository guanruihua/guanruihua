# Map

> - 类似对象
> - 键值对的集合
> - '键' 不限于 字符串

## 实例化

```js
let map = new Map([iterable]);
```

> - Iterable 可以是一个数组或其他的Iterable对象, 其元素为键值对( 两个元素的数组,  eg:`[[1, 'one'],[2,`'two']]`)
> - `null` 会被当做 `undefined`

## 添加 set

```js
let keyObj = {}
let keyFunc = function() {}
let keyString = 'a string'

// 添加键
map.set(keyString, "和键'a string'关联的值")
map.set(keyObj, '和键keyObj关联的值')
map.set(keyFunc, '和键keyFunc关联的值')
```

### 删除 delete & clear

```js
// 删除指定的数据
map.delete(keyObj)
// 删除所有数据
map.clear()
```

### 统计 size

```js
// 统计所有 key-value 的总数
console.log(map.size) //2
// 判断是否有 key-value
console.log(map.has(keyObj)) // true
```

### 查询 get

```js
console.log(map.get(keyObj));
```

## 遍历方法

> - `keys`:  返回一个新的Iterator对象,  (按照插入map对象的每个key值)
> - `values`: 返回一个新的Iterrator对象
> - `entries` : 返回一个新的包含`[ key, value]` 对的迭代(`Iterator`)对象
> - `forEach` : 会以插入顺序对Map对象每一个键值对执行应该参数提供回调函数
> - `for...of`: 直接遍历每一个成员

```js
map.forEach((value, key) => console.log(value, key))

for (let [key, value] of map) {
 console.log(key, value)
}

for (let key of map.keys()) {
 console.log(key)
}

for (let value of map.values()) {
 console.log(value)
}

for (let [key, value] of map.entries()) {
 console.log(key, value)
}
```
