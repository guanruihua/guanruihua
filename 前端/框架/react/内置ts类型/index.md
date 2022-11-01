# React 内置类型

## React.ReactNode和React.Element

## ReactNode | undefined

```tsx
type ReactNode = ReactChild | ReactFragment | ReactPortal | boolean | null | undefined;
```

> `type ReactFragment = {} | ReactNodeArray;`:就是<>xxx</>或者`<React.Fragament>xxx<React.Fragament/>`

```tsx
interface ReactPortal extends ReactElement {
    key: Key | null;
    children: ReactNode;
}
```

> 就是ReactDOM.createPortal(child, container)这个API的返回值

```ts
export function createPortal(children: ReactNode, container: Element, key?: null | string): ReactPortal;
type ReactText = string | number;
type ReactChild = ReactElement | ReactText;
```

> ReactNode是一个联合类型，囊括了ReactElement

```tsx
interface ReactElement<P = any, T extends string | JSXElementConstructor<any> = string | JSXElementConstructor<any>> {
    type: T;
    props: P;
    key: Key | null;
}
```

> 写上***(child as ReactElement).props***就不会报错

```tsx
function createElement<P extends {}>(
    type: FunctionComponent<P> | ComponentClass<P> | string,
    props?: Attributes & P | null,
    ...children: ReactNode[]): ReactElement<P>;
```

> ReactElement: createElement的返回值

```tsx
// DOM Elements
// TODO: generalize this to everything in `keyof ReactHTML`, not just "input"
function createElement(
    type: "input",
    props?: InputHTMLAttributes<HTMLInputElement> & ClassAttributes<HTMLInputElement> | null,
    ...children: ReactNode[]): DetailedReactHTMLElement<InputHTMLAttributes<HTMLInputElement>, HTMLInputElement>;
function createElement<P extends HTMLAttributes<T>, T extends HTMLElement>(
    type: keyof ReactHTML,
    props?: ClassAttributes<T> & P | null,
    ...children: ReactNode[]): DetailedReactHTMLElement<P, T>;
function createElement<P extends SVGAttributes<T>, T extends SVGElement>(
    type: keyof ReactSVG,
    props?: ClassAttributes<T> & P | null,
    ...children: ReactNode[]): ReactSVGElement;
function createElement<P extends DOMAttributes<T>, T extends Element>(
    type: string,
    props?: ClassAttributes<T> & P | null,
    ...children: ReactNode[]): DOMElement<P, T>;

// Custom components

function createElement<P extends {}>(
    type: FunctionComponent<P>,
    props?: Attributes & P | null,
    ...children: ReactNode[]): FunctionComponentElement<P>;
function createElement<P extends {}>(
    type: ClassType<P, ClassicComponent<P, ComponentState>, ClassicComponentClass<P>>,
    props?: ClassAttributes<ClassicComponent<P, ComponentState>> & P | null,
    ...children: ReactNode[]): CElement<P, ClassicComponent<P, ComponentState>>;
function createElement<P extends {}, T extends Component<P, ComponentState>, C extends ComponentClass<P>>(
    type: ClassType<P, T, C>,
    props?: ClassAttributes<T> & P | null,
    ...children: ReactNode[]): CElement<P, T>;
function createElement<P extends {}>(
    type: FunctionComponent<P> | ComponentClass<P> | string,
    props?: Attributes & P | null,
    ...children: ReactNode[]): ReactElement<P>;
```

> - `DOMElement<P, T>，FunctionComponentElement啊，CElement<P, ClassicComponent<P, ComponentState>>`, 都是 extends ReactElement,  所以得出`ReactElement`是由`React.createElement()`这个API创建出来的类型，根本上是一个Js对象，如上图所示，在Jsx里面以自定义组件的形式呈现，比如`<MyComponent />`，这就是一个ReactElement，而`<div />`，也是一个`ReactElement`
> - 被React渲染到浏览器上的东西，可以理解为VDOM上的每个节点，都是是ReactNode

```tsx

const Parent = () => {
  return (
   <>
      <div>
        <Child />
      <div/>
    </>
  )
}

const Child = () => {
  return (
   <>
      <div>
       我是孩子
      <div/>
    </>
  )
}

ReactDOM.render(
    <Parent />,
    document.getElementById('root')
)
```

> React.FC的返回值是ReactElement

```ts
interface FunctionComponent<P = {}> {
    (props: PropsWithChildren<P>, context?: any): ReactElement<any, any> | null;
    propTypes?: WeakValidationMap<P> | undefined;
    contextTypes?: ValidationMap<any> | undefined;
    defaultProps?: Partial<P> | undefined;
    displayName?: string | undefined;
}
```

> 返回的类型是明确的ReactElement, 看之前createElement源码部分，有对自定义组件的说明。(当然其实也可以理解为返回ReactNode,因为Element是Node的子集，但还是根据源码准确点好, 因为可以注意类组件的render方法和函数声明方式写的组件，他们渲染的类型都是JSX.Element，也就是说 JSX.Element == ReactElement)

到这，虽然写的有点啰嗦，但我们要知其然并知其所以然嘛。

## 事件处理

> 事件处理, event事件对象类型

```tsx

const onChange = (e) {
  console.log(e.target.value)
}
```

