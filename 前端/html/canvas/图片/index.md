# 图片

## getImageData

> `CanvasRenderingContext2D.getImageData()`返回一个`ImageData`对象，其中包含Canvas画布部分或完整的像素点信息
> 对图像进行像素级处理
> `getImageData()`方法可能会出现CORS跨域报错

`context.getImageData(sx, sy, sWidth, sHeight)`

|参数| 作用|
|:----|:----|
|`sxNumbe` | 需要返回的图像数据区域的起始横坐标|
|`syNumber` | 需要返回的图像数据区域的起始纵坐标|
|`sWidthNumber` | 需要返回的图像数据区域的宽度|
|`sHeightNumber` | 需要返回的图像数据区域的高度|

```html
<canvas id="canvas" width="250" height="167"></canvas>
```

```js
var img = new Image();
img.onload = function () {
    var context = canvas.getContext('2d');
    // 图片绘制
    context.drawImage(this, 0, 0, 250, 167);
    // 然后获取中间100*100区域数据
    var imageData = context.getImageData(75, 34, 100, 100);
    var length = imageData.data.length;
    for (var index = 0; index < length; index += 4) {
        var r = imageData.data[index];
        var g = imageData.data[index + 1];
        var b = imageData.data[index + 2];
        // 计算灰度
        var gray = r * 0.299 + g * 0.587 + b * 0.114;
        imageData.data[index] = gray;
        imageData.data[index + 1] = gray;
        imageData.data[index + 2] = gray;
    }
    // 更新新数据
    context.putImageData(imageData, 75, 34);
};
img.src = './1.jpg';
```

![](./__assets__/index-2022-03-29-15-49-12.png)
