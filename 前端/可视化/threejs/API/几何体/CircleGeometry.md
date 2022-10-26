
# 圆形缓冲几何体（CircleGeometry）

CircleGeometry是欧式几何的一个简单形状，它由围绕着一个中心点的三角分段的数量所构造，由给定的半径来延展。 同时它也可以用于创建规则多边形，其分段数量取决于该规则多边形的边数。

<iframe id="scene" src="https://threejs.org/docs/scenes/geometry-browser.html#CircleGeometry" style="width: 880px; height: 420px; border: 0px; color: rgb(187, 187, 187); font-family: Inter, sans-serif; font-size: 18px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"></iframe>

## 代码示例

```js
const geometry = new THREE.CircleGeometry( 5, 32 ); 
const material = new THREE.MeshBasicMaterial( { color: 0xffff00 } ); 
const circle = new THREE.Mesh( geometry, material ); scene.add( circle );
```

## 构造器

### CircleGeometry(radius : Float, segments : Integer, thetaStart : Float, thetaLength : Float)

radius — 圆形的半径，默认值为1
segments — 分段（三角面）的数量，最小值为3，默认值为8。
thetaStart — 第一个分段的起始角度，默认为0。（three o'clock position）
thetaLength — 圆形扇区的中心角，通常被称为“θ”（西塔）。默认值是2*Pi，这使其成为一个完整的圆。

## 属性

共有属性请参见其基类BufferGeometry。

### .parameters : Object

一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

## 方法(Methods)

共有方法请参见其基类BufferGeometry。

## 源代码

[src/geometries/CircleGeometry.js](https://github.com/mrdoob/three.js/blob/master/src/geometries/CircleGeometry.js)