> 受控表单一般都会这么写是吧，但如果你不给e限定类型他就会报错，说e这个类型没有target嘛，这个好解决，cmd点进去看这些合成事件就能看到，大部分啥`ChangeEvent,MouseEvent`

- 剪切板事件对象`:ClipboardEvent<T = Element>`

- 拖拽事件对象:`DragEvent<T = Element>`

- 焦点事件对象:`FocusEvent<T = Element>`

- 表单事件对象:`FormEvent<T = Element>`

- Change事件对象:`ChangeEvent<T = Element>`

- 键盘事件对象:`KeyboardEvent<T = Element>`

- 鼠标事件对象:`MouseEvent<T = Element, E = NativeMouseEvent>`

- 触摸事件对象:`TouchEvent<T = Element>`

- 滚轮事件对象:`WheelEvent<T = Element>`

- 动画事件对象:`AnimationEvent<T = Element>`

- 过渡事件对象:`TransitionEvent<T = Element>`

就比如说我之前写受控表单的时候就是这么写的
![](/__assets__/img/2022-02-12-19-15-22.png)
> 处理表单嘛，所以泛型给的就是HTMLInputElement

这里注意到，onChange这个合成事件的类型是啥？ 老长一串，哎，这也直接看简单的声明吧

```tsx
type EventHandler<E extends SyntheticEvent<any>> = { 
    bivarianceHack(event: E): void 
}["bivarianceHack"];

type ReactEventHandler<T = Element> = EventHandler<SyntheticEvent<T>>;
// 剪切板事件处理函数
type ClipboardEventHandler<T = Element> = EventHandler<ClipboardEvent<T>>;
// 复合事件处理函数
type CompositionEventHandler<T = Element> = EventHandler<CompositionEvent<T>>;
// 拖拽事件处理函数
type DragEventHandler<T = Element> = EventHandler<DragEvent<T>>;
// 焦点事件处理函数
type FocusEventHandler<T = Element> = EventHandler<FocusEvent<T>>;
// 表单事件处理函数
type FormEventHandler<T = Element> = EventHandler<FormEvent<T>>;
// Change事件处理函数
type ChangeEventHandler<T = Element> = EventHandler<ChangeEvent<T>>;
// 键盘事件处理函数
type KeyboardEventHandler<T = Element> = EventHandler<KeyboardEvent<T>>;
// 鼠标事件处理函数
type MouseEventHandler<T = Element> = EventHandler<MouseEvent<T>>;
// 触屏事件处理函数
type TouchEventHandler<T = Element> = EventHandler<TouchEvent<T>>;
// 指针事件处理函数
type PointerEventHandler<T = Element> = EventHandler<PointerEvent<T>>;
// 界面事件处理函数
type UIEventHandler<T = Element> = EventHandler<UIEvent<T>>;
// 滚轮事件处理函数
type WheelEventHandler<T = Element> = EventHandler<WheelEvent<T>>;
// 动画事件处理函数
type AnimationEventHandler<T = Element> = EventHandler<AnimationEvent<T>>;
// 过渡事件处理函数
type TransitionEventHandler<T = Element> = EventHandler<TransitionEvent<T>>;
```

> 编译器一般会有提示，vscode是有的，就比如我们写onChange类型的时候写个Change,后面的EventHandler应该会自动弹出来，实在不行也就直接cmd点进去看源码算了。

## HTML标签类型

> 所有的HTML标签类型都定义在了 node_moudles/@types/react/global.d.ts

### 常见的大概有

- `a`: `HTMLAnchorElement`
- `body`: `HTMLBodyElement`
- `br`: `HTMLBRElement`
- `button`: `HTMLButtonElement`
- `div`: `HTMLDivElement`
- `h1`: `HTMLHeadingElement`
- `h2`: `HTMLHeadingElement`
- `h3`: `HTMLHeadingElement`
- `html`: `HTMLHtmlElement`
- `img`: `HTMLImageElement`
- `input`: `HTMLInputElement`
- `ul`: `HTMLUListElement`
- `li`: `HTMLLIElement`
- `link`: `HTMLLinkElement`
- `p`: `HTMLParagraphElement`
- `span`: `HTMLSpanElement`
- `style`: `HTMLStyleElement`
- `table`: `HTMLTableElement`
- `tbody`: `HTMLTableSectionElement`
- `video`: `HTMLVideoElement`
- `audio`: `HTMLAudioElement`
- `meta`: `HTMLMetaElement`
- `form`: `HTMLFormElement`

> 然后每个标签都还有自己的属性类型，不过用的不多，一般在自己封装原生组件的库的时候会用到(比如antd的Radio组件)，简单了解下

- HTML属性类型:`HTMLAttributes`
- 按钮属性类型:`ButtonHTMLAttributes`
- 表单属性类型:`FormHTMLAttributes`
- 图片属性类型:`ImgHTMLAttributes`
- 输入框属性类型:`InputHTMLAttributes`
- 链接属性类型:`LinkHTMLAttributes`
- meta属性类型:`MetaHTMLAttributes`
- 选择框属性类型:`SelectHTMLAttributes`
- 表格属性类型:`TableHTMLAttributes`
- 输入区属性类型:`TextareaHTMLAttributes`
- 视频属性类型:`VideoHTMLAttributes`
- SVG属性类型:`SVGAttributes`
- WebView属性类型:`WebViewHTMLAttributes`
