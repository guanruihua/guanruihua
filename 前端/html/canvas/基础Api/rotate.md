# rotate

## 旋转

`context.rotate(angle)`

```js
// 旋转45度
context.rotate(45 * Math.PI / 180);
// 字体填充
context.font = '20px STHeiti, SimHei';
context.fillText('旋转，跳跃，我闭着眼', 60, -40, 188);
// 重置当前的变换矩阵为初始态
context.setTransform(1, 0, 0, 1, 0, 0);
```

![](./__assets__/rotate-2022-03-29-17-36-28.png)
