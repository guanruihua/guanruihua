# putImageData

> - 将ImageData对象的数据绘制到位图上
> - 若提供举行, 则仅绘制矩形的像素
> - 不受画布变换矩阵影响

```js

context.putImageData(imagedata, dx, dy);
context.putImageData(imagedata, dx, dy, dirtyX, dirtyY, dirtyWidth, dirtyHeight);
```

参数| 类型| 作用
|:----|:----|:----|
imagedata| Object| 包含图像像素信息的ImageData对象
dx |Number| 目标Canvas中被图像数据替换的起点横坐标
dy | Number |目标Canvas中被图像数据替换的起点纵坐标
dirtyX（可选）|Number |图像数据渲染区域的左上角横坐标。默认值是0
dirtyY（可选）|Number| 图像数据渲染区域的左上角纵坐标。默认值是0
dirtyWidth（可选）|Number| 图像数据渲染区域的宽度。默认值是imagedata图像的宽度
dirtyHeight（可选）|Number| 图像数据渲染区域的高度。默认值是imagedata图像的高度

> 3类参数：
>
> - 第1类: `imagedata`，就是用来替换当前已有的Canvas画布上的ImageData数据对象。
>
> - 第2类: `dx`，`dy`，这两个参数是作用在Canvas画布上的。imagedata你可以看成是一个即将贴在Canvas画布上“图片膏药”，究竟贴在什么位置呢？dx，dy参数就是告诉这个膏药，你的左上角位置就是这里。
>
> - 第3类: `dirtyX`, `dirtY`, `dirtyWidth`, `dirtyHeight`, 表示矩形的坐标喝尺寸, 这些参数是作用在`imageData`上, `imageData` 所有数据都参与替换, 同过这几个参数设置, 让其中部分数据用来替换
>
> 需要注意的是，其坐标系并没有发生任何变化。从效果表现上看，可以看成是脏矩形外面的像素被当做透明像素处理了。

```html
<img id="image1" src="./1.jpg" alt="目标图片">
<img id="image2" src="./1.jpg" alt="数据源图片">

<canvas id="canvas" width="300" height="200"></canvas>
```

```js
// 尺寸
var width = 300, height = 200;
// 目标Canvas上下文
var context = canvas.getContext('2d');
// 目标Canvas绘制
context.drawImage(image1, 0, 0, width, height);
// 获取覆盖图数据
var dirtyCanvas = document.createElement('canvas');
var dirtyContext = dirtyCanvas.getContext('2d');
// 设置屏幕外Canvas尺寸
dirtyCanvas.width = width;
dirtyCanvas.height = height;
// 绘制替换图
dirtyContext.drawImage(image2, 0, 0, width, height);
// 此时可以得到imagedata数据
var imagedata = dirtyContext.getImageData(0, 0, width, height);
// 然后中间100*100区域替换目标Canvas
context.putImageData(imagedata, 0, 0, 100, 50, 100, 100);
```
