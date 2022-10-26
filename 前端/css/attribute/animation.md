---
title: animation
date: 2021-5-7 10:10:05
---


# animation
> 名称：关键帧动画，帧动画  
> `animation: 属性名 时长 过度方式 延迟 次数 方向 填充模式 是否暂停;`  
> `animation: name duration timing-function delay iteration-count direction fill-mode;`

| 属性   | 描述 |
| -- | -- |
| animation-name            | 指定 @keyframes 动画的`名称`。|
| animation-duration        | 指定动画完成一个`周期`所需要时间，单位秒（s）或毫秒（ms），默认是 |
| animation-timing-function | 指定动画`计时函数`，即动画的速度曲线，默认是 "ease"。|
| animation-delay           | 指定动画`延迟时间`，即动画何时开始，默认是 0。|
| animation-iteration-count | 指定动画`播放的次数`，默认是 1。|
| animation-direction       | 指定动画`播放的方向`。默认是 normal。|
| animation-fill-mode       | 指定动画`填充模式`。默认是 none。|
| animation-play-state      | 指定动画`播放状态`，正在运行或暂停。默认是 running。|

## animation-name
> 通过`@keyframs`来定义关键帧动画

```css
/* 使用 */
animation： [name] 2s ;

/* 等价于 */
animation-name: [name];
animation-duration: 2s;

/* 定义 */
@keyframes [name] {
	....
}
```
### @keyframes
> 主要两种写法

#### 百分率

```css
@keyframes [name] {
	0%{
    transform: rotate(0deg);
  }
  100%{
    transform: rotate(360deg);
  }
	

@keyframes [name]{
  0%, 66%{
    transform: translate(0,0);
  }
  33%,100%{
    transform: translate(200px,0);
  }
}
```

#### from & to

```css
@keyframes [name] {
	 from{
    transform: rotate(0deg);
  }
  to{
    transform: rotate(360deg);
  }
}
```

## animation-duration        
> 动画运行时间

## animation-timing-function
> 计时函数  
> 常见值： `linear`,`ease`,`ease-in`,`ease-out`,`ease-in-out` （都是`cubicbezier(n,n,n,n)`的特例）
## animation-delay           
## animation-iteration-count
> 动画播放次数  
> `infinite`: 无限播放

## animation-direction    
> 指定动画按照指定顺序播放  
> `normal`:默认值；  
> `reverse`: 动画反向播放；  
> `alternate`: 正向和反向交叉进行；  
> `alternate-reverse`: 表反向和正向交叉进行； 

## animation-fill-mode
> 填充模式
> none: 默认值；  
> forwards: 动画完成后保持最后一帧(停在最后一帧)  
> backwards: 有动画颜值时，动画开始前，元素状态保持第一帧的状态  
> both: 前两者均有；

## animation-play-state
> 表示动画播放状态  
> `running`: 播放
> `paused`: 暂停
