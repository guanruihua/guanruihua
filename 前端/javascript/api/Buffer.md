# Buffer

> buffer 即缓存，是对二进制数据处理的一种方式

## Uint*Array / ArrayBuffer

> ArrayBuffer: 基础的二进制对象, 对固定长度的连续内存控件的引用
> 视图: `Uint8Array`, `Uint16Array`,`Uint32Array`

- Uint8Array 将 ArrayBuffer 中的每个字节视为一个单位。每个单位是 0 到 255 之间的数字。之所以是255，是因为每个单位最多是 8 位，即 2^8 次方。
- Uint16Array 将 ArrayBuffer 中每 2 个字节视为一个单位。每个单位是 0 到 65535 之间的整数。原理同上。
- Uint32Array 将 ArrayBuffer 中每 4 个字节视为一个单位。每个单位是 0 到 4294967295 之间的整数。原理同上。

```js
// 通过 BYTES_PER_ELEMENT 静态属性来得之视图单位的大小
const buf8 = new Uint8Array();
const buf16 = new Uint16Array();
const buf32 = new Uint32Array();
console.log(buf8.BYTES_PER_ELEMENT); // 1
console.log(buf16.BYTES_PER_ELEMENT); // 2
console.log(buf32.BYTES_PER_ELEMENT); // 4
```

## Blob

> 浏览器环境一种承载原始数据的二进制对象, 可以与`ArrayBuffer`相互转化

## DataView

> 一种底层,更灵活读取`ArrayBuffer`的视图

```js
const buffer = new ArrayBuffer(16); // 分配一个内存空间
const view = new DataView(buffer); // 创建 DataView 视图
view.setUint32(0, 4294967295); // 从第 0 个空间开始，以 32 位的形式写入数据

// 有时候我想以 8 位的形式 “翻译” 这个内存空间，从偏移量 0 开始翻译
console.log(view.getUint8(0)); // 255
// 今天心情好，想以 16 位的形式 “翻译” 这个内存空间，从偏移量 0 开始读
console.log(view.getUint16(0)); // 65535
// 今天心情超好，想以 32 位的形式 “翻译” 这个内存空间，从偏移量 0 开始读
console.log(view.getUint32(0)); // 4294967295
```

## Node 中 Buffer

> <http://nodejs.cn/api/buffer.html>
