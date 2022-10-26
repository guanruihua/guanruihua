# fs模块

### 前言

Node.js中赋予了JavaScript很多在浏览器中没有的能力，譬如：文件读写，创建http服务器等等，今天我们就来看看在node中怎样用JavaScript进行文件的读写操作。

1. 读文件
   1. 我们在data文件夹下新建一个`hello.txt`，并且在里面写入：`hello， node.js!!` ，如图：

1. 我们在`hello.txt`同级目录下创建一个`hello.js`文件，我们在这个js文件中利用Node提供的文件操作API, 读取`hello.txt`文件中的内容。

- node中对文件相关的操作需要依赖fs模块，这个是node中内置模块之一，我们需要引入。fs--file system。

```js
   let fs = require('fs')
   fs.readFile() 
   
    // 读文件。 readFile函数接受两个参数：读取文件路径，回调函数（error，data两个参数），
   读取文件成功：data为文件内容，error为null，读取失败：error为错误对象，data为undefined
```

   最后我们`hello.js`中的代码如下:

```js
let fs = require('fs')
fs.readFile('./hello.txt', (error, data) => {
  console.log(data.toString())
})
   
```

   在这里可以说一下，我们读取回来的默认是二进制的内容，所以需要调用toString()方法进行转换。最后，终端可以看到结果如下：

  可以看到我们刚才在`hello.txt`中写入的文本`hello, node.js!!`已经打印出来。看到这里是不是觉得很牛叉，JavaScript居然可以用来读取文件内容，完全颠覆了我们以前对JavaScript的理解，然而这一切都得归功于Node.js。

1. 写文件

   我们在刚才的`hello.js`中写入下面这行代码：

   ```js
   fs.writeFile('./hello.md', '你好，node.js!', (error) => {

         if (!error) {
           console.log('创建成功了。。')
         }
   }) 
   // 写文件。writeFile接受三个参数：写入文件路径，写入内容，回调函数。
      ```

   写入成功时候：error为null，写入失败时候：error为错误对象

   最后我们看到在同级目录下出现了一个`hello.md`文件，并且里面的内容为`你好，node.js`. 如图：

### 删除文件

### 语法

以下为删除文件的语法格式：

```
fs.unlink(path, callback)
```

### 参数

参数使用说明如下：

- **path** - 文件路径。
- **callback** - 回调函数，没有参数。

### 实例

input.txt 文件内容为：

```
site:www.runoob.com
```

接下来我们创建 file.js 文件，代码如下所示：

```
var fs = require("fs");

console.log("准备删除文件！");
fs.unlink('input.txt', function(err) {
   if (err) {
       return console.error(err);
   }
   console.log("文件删除成功！");
});
```

以上代码执行结果如下：

```
$ node file.js 
准备删除文件！
文件删除成功！
```

再去查看 input.txt 文件，发现已经不存在了。

------

### 创建目录

### 语法

以下为创建目录的语法格式：

```
fs.mkdir(path[, options], callback)
```

### 参数

参数使用说明如下：

- **path** - 文件路径。
- options 参数可以是：
  - **recursive** - 是否以递归的方式创建目录，默认为 false。
  - **mode** - 设置目录权限，默认为 0777。
- **callback** - 回调函数，没有参数。

### 实例

接下来我们创建 file.js 文件，代码如下所示：

```js
var fs = require("fs");
// tmp 目录必须存在
console.log("创建目录 /tmp/test/");
fs.mkdir("/tmp/test/",function(err){
   if (err) {
       return console.error(err);
   }
   console.log("目录创建成功。");
});
```

以上代码执行结果如下：

```
$ node file.js 
创建目录 /tmp/test/
目录创建成功。
```

可以添加 recursive: true 参数，不管创建的目录 /tmp 和 /tmp/a 是否存在：

```js
fs.mkdir('/tmp/a/apple', { recursive: true }, (err) => {
  if (err) throw err;
});
```

------

### 读取目录

### 语法

以下为读取目录的语法格式：

```
fs.readdir(path, callback)
```

### 参数

参数使用说明如下：

- **path** - 文件路径。
- **callback** - 回调函数，回调函数带有两个参数err, files，err 为错误信息，files 为 目录下的文件数组列表。

### 实例

接下来我们创建 file.js 文件，代码如下所示：

```js
var fs = require("fs");

console.log("查看 /tmp 目录");
fs.readdir("/tmp/",function(err, files){
   if (err) {
       return console.error(err);
   }
   files.forEach( function (file){
       console.log( file );
   });
});
```

以上代码执行结果如下：

```
$ node file.js 
查看 /tmp 目录
input.out
output.out
test
test.txt
```

------

### 删除目录

### 语法

以下为删除目录的语法格式：

```
fs.rmdir(path, callback)
```

### 参数

参数使用说明如下：

- **path** - 文件路径。
- **callback** - 回调函数，没有参数。

### 实例

接下来我们创建 file.js 文件，代码如下所示：

```
var fs = require("fs");
// 执行前创建一个空的 /tmp/test 目录
console.log("准备删除目录 /tmp/test");
fs.rmdir("/tmp/test",function(err){
   if (err) {
       return console.error(err);
   }
   console.log("读取 /tmp 目录");
   fs.readdir("/tmp/",function(err, files){
      if (err) {
          return console.error(err);
      }
      files.forEach( function (file){
          console.log( file );
      });
   });
});
```

输入输出

```
// 引入readline模块
var readline = require('readline');
    
//创建readline接口实例
var  rl = readline.createInterface({
    input:process.stdin,
    output:process.stdout
});

// question方法
rl.question("你的名字是？",function(answer){
    console.log("我的名字是："+answer);
    // 不加close，则程序不会结束
    rl.close();
});

// close事件监听
rl.on("close", function(){
   // 结束程序
    process.exit(0);
})
```
