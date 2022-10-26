# 基础Api

## 图案对象

> 创建图案对象, 指定平铺方式, 用来作为图案的类型
> `context.createPattern(image, repetition)`
> `return  CanvasPattern`
> `imageObject`
>
> 用来平铺的CanvasImageSource图像。可以是下面的类型：
>
> - HTMLImageElement，也就是`<img>`元素。
> - HTMLVideoElement，也就是`<video>`元素，例如捕获摄像头视频产生的图像信息。
> - HTMLCanvasElement
> - CanvasRenderingContext2D
> - ImageBitmap
> - ImageData
> - Blob
>
> `repetition` `{ String }`
> 图案的平铺方式，可以是下面的值：
>
> - `repeat`，水平和垂直平铺。当repetition属性值为空字符串''或者null，也会按照'repeat'进行渲染
> - `repeat-x`，仅水平平铺
> - `repeat-y`，仅垂直平铺
> - `no-repeat`，不平铺

```html
<canvas id="canvas" width="250" height="167"></canvas>
```

```js
// 先绘制图片
var img = new Image();
img.onload = function () {
 // 我们创建一个Canvas元素
 var canvasCreated = document.createElement('canvas');
 canvasCreated.width = 50;
 canvasCreated.height = 34;
 canvasCreated.getContext('2d').drawImage(this, 0, 0, 50, 34);
 // 页面上需要呈现最终纹理的Canvas上下文
 var context = canvas.getContext('2d');
 // 创建纹理并填充，顺便测试null是否渲染为'repeat'
 var pattern = context.createPattern(canvasCreated, null);
 context.fillStyle = pattern;
 context.fillRect(0, 0, 250, 167);
};
img.src = './1.jpg';
```

![](/__assets__/img/2022-02-15-11-23-51.png)
> 图片缩小，并作为纹理显示。我们直接把`<img>`元素作为纹理图案是无法控制其尺寸的，我们可以将`<img>`元素绘制在大小可控的Canvas元素上，然后把这个Canvas元素作为图案进行平铺即可

## 剪裁

> 先绘制剪裁路径, 执行`clip()`方法, 在绘制内容, 就在这个剪裁路径中呈现

```js
context.clip();
context.clip(fillRule);
context.clip(path, fillRule);
```

> `fillRule { String }`: 填充规则。用来确定一个点实在路径内还是路径外。可选值包括：
>
> - `nonzero`：非零规则 (默认)
> - `evenodd`：奇偶规则。
>
>`pathObject`: 指Path2D对象。

```js
let context = canvas.getContext('2d');
// 需要图片先加载完毕
let img = new Image();
img.onload = function () {
    // 剪裁路径是三角形
    context.beginPath();
    context.moveTo(20, 20);
    context.lineTo(200, 80);
    context.lineTo(110, 150);
    // 剪裁
    context.clip();
    // 填充图片
    context.drawImage(img, 0, 0, 250, 167);
};
img.src = './1.jpg';
```

![](/__assets__/img/2022-02-15-10-50-54.png)
> 利用剪裁实现一个图案填充效果。实现图案填充，标准用法是创建一个Pattern对象，然后作为fillStyle进行路径填充。这里，我们还可以使用clip()剪裁实现，这样就不用new一个Pattern对象了。例如，实现一个三角形，里面是人物照片图案。

## 清除画布

> `context.clearRect(x, y, width, height)`
> `x { Number }`: 矩形左上角x坐标
> `y { Number }`: 矩形左上角y坐标
> `width { Number }`: 被清除的矩形区域的高度
> `height { Number }`: 被清除的矩形区域的宽度度

```js
// 先绘制图片
var img = new Image();
img.onload = function () {
    context.drawImage(img, 0, 0, 250, 167);
    // 中间开个方形的洞
    context.clearRect(50, 50, 100, 66);
};
img.src = './1.jpg';
```

