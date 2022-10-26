# Buffer

> **buffer 即缓存，是对二进制数据处理的一种方式。**
>
> ES6 推出才正式有了 `ArrayBuffer`

## ArrayBuffer

> - 最基础的二进制对象，是对固定长度的连续内存空间的引用
>
> - `Uint8Array`，`Uint16Array`，`Uint32Array`可以理解为`ArrayBuffer`的翻译器

- `Uint8Array` 将 `ArrayBuffer` 中的每个字节视为一个单位。每个单位是 0 到 255 之间的数字。之所以是255，是因为每个单位最多是 8 位，即 2^8 次方。
- `Uint16Array` 将 `ArrayBuffer` 中每 2 个字节视为一个单位。每个单位是 0 到 65535 之间的整数, 2^16。
- `Uint32Array` 将 `ArrayBuffer` 中每 4 个字节视为一个单位。每个单位是 0 到 4294967295 之间的整数。2^32。

```js
// 我们可以通过 BYTES_PER_ELEMENT 静态属性来得之视图单位的大小
const buf8 = new Uint8Array();
const buf16 = new Uint16Array();
const buf32 = new Uint32Array();
console.log(buf8.BYTES_PER_ELEMENT); // 1
console.log(buf16.BYTES_PER_ELEMENT); // 2
console.log(buf32.BYTES_PER_ELEMENT); // 4
```

## TypedArray

> `TypedArray` 是 `Uint8Array`， `Uint16Array`, `Uint32Array` 统称, 都是 `TypedArray` （类型数组）的一种形式罢了。

### DataView

> `DataView` 就是一种更灵活的视图

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

### Node 中的 Buffer

- `Buffer.alloc(size[, fill[, encoding]])` 创建一个 buffer 空间，可以填充制定元素，也可以指定编码类型

- ```
  Buffer.from()
  ```

   以 buffer 方式存储内容

  - `Buffer.from(array)` 创建一个 buffer 数组 buf，`buf.values()` 返回一个可以迭代的对象
  - `Buffer.from(string[, encoding])` 创建一个 buffer 字符串，可以指定编码类型
  - `Buffer.from(buffer)` 拷贝一个 buffer 对象

### 前端人眼中的 Buffer

先从前端比较常见的说起。在 Web 开发中，我们可能需要对文件做一些处理。比如**导出excel，下载文件，上传头像等等。** 这些操作其实都是操作二进制数据。

```js
// 伪代码示例
// Blob 上传图片

// fileHandler 为前置工作伪代码
// FileReader 加载图片，cavans 压缩图片等
const canvas = fileHandler()

// 创建 Blob 对象和 FormData
canvas.toBlob(blob => {
  this.imgBlob = blob
}, 'image/jpeg')
let formdata = new FormData()
formdata.append('file', this.imgBlob, 'img.jpeg')

// 上传图片
axios({
  headers: {
    "Content-Type": "multipart/form-data"
  },
  method: "post",
  url: uploadUrl,
  data: formdata
})
  .then(res => {
    // do something
  })
  .catch(err => {
    // do something
  });
```

### 大前端眼中的 Buffer

大前端可能就涉及一些前端工程化，脚手架，打包工具等。这时候就可以通过流式处理操作 Buffer。比如**按行读取某个配置文件**，处理 webpack 的一些工作流，和 cli 交互读取和写入文件等等。

```ts
// 伪代码示例
// 逐行读取配置文件，个性化配置

const fs = require("fs");
const readline = require("readline");

// 创建可读流
const rl = readline.createInterface({
  input: fs.createReadStream("theme.less"),
});

rl.on("line", (line: string) => {
  if (line.trim().startsWith("configStart")) {
    // 伪代码，处理变量
    themeHandlerStart()
  }
  if (line.trim().startsWith("configStart")) {
    // 伪代码，处理变量
    themeHandlerEnd()
  }
});
```

### 服务端眼中的 Buffer

对于服务端的同学来说，buffer 的应用就更广泛了
 比如压缩和解压缩，比如加密解密，信息脱敏等等，其实都和 buffer 脱不了干系

此外因为 buffer 是操作二进制对象，所以他的性能和灵活性比常规的 js API 会强很多

- 比如要求更快速的响应时，http 直接传输 buffer 会比传输字符串的效率更高
- 比如日志持久化需要节省空间时，用不同的编码来压缩空间等等

用 Buffer 能更自由灵活的去调和时间复杂度和空间复杂度之间的关系

```js
// 伪代码示例
// 压缩文件

const { createGzip } = require("zlib");
const { pipeline } = require("stream");
const { createReadStream, createWriteStream } = require("fs");

const gzip = createGzip();
const source = createReadStream("./package.json");
const destination = createWriteStream("./package.json.gz");

pipeline(source, gzip, destination, (err) => {
  if (err) {
    console.error("发生错误:", err);
    process.exitCode = 1;
  }
});
```
