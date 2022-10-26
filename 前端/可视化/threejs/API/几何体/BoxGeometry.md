# 立方缓冲几何体（BoxGeometry）

BoxGeometry是四边形的原始几何类，它通常使用构造函数所提供的“width”、“height”、“depth”参数来创建立方体或者不规则四边形。

<iframe id="scene" src="https://threejs.org/docs/scenes/geometry-browser.html#BoxGeometry" style="width: 880px; height: 420px; border: 0px; color: rgb(187, 187, 187); font-family: Inter, sans-serif; font-size: 18px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"></iframe>

## 代码示例

```js
const geometry = new THREE.BoxGeometry( 1, 1, 1 ); 
const material = new THREE.MeshBasicMaterial( {color: 0x00ff00} ); 
const cube = new THREE.Mesh( geometry, material ); scene.add( cube );
```

## 构造器

### BoxGeometry(width : Float, height : Float, depth : Float, widthSegments : Integer, heightSegments : Integer, depthSegments : Integer)

> - width — X轴上面的宽度，默认值为1。
> - height — Y轴上面的高度，默认值为1。
> - depth — Z轴上面的深度，默认值为1。
> - widthSegments — （可选）宽度的分段数，默认值是1。
> - heightSegments — （可选）高度的分段数，默认值是1。
> - depthSegments — （可选）深度的分段数，默认值是1。

## 属性

共有属性请参见其基类BufferGeometry。

### .parameters : Object

一个包含着构造函数中每个参数的对象。在对象实例化之后，对该属性的任何修改都不会改变这个几何体。

使用上面的示例代码来作为基础：

```js
geometry.parameters; // outputs an object {width: 1, height: 1, depth: 1, widthSegments: undefined, heightSegments: undefined}  
cube.geometry.parameters; // as above  
cube.geometry.parameters.width; // === 1  
cube.geometry.parameters.widthSegments // === undefined.
```

## 方法(Methods)

共有方法请参见其基类BufferGeometry。

## 源代码

[src/geometries/BoxGeometry.js](https://github.com/mrdoob/three.js/blob/master/src/geometries/BoxGeometry.js)

![img](https://threejs.org/files/ic_mode_edit_black_24dp.svg)
