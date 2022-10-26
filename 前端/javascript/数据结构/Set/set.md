# Set

> - 在 JavaScript 里通常使用 Array 或 Object 来存储数据。
>
> - 但是在频繁操作数据的过程中查找或者统计并需要手动来实现，并不能简单的直接使用。
> - 比如如何保证 Array 是去重的，如何统计 Object 的数据总数等，必须自己去手动实现类似的需求，不是很方便。
> - 在 ES6 中为了解决上述痛点，新增了数据结构 Set 和 Map，它们分别对应传统数据结构的“集合”和“字典”
> - 特性
>   - 不允许数据重复

## 基本用法

### 生成Set实例

```js
let s = new Set();
let s = new Set([1,2,3,4]);
```

### 添加数据

```js
s.add('a');
s.add('b');
// or
s.add('a').add('b');
```

### 删除数据

```js
// 删除指定
s.delete('a');
// 删除全部
s.clear();
```

### 统计数据

> - `has`: 是否拥有该元素
> - `size`: 统计数据的总数

```js
// 判断是否拥有数据项, 返回boolean
s.has('a')// true
// 计算总数
s.size // 2
```

## 遍历方法

> `keys`: 键名的遍历器
>
> `values`: 键值遍历器
>
> `entries`: 返回键值对的遍历器
>
> `forEach`: 使用回调函数遍历每个成员
>
> `for...of`: 可以直接遍历每一个成员

```js
console.log(s.keys()) // SetIterator {"hello", "goodbye"}
console.log(s.values()) // SetIterator {"hello", "goodbye"}
console.log(s.entries()) // SetIterator {"hello" => "hello", "goodbye" => "goodbye"}
s.forEach(item => {
    console.log(item) // hello // goodbye
})

for (let item of s) {
    console.log(item)
}

for (let item of s.keys()) {
    console.log(item)
}

for (let item of s.values()) {
    console.log(item)
}

for (let item of s.entries()) {
    console.log(item[0], item[1])
}
```

## set 运用

### 数据去重

```js
let arr = [1,2,3,4,5,6,7]
let s = new Set(arr);
```

### 合并去重

```js
let arr1 = [1,2,3];
let arr2 = [2,3,4];
let s = new Set([...arr1, ...arr2]);
console.log(s)
console.log([...s])
console.log(Array.from(s))
```

### 交集

```js
// let s1 = new Set(arr1);
let s2 = new Set(arr2);
let result = new Set(arr1.filter(item => s2.has(item)))
Array.from(result);
```
