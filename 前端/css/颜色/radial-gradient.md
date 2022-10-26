# radial-gradient

> 渐变色

```css
ackground-image: radial-gradient(shape size at position, start-color, ..., last-color);
```

| 值                             | 描述                                                         |
| :----------------------------- | :----------------------------------------------------------- |
| *shape*                        | 确定圆的类型:<br/>ellipse (默认): 指定椭圆形的径向渐变。<br/>circle ：指定圆形的径向渐变 |
| *size*                         | 定义渐变的大小，可能值：<br/>farthest-corner (默认) : 指定径向渐变的半径长度为从圆心到离圆心最远的角<br/>closest-side ：指定径向渐变的半径长度为从圆心到离圆心最近的边<br/>closest-corner ： 指定径向渐变的半径长度为从圆心到离圆心最近的角<br/>farthest-side ：指定径向渐变的半径长度为从圆心到离圆心最远的边 |
| *position*                     | 定义渐变的位置。可能值：<br/>**center**（默认）：设置中间为径向渐变圆心的纵坐标值。<br/>**top**：设置顶部为径向渐变圆心的纵坐标值。<br/>**bottom**：设置底部为径向渐变圆心的纵坐标值。 |
| *start-color, ..., last-color* | 用于指定渐变的起止颜色。                                     |
