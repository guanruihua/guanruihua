# transform

> 对当前坐标进行进一步变换, 实现缩放, 旋转, 拉伸或位移效果
> 与`setTransform` 区别在于, `setTransform`会重置已有的变换, `transform`是累加

```js
context.transform(a, b, c, d, e, f);
```

参数| 类型 |作用
|:----|:----|:----|
`a`| `Number` |水平缩放
`b`| `Number` |水平斜切
`c`| `Number` |垂直斜切
`d`| `Number` |垂直缩放
`e`| `Number` |水平位移
`f`| `Number` |垂直位移
![](./__assets__/setTransform-2022-03-29-17-48-14.png)

```js
context.transform(1, 0, 1, 1, 0, 0);
context.fillRect(10, 20, 100, 100);
```

![](./__assets__/setTransform-2022-03-29-17-50-49.png)
