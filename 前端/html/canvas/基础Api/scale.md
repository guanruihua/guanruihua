# scale 缩放
>
> - 不会影响之前绘制好的
> - 影响之后的绘制
> - 默认缩放中心 (0, 0)

## `context.scale(x, y)`

参数 | 类型 | 作用
|:----|:----|:----|
`x` |`Number` | Canvas坐标系水平缩放的比例。支持小数，如果值是-1，表示水平翻转
`y` |`Number` | Canvas坐标系垂直缩放的比例。支持小数，如果值是-1，表示垂直翻转

```js
// 显示绘制个正方形用来对比
context.fillRect(10, 10, 10, 10);
// 缩放
context.scale(10, 3);
// 再次绘制
context.fillRect(10, 10, 10, 10);
// 恢复坐标系
context.setTransform(1, 0, 0, 1, 0, 0);
```

![](./__assets__/scale-2022-03-29-17-40-58.png)
