---
title: transition
date: 2021-05-07 11:38:27
---

# transition
> vscode可以支持动图  
> transition-property、transition-duration、transition-timing-function,transition-delay
> `transition-duration`: 为必填项
简写
```css
transition: all 5s linear .2s; // 以空格隔开属性
transition: all 5s linear .2s, height 3s ease-in-out; // 可以以逗号隔开多个过渡。
```

<div>
	<style>	
	.test1{
		width: 300px;
		height: 100px;
		background: green;
		transition: all 2s linear .2s;
	}
	.test1:hover{
		width: 250px;
		height: 50px;
		background: #ff85c0;
	}
	</style>
	<div class="test1"></div>
</div>


## transition-property
> 可以选择对某一个属性进行添加特效
```css
transition-property: all; // 默认为all，所有可被动画的属性都表现出过渡动画。
transition-property: none; // 没有动画效果
transition-property: width, height; //也可以取其他属性的值
```

### all

<div>
	<style>	
	.test2{
		width: 300px;
		height: 100px;
		background: green;
		transition: all 2s linear .2s;
	}
	.test2:hover{
		width: 250px;	
		height: 50px;
		background: #ff85c0;
	}
	</style>
	<div class="test2"></div>
</div>

### none

<div>
	<style>	
	.test3{
		width: 300px;
		height: 100px;
		background: green;
		transition: none 2s linear .2s;
	}
	.test3:hover{
		width: 250px;	
		height: 50px;
		background: #ff85c0;
	}
	</style>
	<div class="test3"></div>
</div>

### 自选
> 这里选的是 `width`

<div>
	<style>	
	.test4{
		width: 300px;
		height: 100px;
		background: green;
		transition: width 2s linear .2s;
	}
	.test4:hover{
		width: 250px;	
		height: 50px;
		background: #ff85c0;
	}
	</style>
	<div class="test4"></div>
</div>

## transition-duration
> 动画时间
```css
transition-duration: 1s;
transition-duration: 1ms;
transition-duration: 1s, 10s, 10ms; // 对应多个动画的属性
```

### 1s
<div>
	<style>	
	.test5{
		width: 300px;
		height: 100px;
		background: green;
		transition: all 1s linear .2s;
	}
	.test5:hover{
		width: 250px;	
		height: 50px;
		background: #ff85c0;
	}
	</style>
	<div class="test5"></div>
</div>

### 1ms

<div>
	<style>	
	.test7{
		width: 300px;
		height: 100px;
		background: green;
		transition: all 1ms linear .2s;
	}
	.test7:hover{
		width: 250px;
		heigth: 50px;
		background: #ff85c0;
	}
	</style>
	<div class="test7"></div>
</div>

### 1s, 3s, 10ms

<div>
	<style>	
	.test6{
		width: 300px;
		height: 100px;
		background: green;
		margin-left:0;
		/* transition: all 1ms linear .2s; */
		transition-property:width,margin-left,background;
		transition-duration: 1s, 3s, 10ms;
		transition-timing-function: linear;
		transition-delay: .2s;
	}
	.test6:hover{
		width: 250px;
		heigth: 50px;
		margin-left: 10px;
		background: #ff85c0;
	}
	</style>
	<div class="test6"></div>
</div>

## transition-timing-function

> 过渡动画

<div>
<style>
	:root{
		--w-before: 100px;
		--w-after: 50px;
		--h-before: 100px;
		--h-after: 50px;
		--bg-before: #20810f;
		--bg-after: #ff85c0;
	}
	table{ width:100%; }
	.t-center{ text-align:center;}
	.ttf{
		width: var(--w-before);
		height: var(--h-before);
		background: var(--bg-before);
		/* transition: all 2s linear .2s; */
		transition-property:width,height,background;
		transition-duration: 1s, 3s, 10ms;
		transition-timing-function: var(--ttf);
		transition-delay: .2s;
		display:flex;
		justify-content:center;
		align-items:center;
	}
	.ttf:hover{
		width: var(--w-after);
		height: var(--h-after);
		background: var(--bg-after);
	}
</style>
<table border="1">
	<tr>
		<td>transition-timing-function</td>
		<td class="t-center">示例</td>
		<td class="t-center">描述</td>
	</tr>
	<tr>
		<td>ease</td>  
		<td><div class="ttf" style="--ttf:ease;">ease</div></td>
		<td>默认值，慢速开始，中间变快，慢速结束；相当于 cubic-bezier(0.25, 0.1, 0.25, 1)</td>
	</tr>
	<tr>
		<td>linear</td> 
		<td><div class="ttf" style="--ttf:linear;">linear</div></td>
		<td>匀速运动；相当于 cubic-bezier(0, 0, 1, 1)</td>
	</tr>
	<tr>
		<td>ease-in</td> 
		<td><div class="ttf" style="--ttf:ease-in;">ease-in</div></td>
		<td>慢速开始；相当于 cubic-bezier(0.42, 0, 1, 1)</td>
	</tr>
	<tr>
		<td>ease-out</td> 
		<td><div class="ttf" style="--ttf:ease-out;">ease-out</div></td>
		<td>慢速结束；相当于 cubic-bezier(0, 0, 0.58, 1)</td>
	</tr>
	<tr>
		<td>ease-in-out</td> 
		<td><div class="ttf" style="--ttf:ease-in-out;">ease-in-out</div></td>
		<td>慢速开始，慢速结束；相当于 cubic-bezier(0.42, 0, 0.58, 1)</td>
	</tr>
	<tr>
		<td>step-start</td> 
		<td><div class="ttf" style="--ttf:step-start;">step-start</div></td>
		<td></td>
	</tr>
	<tr>
		<td>step-end</td> 
		<td><div class="ttf" style="--ttf:step-end;">step-end</div></td>
		<td></td>
	</tr>
	<tr>
		<td>steps(4, end)</td> 
		<td><div class="ttf" style="--ttf:step(4,end);">step(4,end)</div></td>
		<td></td>
	</tr>
</table>
</div>


## transition-delay
> 延迟时间
```css
transition-delay: 1s;
```

<div>
	<style>	
	.test41{
		width: 300px;
		height: 100px;
		background: green;
		transition: all 2s linear 1s;
	}
	.test41:hover{
		width: 250px;
		height: 50px;
		background: #ff85c0;
	}
	</style>
	<div class="test41"></div>
</div>



## 过渡状态切换
```css
.app {
  width: 300px;
 transition: all 3s linear .2s;
}

.app:hover {
  width: 100;
  transition: all 1s linear .2s;
}
```