![](/__assets__/img/2022-02-15-10-41-05.png)
> 先把一张图片绘制在Canvas画布上，然后再把中间一块矩形区域的像素信息清除

## 渐变

### 渐变对象

> 创建 渐变对象
> `context.createLinearGradient(x0, y0, x1, y1);`
> `x0 { Number }`:  渐变起始点横坐标
> `y0 { Number }`:  渐变起始点纵坐标
> `x1 { Number }`:  渐变结束点横坐标
> `y1 { Number }`:  渐变结束点纵坐标
> 线性渐变效果比较好脑补，就是从坐标点[x0, y0]到坐标点[x1, y1]的位置画一条线，然后整个渐变色带与与这条线垂直

```js
var context = canvas.getContext('2d');
// 创建渐变
var gradient = context.createLinearGradient(0, 0, 300, 0);
gradient.addColorStop(0, 'red');
gradient.addColorStop(1, 'green');
// 设置填充样式为渐变
context.fillStyle = gradient;
// 左上角和右下角分别填充2个矩形
context.fillRect(10, 10, 160, 60);
context.fillRect(120, 80, 160, 60);
```

![](/__assets__/img/2022-02-15-11-15-04.png)

### 径向渐变

> 创建径向渐变
> 不同于CSS3的径向渐变, Canvas中径向渐变的起始点由两个圆环坐标构成
> `context.createRadialGradient(x0, y0, r0, x1, y1, r1)`
> `return CanvasPattern对象`
> `x0 { Number }`: 起始圆的横坐标
> `y0 { Number }`: 起始圆的纵坐标
> `r0 { Number }`: 起始圆的半径
> `x1 { Number }`: 结束圆的横坐标
> `y1 { Number }`: 结束圆的纵坐标
> `r1 { Number }`: 结束圆的半径

#### 标准径向渐变

> 看似标准两色径向渐变的实现并不是理所当然，反而是需要一些技巧，技巧就是我们的起始圆半径设置为0，化作一个点。例如实现一个红绿渐变

```html
<canvas width="240" height="120"></canvas>
```

```js
var context = canvas.getContext('2d');
// 创建一个起始圆半径为0的径向渐变对象
var gradient = context.createRadialGradient(120, 60, 0, 120, 60, 60);
// 设置起止颜色
gradient.addColorStop(0, 'red');
gradient.addColorStop(1, 'green');
// 矩形填充
context.fillStyle = gradient;
context.fillRect(0, 0, 240, 120);
```

![](/__assets__/img/2022-02-15-11-32-48.png)

#### 色带分隔明显的色环

> 同心五环效果

```html
<canvas width="150" height="150"></canvas>
```

```js
var context = canvas.getContext('2d');
// 创建一个起始圆半径为0的径向渐变对象
var gradient = context.createRadialGradient(75, 75, 0, 75, 75, 75);
// 设置起止颜色
gradient.addColorStop(0, 'red');
gradient.addColorStop(0.2, 'red');
gradient.addColorStop(0.2, 'orange');
gradient.addColorStop(0.4, 'orange');
gradient.addColorStop(0.4, 'yellow');
gradient.addColorStop(0.6, 'yellow');
gradient.addColorStop(0.6, 'green');
gradient.addColorStop(0.8, 'green');
gradient.addColorStop(0.8, 'purple');
gradient.addColorStop(1, 'purple');
gradient.addColorStop(1, 'transparent');
// 矩形填充
context.fillStyle = gradient;
context.fillRect(0, 0, 150, 150);
```

![](/__assets__/img/2022-02-15-11-36-09.png)
> 不同浏览器还是有差异的，Chrome浏览器下的锯齿比较明显
> 实际上，如果起始渐变圆的半径如果不是0，则在Firefox浏览器下，第5环会无法呈现，且也会出现明显锯齿，这个案例可参见CanvasGradient.addColorStop

## 状态保存&恢复

```js
save()// 状态存储
restore()// 恢复上一次状态
```
