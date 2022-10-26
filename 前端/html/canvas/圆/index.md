# 圆

## 圆弧 & 正圆

### arc

> `context.arc(x, y, radius, startAngle, endAngle [, anticlockwise]);`
>
> `x { Number }`: 圆弧对应的圆心横坐标
> `y { Number }`: 圆弧对应的圆心纵坐标
> `radius { Number }`: 圆弧的半径大小。
> `startAngle { Number }`: 圆弧开始的角度，单位是弧度。
> `endAngle { Number }`: 圆弧结束的角度，单位是弧度。
> `anticlockwise（可选）{ Boolean }`: 弧度的开始到结束的绘制是按照顺时针来算，还是按时逆时针来算。如何设置为`true`，则表示按照逆时针方向从`startAngle`绘制到`endAngle`。

```js
// 顺时针绘制0到1/4弧度圆弧
context.beginPath();
context.arc(150, 75, 50, 0, Math.PI / 2);
context.stroke();
```

![](/__assets__/img/2022-02-15-10-04-26.png)

### arcTo

> - 正圆圆弧, 给路径添加圆弧
> - 根据 两个控制点 (x1,y1) 和 (x2, y2)以及半径绘制弧线 同时连接两个控制点
> `context.arcTo(x1, y1, x2, y2, radius);`
>
> `x1 { Number }`: 第1个控制点的横坐标
> `y1 { Number }`: 第1个控制点的纵坐标
> `x2 { Number }`: 第2个控制点的横坐标
> `y2 { Number }`: 第2个控制点的纵坐标
> `radius { Number }`: 圆弧的半径大小

```js
context.beginPath();
context.moveTo(50, 50);
context.arcTo(150, 100, 200, 40, 40);
context.lineTo(200, 40);
context.stroke();
```

![](/__assets__/img/2022-02-15-10-11-03.png)

## 椭圆

> 绘制椭圆
> 不支持IE浏览器, Edge13+支持
> `context.ellipse(x, y, radiusX, radiusY, rotation, startAngle, endAngle, anticlockwise);`
> `x { Number }`: 椭圆弧对应的 圆心横坐标
> `y { Number }`: 椭圆弧对应的 圆心纵坐标
> `radiusX { Number }`: 椭圆弧的长轴半径大小
> `radiusY { Number }`: 椭圆弧的短轴半径大小
> `rotation { Number }`: 椭圆弧的旋转角度，单位是弧度
> `startAngle { Number }`: 圆弧开始的角度，角度从横轴开始算，单位是弧度
> `endAngle { Number }`: 圆弧结束的角度，单位是弧度
> `anticlockwise（可选）{ Boolean }`: 弧度的开始到结束的绘制是按照顺时针来算，还是按时逆时针来算。如何设置为true，则表示按照逆时针方向从 startAngle 绘制到 endAngle

```js
// 绘制椭圆
context.ellipse(150, 75, 80, 40, Math.PI / 4, 0, 2 * Math.PI);
context.stroke();
```

![](/__assets__/img/2022-02-15-14-35-03.png)
