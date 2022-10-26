---
title: transform
date: 2021-5-7 11:10:26
---

# transform

> `transform: translate(位移)|scale(缩放)| rotete(旋转)| skew(倾斜)`
> 不支持inline元素, 使用时候需要变成block  
> deg单位为角度单位

## translate

> 位移

```css
translate(x,y);
translateX(x);
translateY(y);
translateZ(z);/*需要在父容器上加上perspective元素 */
translate3d(x,y,z);/*需要在父容器上加上perspective元素 */
```

<style>
	:root{
		--w: 20px;
		--h: 20px;
		--bg: #20810f;
    --box-c: #ff85c0;
	}
	table{ width:100%; }
  .td-container{width:100px; height:50px;background:#fff;}
	.tf{
		width: var(--w);
		height: var(--h);
		background: var(--bg);
		display:flex;
		justify-content:center;
		align-items:center;
	}
</style>

<style>
 .tfx{ transform: translateX(var(--X)); }
  .tfy{ transform: translateY(var(--Y)); }
  .tfxy{ transform: translate(var(--X),var(--Y)); }
  .tfz{ transform: perspective(30px) translateZ(var(--Z)); }
	.tf3d{ transform: perspective(30px) translate3d(var(--X), var(--Y), var(--Z)); }	
</style>
<table border="1">
	<tr>
		<td>translate</td>
		<td>示例</td>
	</tr>
	<tr>
		<td>translateX(x);</td> <td>
    <div class="td-container">
      <div class="tf tfx" style="--X:30px;"></div>
    </td>
	</tr>
  <tr>
		<td>translateY(y);</td> <td>
    <div class="td-container">
      <div class="tf tfy" style="--Y:10px;"></div>
    </td>
	</tr>
  <tr>
		<td>translate(x,y);</td> <td>
    <div class="td-container">
      <div class="tf tfxy" style="--Y:10px;--X:10px;"></div>
    </td>
	</tr>
	<tr>
		<td>translateZ(z);<br/>需要和perspective配合使用(将观察者相对于z=0处的平面进行定位)<br/>里面的参数就相当于向前移动了多少像素</td> <td>
    <div class="td-container">
      <div class="tf tfz" style="--Z:10px;"></div>
    </td>
	</tr>
		<tr>
		<td>translate3d(z);<br/>需要和perspective配合使用(将观察者相对于z=0处的平面进行定位)<br/>里面的参数就相当于向前移动了多少像素</br>z轴大小要小于该参数</td> <td>
    <div class="td-container">
      <div class="tf tf3d" style="--Z:10px;--X:10px;--Y:10px;"></div>
    </td>
	</tr>
</table>


## scale
> 注意参数都是数字  
> 放大

```css
scaleX(x);
scaleY(y);
scaleZ(z);
scale(x, y);
scale3d();/* 待补充 */
scale(整数放大倍数);
```

<style>
	.scx{ transform: scaleX(var(--X))}
	.scy{ transform: scaleY(var(--Y))}
	.scz{ transform: scaleX(var(--Z))}
	.sc{ transform: scale(var(--X),var(--Y))}
	.scn{ transform: scale(var(--Num))}
	.sc3d{ transform: scale3d(var(--X),var(--Y),var(--Z))}
</style>
<table border="1">
	<tr>
		<td>scale</td>
		<td>示例</td>
	</tr>
	<tr>
		<td>scaleX(x);</td> <td>
    <div class="td-container">
      <div class="tf scx" style="--X:1.5;">1.5</div>
    </td>
	</tr>
  <tr>
		<td>scaleY(y);</td> <td>
    <div class="td-container">
      <div class="tf scy" style="--Y:1.5;">1.5</div>
    </td>
	</tr>
  <tr>
		<td>scale(x,y);</td> <td>
    <div class="td-container">
      <div class="tf sc" style="--Y:1.5;--X:1.5;">(1.5,1.5)</div>
    </td>
	</tr>
	<tr>
		<td>scaleZ(z);<td>
    <div class="td-container">
      <div class="tf scz" style="--Z:1.5;">1.5</div>
    </td>
	</tr>
		<tr>
		<td>scale3d(z);</td> <td>
    <div class="td-container">
      <div class="tf sc3d" style="--Z:2;--X:3;--Y:2;">(2,3,2)</div>
    </td>
	</tr>
		</tr>
		<tr>
		<td>scale(Num);(整体放大)</td> <td>
    <div class="td-container">
      <div class="tf scn" style="--Num:2">2</div>
    </td>
	</tr>
</table>


## rotate
> 旋转

```css
rotate([<angle>|<zero>])/* 顺时针旋转 */
rotateZ([<angle>|<zero>])/* 绕Z轴旋转 */
rotateX([<angle>|<zero>])/* 绕X轴旋转 */
rotateY([<angle>|<zero>])/* 绕Y轴旋转 */
rotate3d([number],[number],[number],[<angle>|<zero>])
```

<style>
	.rt{ transform: rotate(var(--angle))}
	.rtx{ transform:rotateX(var(--angle))}
	.rty{ transform: rotateY(var(--angle))}
	.rtz{ transform: rotateZ(var(--angle))}
	.rt3d{ transform: rotate3d(var(--X),var(--Y),var(--Z),var(--angle))}
</style>
<table border="1">
	<tr>
		<td>rotate</td>
		<td>示例</td>
	</tr>
	<tr>
		<td>rotateX(45deg);</td> <td>
    <div class="td-container">
      <div class="tf rtx" style="--angle:45deg;"></div>
    </td>
	</tr>
  <tr>
		<td>rotateY(45deg);</td> <td>
    <div class="td-container">
      <div class="tf rty" style="--angle:45deg"></div>
    </td>
	</tr>
  <tr>
		<td>rotate(45deg);</td> <td>
    <div class="td-container">
      <div class="tf rt" style="--angle:45deg"></div>
    </td>
	</tr>
	<tr>
		<td>rotateZ(z);<td>
    <div class="td-container">
      <div class="tf rtz" style="--angle:-33deg"></div>
    </td>
	</tr>
		<tr>
		<td>rotate3d(1,2,3,45deg);</td> <td>
    <div class="td-container">
      <div class="tf rt3d" style="--Z:2;--X:3;--Y:2;--angle:45deg;"></div>
    </td>
</table>


## skew

> 歪斜

```css
skewX([<angle>|<zero>])
skewY([<angle>|<zero>])
skew(([<angle>|<zero>],[<angle>|<zero>]? )
```

<style>
	.skew{ transform: skew(15deg,15deg)}
	.skewx{ transform: skewX(var(--angle))}
	.skewy{ transform: skewY(var(--angle))}
</style>
<table border="1">
	<tr>
		<td>skew</td>
		<td>示例</td>
	</tr>
	<tr>
		<td>skewX(45deg);</td> <td>
    <div class="td-container">
      <div class="tf skewx" style="--angle:45deg;"></div>
    </td>
	</tr>
  <tr>
		<td>skewY(45deg);</td> <td>
    <div class="td-container">
      <div class="tf skewy" style="--angle:45deg"></div>
    </td>
	</tr>
  <tr>
		<td>skew(15deg, 15dep);</td> <td>
    <div class="td-container">
      <div class="tf skew"></div>
    </td>
	</tr>
	<tr>
</table>

## matrix
> 矩阵变换
```css
matrix()
matrix(a, b, c, d, tx, ty) 
=> matrix3d(a, b, 0, 0, c, d, 0, 0, 0, 0, 1, 0, tx, ty, 0, 1) 
=> matrix( scaleX(), skewY(), skewX(), scaleY(), translateX(), translateY() )


matrix3d()
matrix3d(a1, b1, c1, d1, a2, b2, c2, d2, a3, b3, c3, d3, a4, b4, c4, d4)
```


<style>
	.matr3d{ transform: matrix3d(1,0,0,0, 0,1,0,0, 0,0,1,0,-5,-10,0,1.1)}
	.matr{ transform: matrix(1,2,-1,1,40,20)}
</style>
<table border="1">
	<tr>
		<td>skew</td>
		<td>示例</td>k
	</tr>
	<tr>
		<td>matrix3d(1,0,0,0, 0,1,0,0, 0,0,1,0,-5,-10,0,1.1);</td> <td>
    <div class="td-container">
      <div class="tf matr3d"></div>
    </td>
	</tr>
  <tr>
		<td>matrix(1, 2, -1, 1, 40, 20);</td> <td>
    <div class="td-container">
      <div class="tf matr"></div>
    </td>
	</tr>
</table>



## 视距 `prtspective`
> - perspective([length])  : 就是相当于定义人物（或观察者）＂离显示器中该元素的距离＂
> - <b style="color:red;"> 在3D空间中改变元素的形态时, 基本上都要赋予元素一定H的视域。</b>
> - 视域为元素赋予前后深度，而这深度可以根据需要设定。

​     

## 移动原点`transform-origin`
> - 修改元素动画的变形原点
> - 旋转中心默认中心
> - 其他变换中心是是不太确定的
> - 都可以通过该属性进行修改

```css
transform-origin: 50%,50%;
```

## 变换方式 `tansform-style`
> 在一个三维空间中改变元素的形态，使用translate3d()，或许希望在3D空间中呈现元素。  
> 一般这不是默认行为。默认情况下，不管怎么变形，得到的结果都是扁平的。但是可以使用transform-style修改。

| `transform-style` |                  |
| ----------------- | ---------------- |
| 取值              | `flat            |
| 初始值            | `flat`           |
| 适用于            | 任何可变形的元素 |
| 计算值            | 指定的值         |
| 继承性            | 否               |
| 动画性            | 否               |


### 原来

```html
<style>
.mbd{ border:1px solid red; }
.outer{
	display:flex;
	width:150px;
	height:125px;
	justify-content:center;
	align-items:center;
}
.inner { 
	width:50px;
	transform: perspective(750px) translateZ(60px) rotateX(60deg); 
	}
</style>

<div class="outer mbd">
    <div class="inner mbd">inner</div>
</div>
```

<style>
.mbd{ border:1px solid red; }
.outer{
	display:flex;
	width:150px;
	height:125px;
	justify-content:center;
	align-items:center;
}
.inner { 
	width:50px;
	transform: perspective(750px) translateZ(60px) rotateX(60deg); 
	}
</style>

<div class="outer mbd">
    <div class="inner mbd">inner</div>
</div>


### 后

```html
<div style="
		display:flex;
		width:150px;
		height:125px;
		justify-content:center;
		align-items:center;
		transform: perspective(750px) translateY(60px) rotateX(-20deg); 
		transform-style: perserve-3d;
		overflow: visible;
		filter: none;
		clip: auto;
		clip-path: none;
		mask-image: none;
		mask-border-source: none;
		mix-blend-mode: normal;
		border:1px solid red;">
    <div style="	
			width:50px;
			border:1px solid red;
			transform: perspective(750px) translateZ(60px) rotateX(60deg); 
			">inner</div>
</div>
```

<div style="
		display:flex;
		width:150px;
		height:125px;
		justify-content:center;
		align-items:center;
		transform: perspective(750px) translateY(60px) rotateX(-20deg); 
		transform-style: perserve-3d;
		overflow: visible;
		filter: none;
		clip: auto;
		clip-path: none;
		mask-image: none;
		mask-border-source: none;
		mix-blend-mode: normal;
		border:1px solid red;">
    <div style="	
			width:50px;
			border:1px solid red;
			transform: perspective(750px) translateZ(60px) rotateX(60deg); 
			">inner</div>
</div>

<br/>
<br/>
<br/>
<br/>
<br/>

## 处理背面 `backface-visibility`
> 在3D变形中，backface-visibility属性可以看到元素的背面。
> 一个元素的backface-visibility属性设置为visible，另一个设置为hidden。


<style>
.card {
  position: relative;
  perspective: 800px;
  transform-style: preserve-3d;
  width:200px;
  height:280px;
  transition: all .6s;
  border: 1px solid #000;
  border-radius: 4px;
}

.card:hover {
  transform: rotateY(180deg);
}
img {
  position: absolute;
  width:100%;
  height: 100%;
  transition: all .5s;
  backface-visibility: hidden;
}
/*由于我们将两个图像都藏在了背面，所以另一面没有任何东西。 所以接下来需要再把.front-face翻转180度*/
.back-face {
  transform: rotateY(180deg);
}
</style>

<div class="card">
    <img class="front-face" src="https://api.pixivweb.com/anime18r.php?return=img">
    <img class="back-face" src="https://api.pixivweb.com/bw.php?return=img">
</div>
