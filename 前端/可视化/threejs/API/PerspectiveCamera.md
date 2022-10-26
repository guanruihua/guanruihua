
# 透视相机（PerspectiveCamera）

这一摄像机使用[perspective projection](https://en.wikipedia.org/wiki/Perspective_(graphical))（透视投影）来进行投影。

这一投影模式被用来模拟人眼所看到的景象，它是3D场景的渲染中使用得最普遍的投影模式。

## 代码示例

```js
const camera = new THREE.PerspectiveCamera( 45, width / height, 1, 1000 ); 
scene.add( camera );
```

## 例子

[animation / skinning / blending](https://threejs.org/examples/#webgl_animation_skinning_blending)
[animation / skinning / blending](https://threejs.org/examples/#webgl_animation_skinning_morph)
[effects / stereo](https://threejs.org/examples/#webgl_effects_stereo)
[interactive / cubes](https://threejs.org/examples/#webgl_interactive_cubes)
[loader / collada / skinning](https://threejs.org/examples/#webgl_loader_collada_skinning)

## 构造器

### PerspectiveCamera( fov : Number, aspect : Number, near : Number, far : Number )

> - fov — 摄像机视锥体垂直视野角度
> - aspect — 摄像机视锥体长宽比
> - near — 摄像机视锥体近端面
> - far — 摄像机视锥体远端面

这些参数一起定义了摄像机的[viewing frustum](https://en.wikipedia.org/wiki/Viewing_frustum)（视锥体）。

## 属性

> 共有属性请参见其基类 Camera
> 请注意，在大多数属性发生改变之后，你将需要调用.updateProjectionMatrix来使得这些改变生效。

### .aspect : Float

> 摄像机视锥体的长宽比，通常是使用画布的宽/画布的高。默认值是**1**（正方形画布）。

### .far : Float

> 摄像机的远端面，默认值是**2000**。
>
> 该值必须大于near plane（摄像机视锥体近端面）的值。

### .filmGauge : Float

> 胶片尺寸，其默认值为35（毫米）。 这个参数不会影响摄像机的投影矩阵，除非.filmOffset被设置为了一个非零的值。

### .filmOffset : Float

> 水平偏离中心偏移量，和.filmGauge单位相同。默认值为**0**。

### .focus : Float

> 用于立体视觉和景深效果的物体的距离。 这个参数不会影响摄像机的投影矩阵，除非使用了StereoCamera。 默认值是**10**。

### .fov : Float

> 摄像机视锥体垂直视野角度，从视图的底部到顶部，以角度来表示。默认值是**50**。

### .near : Float

> 摄像机的近端面，默认值是**0.1**。
>
> 其有效值范围是0到当前摄像机far plane（远端面）的值之间。 请注意，和OrthographicCamera不同，**0**对于PerspectiveCamera的近端面来说*不是*一个有效值。

### .view : Object

> Frustum window specification or null. 这个值使用.setViewOffset方法来进行设置，使用.clearViewOffset方法来进行清除。

### .zoom : number

> 获取或者设置摄像机的缩放倍数，其默认值为**1**。

## 方法

> 共有方法请参见其基类Camera。

### .clearViewOffset () : undefined

> 清除任何由.setViewOffset设置的偏移量。

### .getEffectiveFOV () : Float

> 结合.zoom（缩放倍数），以角度返回当前垂直视野角度。

### .getFilmHeight () : Float

> 返回当前胶片上图像的高，如果.aspect小于或等于1（肖像格式、纵向构图），则结果等于.filmGauge。

### .getFilmWidth () : Float

> 返回当前胶片上图像的宽，如果.aspect大于或等于1（景观格式、横向构图），则结果等于.filmGauge。

### .getFocalLength () : Float

> 返回当前.fov（视野角度）相对于.filmGauge（胶片尺寸）的焦距。

### .setFocalLength ( focalLength : Float ) : undefined

> 通过相对于当前.filmGauge的焦距，设置FOV。
>
> 默认情况下，焦距是为35mm（全画幅）摄像机而指定的。

### .setViewOffset ( fullWidth : Float, fullHeight : Float, x : Float, y : Float, width : Float, height : Float ) : undefined

fullWidth — 多视图的全宽设置
fullHeight — 多视图的全高设置
x — 副摄像机的水平偏移
y — 副摄像机的垂直偏移
width — 副摄像机的宽度
height — 副摄像机的高度

在较大的viewing frustum（视锥体）中设置偏移量，对于多窗口或者多显示器的设置是很有用的。

例如，如果你有一个3x2的显示器阵列，每个显示器分辨率都是1920x1080，且这些显示器排列成像这样的网格：

```txt
+---+---+---+
| A | B | C |
+---+---+---+
| D | E | F |
+---+---+---+
```

那对于每个显示器，你可以这样来设置、调用：

```js
const w = 1920; 
const h = 1080; 
const fullWidth = w * 3; 
const fullHeight = h * 2; 
// A 
camera.setViewOffset( fullWidth, fullHeight, w * 0, h * 0, w, h ); 
// B 
camera.setViewOffset( fullWidth, fullHeight, w * 1, h * 0, w, h ); 
// C 
camera.setViewOffset( fullWidth, fullHeight, w * 2, h * 0, w, h ); 
// D 
camera.setViewOffset( fullWidth, fullHeight, w * 0, h * 1, w, h ); 
// E 
camera.setViewOffset( fullWidth, fullHeight, w * 1, h * 1, w, h ); 
// F 
camera.setViewOffset( fullWidth, fullHeight, w * 2, h * 1, w, h );
```

请注意，显示器的不必具有相同的大小，或者不必在网格中。

### .updateProjectionMatrix () : undefined

更新摄像机投影矩阵。在任何参数被改变以后必须被调用。

### .toJSON (meta : Object) : Object

meta -- 包含有元数据的对象，例如对象后代中的纹理或图像
将摄像机转换为 three.js [JSON Object/Scene format](https://github.com/mrdoob/three.js/wiki/JSON-Object-Scene-format-4)（three.js JSON 物体/场景格式）。

## 源代码

[src/cameras/PerspectiveCamera.js](https://github.com/mrdoob/three.js/blob/master/src/cameras/PerspectiveCamera.js)
