# CSS 3D渲染器（CSS3DRenderer）

CSS3DRenderer用于通过CSS3的[transform](https://www.w3schools.com/cssref/css3_pr_transform.asp)属性， 将层级的3D变换应用到DOM元素上。 如果你希望不借助基于canvas的渲染来在你的网站上应用3D变换，那么这一渲染器十分有趣。 同时，它也可以将DOM元素与WebGL的内容相结合。

然而，这一渲染器也有一些十分重要的限制：

- 它不可能使用**three.js**中的材质系统。
- 同时也不可能使用几何体。

因此，CSS3DRenderer仅仅关注普通的DOM元素，这些元素被包含到了特殊的对象中（**CSS3DObject**或者**CSS3DSprite**），然后被加入到场景图中。



## 例子

[molecules](https://threejs.org/examples/#css3d_molecules)
[orthographic camera](https://threejs.org/examples/#css3d_orthographic)
[periodictable](https://threejs.org/examples/#css3d_periodictable)
[sprites](https://threejs.org/examples/#css3d_sprites)

## 构造函数

### CSS3DRenderer()

## 方法

### .getSize () : Object

返回一个包含有渲染器宽和高的对象。

### .render ( scene : Scene, camera : PerspectiveCamera ) : undefined

使用perspective camera渲染scene。

### .setSize (width : Number, height : Number) : undefined

将渲染器尺寸重新调整为(width, height)。

## 源代码

[examples/jsm/renderers/CSS3DRenderer.js](https://github.com/mrdoob/three.js/blob/master/examples/jsm/renderers/CSS3DRenderer.js)