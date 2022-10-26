# translate

> 对Canvas坐标进行整体位移, 实际开发中常用来改变其他变换的方法的变换中心点

`context.translate(x, y);`

|参数| 类型 |作用|
|:----|:----|:----|
|x| Number| 坐标系水平位移的距离|
|y| Number| 坐标系垂直位移的距离|

```html
<canvas id="canvas" width="300" height="200"></canvas>
```

```js
var img = new Image();
img.onload = function () {
    var context = canvas.getContext('2d');
    // 坐标位移
    context.translate(150, 100);
    // 旋转45度
    context.rotate(45 * Math.PI / 180);
    // 再位移回来
    context.translate(-150, -100);
    // 此时绘制图片就是中心旋转了
    context.drawImage(this, 0, 0, 300, 200);

    // 坐标系还原
    context.setTransform(1, 0, 0, 1, 0, 0);
};
img.src = './1.jpg';
```

![](./__assets__/translate-2022-04-01-16-30-05.png)
