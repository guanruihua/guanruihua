# overflow

## overflow:clip

> 视觉上和`overflow: hidden`一样
> 设置 overflow: hidden 就会创建 BFC，自然没法只限制一个方向；而 clip 不会创建 BFC，这是本质原因。同时，BFC 还会带来的一些小区别这里也会有体现，比如 float 是否能限制在元素内，relative 定位的基准啥的
