# drawImage

```js
context.drawImage(image, dx, dy);
context.drawImage(image, dx, dy, dWidth, dHeight);
context.drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
```

> `image { Object }`: 绘制在Canvas上的元素，可以是各类Canvas图片资源（见CanvasImageSource），如`<img>`图片，SVG图像，Canvas元素本身等
> `dx { Number }`: 在Canvas画布上规划一片区域用来放置图片，dx就是这片区域的左上角横坐标
> `dy { Number }`: 在Canvas画布上规划一片区域用来放置图片，dy就是这片区域的左上角纵坐标
> `dWidth { Number }`: 在Canvas画布上规划一片区域用来放置图片，dWidth就是这片区域的宽度
> `dHeight { Number }`: 在Canvas画布上规划一片区域用来放置图片，dHeight就是这片区域的高度
> `sx { Number }`: 表示图片元素绘制在Canvas画布上起始横坐标
> `sy { Number }`: 表示图片元素绘制在Canvas画布上起始纵坐标
> `sWidth { Number }`: 表示图片元素从坐标点开始算，多大的宽度内容绘制Canvas画布上
> `sHeight { Number }`: 表示图片元素从坐标点开始算，多大的高度内容绘制Canvas画布上

`context.drawImage(image, 0, 0, 300, 150);`
![](/__assets__/img/2022-02-15-14-17-29.png)
> 原图: 500 * 333, 这样子设置会被压扁了些
