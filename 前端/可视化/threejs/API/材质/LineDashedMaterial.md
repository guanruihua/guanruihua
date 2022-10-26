

# 虚线材质(LineDashedMaterial)

一种用于绘制虚线样式几何体的材质。

## 代码示例

```js
const material = new THREE.LineDashedMaterial( { 
	color: 0xffffff,
	linewidth: 1, 
	scale: 1, 
	dashSize: 3, 
	gapSize: 1, 
} );
```

## 例子

[WebGL / lines / dashed](https://threejs.org/examples/#webgl_lines_dashed)

## 构造函数(Constructor)

### LineDashedMaterial( parameters : Object )

parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从LineBasicMaterial继承的任何属性)。

## 属性(Properties)

共有属性请参见其基类LineBasicMaterial。

### .dashSize : number

虚线的大小，是指破折号和间隙之和。默认值为 **3**。

### .gapSize : number

间隙的大小，默认值为 **1**。

### .scale : number

线条中虚线部分的占比。默认值为 **1**。

## 方法(Methods)

共有方法请参见其基类LineBasicMaterial。

## 源码(Source)

[src/materials/LineDashedMaterial.js](https://github.com/mrdoob/three.js/blob/master/src/materials/LineDashedMaterial.js)