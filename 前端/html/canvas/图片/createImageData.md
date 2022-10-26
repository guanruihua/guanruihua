# createImageData

> 创建一个全新的空的ImageData对象。该对象中的所有像素信息都是透明黑

```js
context.createImageData(width, height); 
context.createImageData(imagedata);
```

返回值是ImageData对象，包含width，height和data这3个只读属性。参数具体含义如下：
> `width { Number }` ImageData 对象包含的width值。如果ImageData对象转换成图像，则此width也是最终图像呈现的宽度
> `height { Number }` ImageData 对象包含的height值。如果ImageData对象转换成图像，则此height也是最终图像呈现的高度
> `imagedata { Object }` 一个存在的ImageData对象，只会使用该ImageData对象中的width和height值，包含的像素信息会全部转换为透明黑

```html
<canvas id="canvas"></canvas>
```

```js
// 绘制在Canvas上
var context = canvas.getContext('2d');
var imagedata = context.createImageData(300, 150);
// 给对应坐标位置的数据设置色值为绿色
for (var x = 1; x <= 300; x+=5) {
    for (var y = 1; y <= 150; y+= 5) {
        var index = 4 * ((y - 1) * 300 + (x - 1));
        // 变为绿色，色值依次是0, 128, 0, 256
        imagedata.data[index] = 0;
        imagedata.data[index + 1] = 128;
        imagedata.data[index + 2] = 0;
        imagedata.data[index + 3] = 256;
    }
}
// 再重绘
context.putImageData(imagedata, 0, 0);
```

![](/__assets__/img/2022-02-15-11-09-10.png)
> 直接使用createImageData()方法创建一个图像，例如，所有位置是5的倍数的地方我们塞入一个绿色颜色值，这样可以得到一个点阵图效果
