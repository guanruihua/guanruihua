# 十二面缓冲几何体（DodecahedronGeometry）

一个用于创建十二面几何体的类。

<iframe id="scene" src="https://threejs.org/docs/scenes/geometry-browser.html#DodecahedronGeometry" style="width: 880px; height: 420px; border: 0px; color: rgb(187, 187, 187); font-family: Inter, sans-serif; font-size: 18px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"></iframe>



## 构造器

### DodecahedronGeometry(radius : Float, detail : Integer)

radius — 十二面体的半径，默认值为1。
detail — 默认值为0。将这个值设为一个大于0的数将会为它增加一些顶点，使其不再是一个十二面体。

## 属性

共有属性请参见其基类PolyhedronGeometry。

### .parameters : Object

一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

## 方法(Methods)

共有方法请参见其基类PolyhedronGeometry。

## 源代码

[src/geometries/DodecahedronGeometry.js](https://github.com/mrdoob/three.js/blob/master/src/geometries/DodecahedronGeometry.js)