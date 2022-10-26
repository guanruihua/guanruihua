# WebGLRenderer

WebGL Render 用[WebGL](https://en.wikipedia.org/wiki/WebGL)渲染出你精心制作的场景。

## 构造器

### WebGLRenderer( parameters : Object )

parameters - (可选) 该对象的属性定义了渲染器的行为。也可以完全不传参数。在所有情况下，当缺少参数时，它将采用合理的默认值。 以下是合法参数：

> - canvas - 一个供渲染器绘制其输出的[canvas](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas) 它和下面的domElement属性对应。 如果没有传这个参数，会创建一个新canvas
> - context - 可用于将渲染器附加到已有的渲染环境([RenderingContext](https://developer.mozilla.org/en-US/docs/Web/API/WebGLRenderingContext))中。默认值是null
> - precision - 着色器精度. 可以是 **"highp"**, **"mediump"** 或者 **"lowp"**. 如果设备支持，默认为**"highp"** . 点击[here](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API/WebGL_best_practices) 查看"应该避免的事"
> - alpha - canvas是否包含alpha (透明度)。默认为 **false**
> - premultipliedAlpha - renderer是否假设颜色有 [premultiplied alpha](https://en.wikipedia.org/wiki/Glossary_of_computer_graphics#Premultiplied_alpha). 默认为**true**
> - antialias - 是否执行抗锯齿。默认为**false**.
> - stencil - 绘图缓存是否有一个至少8位的模板缓存([stencil buffer](https://en.wikipedia.org/wiki/Stencil_buffer))。默认为**true**
> - preserveDrawingBuffer -是否保留缓直到手动清除或被覆盖。 默认**false**.
> - powerPreference - 提示用户代理怎样的配置更适用于当前WebGL环境。 可能是**"high-performance"**, **"low-power"** 或 **"default"**。默认是**"default"**. 详见[WebGL spec](https://www.khronos.org/registry/webgl/specs/latest/1.0/#5.14.12)
> - failIfMajorPerformanceCaveat - whether the renderer creation will fail upon low perfomance is detected. Default is **false**. See [WebGL spec](https://www.khronos.org/registry/webgl/specs/latest/1.0/#5.14.12) for details.
> - depth - 绘图缓存是否有一个至少6位的深度缓存([depth buffer](https://en.wikipedia.org/wiki/Z-buffering) )。 默认是**true**.
> - logarithmicDepthBuffer - 是否使用对数深度缓存。如果要在单个场景中处理巨大的比例差异，就有必要使用。 Note that this setting uses gl_FragDepth if available which disables the [Early Fragment Test](https://www.khronos.org/opengl/wiki/Early_Fragment_Test) optimization and can cause a decrease in performance. 默认是**false**。 示例：camera / logarithmicdepthbuffer

## 属性

### .autoClear : Boolean

> 定义渲染器是否在渲染每一帧之前自动清除其输出。

### .autoClearColor : Boolean

> 如果autoClear为true, 定义renderer是否清除颜色缓存。 默认是**true**

### .autoClearDepth : Boolean

> 如果autoClear是true, 定义renderer是否清除深度缓存。 默认是**true**

### .autoClearStencil : Boolean

> 如果autoClear是true, 定义renderer是否清除模板缓存. 默认是**true**

### .debug.checkShaderErrors : Boolean

> 如果checkShaderErrors为true，定义是否检查材质着色器程序 编译和链接过程中的错误。 禁用此检查生产以获得性能增益可能很有用。 强烈建议在开发期间保持启用这些检查。 如果着色器没有编译和链接 - 它将无法工作，并且相关材料将不会呈现。 默认是**true**

### .capabilities : Object

> 一个包含当前渲染环境([RenderingContext](https://developer.mozilla.org/en-US/docs/Web/API/WebGLRenderingContext))的功能细节的对象。
>
> - floatFragmentTextures: 环境是否支持[OES_texture_float](https://developer.mozilla.org/en-US/docs/Web/API/OES_texture_float)扩展
> - floatVertexTextures: 如果floatFragmentTextures和vertexTextures都是true， 则此值为**true**
> - getMaxAnisotropy(): 返回最大可用各向异性。
> - getMaxPrecision(): 返回顶点着色器和片元着色器的最大可用精度。
> - isWebGL2: **true** if the context in use is a WebGL2RenderingContext object.
> - logarithmicDepthBuffer: 如果logarithmicDepthBuffer在构造器中被设为true且 环境支持[EXT_frag_depth](https://developer.mozilla.org/en-US/docs/Web/API/EXT_frag_depth)扩展，则此值为**true**
> - maxAttributes: **gl.MAX_VERTEX_ATTRIBS**的值
> - maxCubemapSize: **gl.MAX_CUBE_MAP_TEXTURE_SIZE** 的值，着色器可使用的立方体贴图纹理的最大宽度*高度
> - maxFragmentUniforms: **gl.MAX_FRAGMENT_UNIFORM_VECTORS**的值，片元着色器可使用的全局变量(uniforms)数量
> - maxTextureSize: **gl.MAX_TEXTURE_SIZE**的值，着色器可使用纹理的最大宽度*高度
> - maxTextures: *gl.MAX_TEXTURE_IMAGE_UNITS的值，着色器可使用的纹理数量
> - maxVaryings: **gl.MAX_VARYING_VECTORS**的值，着色器可使用矢量的数量
> - maxVertexTextures: **gl.MAX_VERTEX_TEXTURE_IMAGE_UNITS**的值，顶点着色器可使用的纹理数量。
> - maxVertexUniforms: **gl.MAX_VERTEX_UNIFORM_VECTORS**的值，顶点着色器可使用的全局变量(uniforms)数量
> - precision: 渲染器当前使用的着色器的精度
> - vertexTextures: 如果 .maxVertexTextures : Integer大于0，此值为**true** (即可以使用顶点纹理)

### .clippingPlanes : Array

> 用户自定义的剪裁平面，在世界空间中被指定为THREE.Plane对象。 这些平面全局使用。空间中与该平面点积为负的点将被切掉。 默认值是`[]`

### .domElement : DOMElement

> 一个[canvas](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/canvas)，渲染器在其上绘制输出。
渲染器的构造函数会自动创建(如果没有传入canvas参数);你需要做的仅仅是像下面这样将它加页面里去:
`document.body.appendChild( renderer.domElement );`

### .extensions : Object

.extensions.get方法的包装, 用于检查是否支持各种WebGL扩展

### .outputEncoding : number

定义渲染器的输出编码。默认为THREE.LinearEncoding

如果渲染目标已经使用 .setRenderTarget、之后将直接使用renderTarget.texture.encoding

查看texture constants页面以获取其他格式细节

### .info : Object

一个对象，包含有关图形板内存和渲染过程的一系列统计信息。这些信息可用于调试或仅仅满足下好奇心。改对象包含以下字段:

- memory:
  - geometries
  - textures
- render:
  - calls
  - triangles
  - points
  - lines
  - frame
- programs

By default these data are reset at each render call but when having multiple render passes per frame (e.g. when using post processing) it can be preferred to reset with a custom pattern. First, set **autoReset** to **false**.`renderer.info.autoReset = false;`Call **reset()** whenever you have finished to render a single frame.`renderer.info.reset();`

### .localClippingEnabled : Boolean

定义渲染器是否考虑对象级剪切平面。 默认为**false**.

### .physicallyCorrectLights : Boolean

是否使用物理上正确的光照模式。 默认是**false**。 示例：[lights / physical](https://threejs.org/examples/#webgl_lights_physical)

### .properties : Object

渲染器内部使用，以跟踪各种子对象属性。

### .renderLists : WebGLRenderLists

在内部用于处理场景渲染对象的排序。

### .shadowMap : WebGLShadowMap

如果使用，它包含阴影贴图的引用。

### .shadowMap.enabled : Boolean

如果设置开启，允许在场景中使用阴影贴图。默认是 **false**。

### .shadowMap.autoUpdate : Boolean

启用场景中的阴影自动更新。默认是**true**

如果不需要动态光照/阴影, 则可以在实例化渲染器时将之设为false

### .shadowMap.needsUpdate : Boolean

当被设为**true**, 场景中的阴影贴图会在下次**render**调用时刷新。默认是**false**

如果你已经禁用了阴影贴图的自动更新(**shadowMap.autoUpdate = false**), 那么想要在下一次渲染时更新阴影的话就需要将此值设为**true**

### .shadowMap.type : Integer

定义阴影贴图类型 (未过滤, 关闭部分过滤, 关闭部分双线性过滤)

可选值有THREE.BasicShadowMap, THREE.PCFShadowMap (默认), THREE.PCFSoftShadowMap 和 THREE.VSMShadowMap。详见Renderer constants

### .sortObjects : Boolean

定义渲染器是否应对对象进行排序。默认是**true**.

说明: 排序用于尝试正确渲染出具有一定透明度的对象。根据定义，排序可能不总是有用。根据应用的需求，可能需要关闭排序并使其他方法来处理透明度的渲染，例如， 手动确定每个对象的显然顺序。

### .state : Object

包含设置WebGLRenderer.context状态的各种属性的函数。

### .toneMapping : Constant

默认是NoToneMapping。查看Renderer constants以获取其它备选项

### .toneMappingExposure : Number

色调映射的曝光级别。默认是**1**

### .xr : WebXRManager

Provides access to the WebXR related interface of the renderer.

## 方法

### .clear ( color : Boolean, depth : Boolean, stencil : Boolean ) : undefined

告诉渲染器清除颜色、深度或模板缓存. 此方法将颜色缓存初始化为当前颜色。参数们默认都是**true**

### .clearColor ( ) : undefined

清除颜色缓存。 相当于调用.clear( true, false, false )

### .clearDepth ( ) : undefined

清除深度缓存。相当于调用.clear( false, true, false )

### .clearStencil ( ) : undefined

清除模板缓存。相当于调用.clear( false, false, true )

### .clearTarget (renderTarget : WebGLRenderTarget, color : Boolean, depth : Boolean, stencil : Boolean) : undefined

renderTarget -- 需要被清除的renderTarget
color -- 如果设置, 颜色会被清除
depth -- 如果设置, 深度缓存会被清除
stencil -- 如果设置, 模板缓存会被清除

该方法清楚了一个rendertarget。为此它会激活此endertarget

### .compile ( scene : Object3D, camera : Camera ) : undefined

使用相机编译场景中的所有材质。这对于在首次渲染之前预编译着色器很有用。

### .copyFramebufferToTexture ( position : Vector2, texture : FramebufferTexture, level : Number ) : undefined

将当前WebGLFramebuffer中的像素复制到2D纹理中。可访问[WebGLRenderingContext.copyTexImage2D](https://developer.mozilla.org/en-US/docs/Web/API/WebGLRenderingContext/copyTexImage2D).

### .copyTextureToTexture ( position : Vector2, srcTexture : Texture, dstTexture : Texture, level : Number ) : undefined

将纹理的所有像素复制到一个已有的从给定位置开始的纹理中。可访问[WebGLRenderingContext.texSubImage2D](https://developer.mozilla.org/en-US/docs/Web/API/WebGLRenderingContext/texSubImage2D).

### .dispose ( ) : undefined

处理当前的渲染环境

### .extensions.get ( extensionName : String ) : Object

用于检查是否支持各种扩展，并返回一个对象，其中包含扩展的详细信息。 该方法检查以下扩展：

\- **WEBGL_depth_texture**
\- **EXT_texture_filter_anisotropic**
\- **WEBGL_compressed_texture_s3tc**
\- **WEBGL_compressed_texture_pvrtc**
\- **WEBGL_compressed_texture_etc1**

### .forceContextLoss () : undefined

模拟WebGL环境的丢失。需要支持 [WEBGL_lose_context](https://developer.mozilla.org/en-US/docs/Web/API/WEBGL_lose_context) 扩展才能用。

### .forceContextRestore ( ) : undefined

模拟WebGL环境的恢复。需要支持 [WEBGL_lose_context](https://developer.mozilla.org/en-US/docs/Web/API/WEBGL_lose_context) 扩展才能用。

### .getClearAlpha () : Float

返回一个表示当前alpha值的float，范围0到1

### .getClearColor ( target : Color ) : Color

返回一个表示当前颜色值的THREE.Color实例

### .getContext () : WebGLRenderingContext

返回当前WebGL环境

### .getContextAttributes () : WebGLContextAttributes

返回一个对象，这个对象中存有在WebGL环境在创建的时候所设置的属性

### .getActiveCubeFace () : Integer

Returns the current active cube face.

### .getActiveMipmapLevel () : Integer

Returns the current active mipmap level.

### .getRenderTarget () : RenderTarget

如果当前存在RenderTarget，则返回该值；否则返回**null**。

### .getCurrentViewport () : RenderTarget

返回当前视口

### .getDrawingBufferSize () : Object

返回一个包含渲染器绘图缓存宽度和高度(单位像素)的对象。

### .getPixelRatio () : number

返回当前使用设备像素比

### .getSize () : Object

返回包含渲染器输出canvas的宽度和高度(单位像素)的对象。

### .initTexture ( texture : Texture ) : undefined

Initializes the given texture. Useful for preloading a texture rather than waiting until first render (which can cause noticeable lags due to decode and GPU upload overhead).

### .resetGLState ( ) : undefined

将GL状态重置为默认值。WebGL环境丢失时会内部调用

### .readRenderTargetPixels ( renderTarget : WebGLRenderTarget, x : Float, y : Float, width : Float, height : Float, buffer : TypedArray, activeCubeFaceIndex : Integer ) : undefined

buffer - Uint8Array is the only destination type supported in all cases, other types are renderTarget and platform dependent. See [WebGL spec](https://www.khronos.org/registry/webgl/specs/latest/1.0/#5.14.12) for details.

将enderTarget中的像素数据读取到传入的缓冲区中。这是[WebGLRenderingContext.readPixels](https://developer.mozilla.org/en-US/docs/Web/API/WebGLRenderingContext/readPixels)()的包装器
示例：[interactive / cubes / gpu](https://threejs.org/examples/#webgl_interactive_cubes_gpu)

For reading out a WebGLCubeRenderTarget use the optional parameter activeCubeFaceIndex to determine which face should be read.

### .render ( scene : Object3D, camera : Camera ) : undefined

用相机(camera)渲染一个场景(scene)或是其它类型的object。
渲染一般是在canvas上完成的，或者是renderTarget(如果有指定)
如果forceClear值是**true**，那么颜色、深度及模板缓存将会在渲染之前清除，即使渲染器的autoClear属性值是false
即便forceClear设为true, 也可以通过将autoClearColor、autoClearStencil或autoClearDepth属性的值设为false来阻止对应缓存被清除。

### .resetState () : undefined

Can be used to reset the internal WebGL state. This method is mostly relevant for applications which share a single WebGL context across multiple WebGL libraries.

### .setAnimationLoop ( callback : Function ) : undefined

callback — 每个可用帧都会调用的函数。 如果传入‘null’,所有正在进行的动画都会停止。

可用来代替[requestAnimationFrame](https://developer.mozilla.org/en-US/docs/Web/API/window/requestAnimationFrame)的内置函数. 对于WebXR项目，必须使用此函数。

### .setClearAlpha ( alpha : Float ) : undefined

设置alpha。合法参数是一个**0.0**到 **1.0**之间的浮点数

### .setClearColor ( color : Color, alpha : Float ) : undefined

设置颜色及其透明度

### .setPixelRatio ( value : number ) : undefined

设置设备像素比。通常用于避免HiDPI设备上绘图模糊

### .setRenderTarget ( renderTarget : WebGLRenderTarget, activeCubeFace : Integer, activeMipmapLevel : Integer ) : undefined

renderTarget -- 需要被激活的renderTarget(可选)。若此参数为空，则将canvas设置成活跃render target。
activeCubeFace -- Specifies the active cube side (PX 0, NX 1, PY 2, NY 3, PZ 4, NZ 5) of WebGLCubeRenderTarget (optional).
activeMipmapLevel -- Specifies the active mipmap level (optional).

该方法设置活跃rendertarget。

### .setScissor ( x : Integer, y : Integer, width : Integer, height : Integer ) : undefined

将剪裁区域设为(x, y)到(x + width, y + height) Sets the scissor area from

### .setScissorTest ( boolean : Boolean ) : undefined

启用或禁用剪裁检测. 若启用，则只有在所定义的裁剪区域内的像素才会受之后的渲染器影响。

### .setSize ( width : Integer, height : Integer, updateStyle : Boolean ) : undefined

将输出canvas的大小调整为(width, height)并考虑设备像素比，且将视口从(0, 0)开始调整到适合大小 将updateStyle设置为false以阻止对canvas的样式做任何改变。

### .setViewport ( x : Integer, y : Integer, width : Integer, height : Integer ) : undefined

将视口大小设置为(x, y)到 (x + width, y + height).

## 源码

[src/renderers/WebGLRenderer.js](https://github.com/mrdoob/three.js/blob/master/src/renderers/WebGLRenderer.js)
