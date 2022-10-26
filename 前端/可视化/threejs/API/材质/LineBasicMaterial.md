

# 基础线条材质（LineBasicMaterial）

一种用于绘制线框样式几何体的材质。

## 代码示例

```js
const material = new THREE.LineBasicMaterial( {
  color: 0xffffff,
  linewidth: 1,
  linecap: 'round', //ignored by WebGLRenderer 
	linejoin:  'round' //ignored by WebGLRenderer 
} );
```

## 例子

[WebGL / buffergeometry / drawrange](https://threejs.org/examples/#webgl_buffergeometry_drawrange)
[WebGL / buffergeometry / lines](https://threejs.org/examples/#webgl_buffergeometry_lines)
[WebGL / buffergeometry / lines / indexed](https://threejs.org/examples/#webgl_buffergeometry_lines_indexed)
[WebGL / decals](https://threejs.org/examples/#webgl_decals)
[WebGL / geometry / nurbs](https://threejs.org/examples/#webgl_geometry_nurbs)
[WebGL / geometry / shapes](https://threejs.org/examples/#webgl_geometry_shapes)
[WebGL / geometry / spline / editor](https://threejs.org/examples/#webgl_geometry_spline_editor)
[WebGL / interactive / buffergeometry](https://threejs.org/examples/#webgl_interactive_buffergeometry)
[WebGL / interactive / voxelpainter](https://threejs.org/examples/#webgl_interactive_voxelpainter)
[WebGL / lines / colors](https://threejs.org/examples/#webgl_lines_colors)
[WebGL / lines / dashed](https://threejs.org/examples/#webgl_lines_dashed)
[WebGL / lines / sphere](https://threejs.org/examples/#webgl_lines_sphere)
[WebGL / materials](https://threejs.org/examples/#webgl_materials)
[physics / ammo / rope](https://threejs.org/examples/#physics_ammo_rope)

## 构造函数(Constructor)

### LineBasicMaterial( parameters : Object )

parameters - (可选)用于定义材质外观的对象，具有一个或多个属性。材质的任何属性都可以从此处传入(包括从Material继承的任何属性)。

属性color例外，其可以作为十六进制字符串传递，默认情况下为 **0xffffff**（白色），内部调用Color.set(color)。

## 属性(Properties)

共有属性请参见其基类Material。

### .color : Color

材质的颜色(Color)，默认值为白色 (0xffffff)。

### .linewidth : Float

控制线宽。默认值为 **1**。

由于[OpenGL Core Profile](https://www.khronos.org/registry/OpenGL/specs/gl/glspec46.core.pdf)与 大多数平台上WebGL渲染器的限制，无论如何设置该值，线宽始终为1。

### .linecap : String

定义线两端的样式。可选值为 'butt', 'round' 和 'square'。默认值为 'round'。

该属性对应[2D Canvas lineCap](https://developer.mozilla.org/en/docs/Web/API/CanvasRenderingContext2D/lineCap)属性， 并且会被WebGL渲染器忽略。

### .linejoin : String

定义线连接节点的样式。可选值为 'round', 'bevel' 和 'miter'。默认值为 'round'。

该属性对应[2D Canvas lineJoin](https://developer.mozilla.org/en/docs/Web/API/CanvasRenderingContext2D/lineJoin)属性， 并且会被WebGL渲染器忽略。

## 方法(Methods)

共有方法请参见其基类Material。

## 源码(Source)

[src/materials/LineBasicMaterial.js](https://github.com/mrdoob/three.js/blob/master/src/materials/LineBasicMaterial.js)

