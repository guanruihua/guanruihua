# react开发建议

- `react`使用 ts 开发建议
- 钩子函数使用以及封装建议
- 代码提取, 减低耦合度
- 很多都是建议, 具体要看自己去发散

## 代码提取, 减低耦合度

- 提取(常量, 方法等)模块的时候, 要记得区分是否包含这页面业务, 若是非通用的模块, 就不用提取到全局的`uti`l的方法文件中, 直接在当前页面模块中新建一个`util`文件来书写, 组件封装也同理

### 常用变量做提取

```tsx
// index.tsx
function App(){

 // 假设当前变量只存在引用, 不存在修改当前数组的原始值, 就可以把当前变量提取到一个常量文件(constant)中
 const list = [
  1,2,3, 
  1,2,3, 
  1,2,3, 
  1,2,3, 
 ...
 ]
 ...
}
```

- 修改后
  
```tsx
// constant.ts
 export const list = [
  1,2,3, 
  1,2,3, 
  1,2,3, 
  1,2,3, 
 ...
 ]


// index.tsx
import { list } from "./constant";
function App(){

 ...
}
```

- 提取建议:
  - 数组,对象, 常量等, 不需要修改的值, 都可以提取, 若需要频繁修改的不建议使用
  - 类型使用: 不需要, 不可修改的值, 不需要写类型

### 方法的提取

```tsx
function App(){
 const a = 100

 // 这里假设algo 是个复杂方法
 function algo():number{
  return a + 1000
 }

 return <div>{algo()}</div>
}
```

- 修改后

```tsx
// util.ts
export function algo(a:number):number{
  return a + 1000
}
// index.tsx
import { algo } from './util'

function App(){
 const a = 100
 return <div>{algo(a)}</div>
}
```

## 钩子函数使用

- 不熟悉钩子的, 不建议使用
- 这次只会说`useState`, `useEffect`, 最基础, 最常用的钩子
- 其他的可以自行摸索

### useState

- 类似强类型语言Java的泛型用法

```tsx
interface User{
 name: string,
 sex: 'man' | 'woman'
}

function App(){
 const [name, setName] = useState<string>('ruihuag')

 const [user, setUser] = useState<User>({
   name: 'ruihuag',
   sex: 'man'
  })

 return <div />
}
```

### useEffect

- 可以理解为 class 组件中的 `componentDidMount`、`componentDidUpdate` 和 `componentWillUnmount` 的组合
- 当依赖参数发生修改, `useEffect` 可以再次触发
  - 注意防止死循环的出现

  - 若依赖参数为引用类型的参, 只有重新赋值, 才会发生改变, 才可以触发`useEffect`, 大部分情况不建议添加引用类型做为依赖, 很难判断是否重新触发, 建议指定到对应到引用类型中的基础类型参中

```tsx
function App(){

 useEffect(()=>{
  ...
 }, [ 依赖参数])

 return <div/>
}
```

## 钩子封装

- 这个包含一个及以上钩子一起使用, 并封装成自己的hook
- 用use开头的方法名
- 具体封装可以看该项目源码或思路: [ahooks](ahooks.js.org/zh-CN/hooks/use-set-state)
- hooks 封装可以当做特殊的util的来封装, hook 的加入, 可以达到 将组件的处理转移到hook里

### useSetState

- 为了实现和 class 组件 的 `this.setState`基本一致性

```tsx
// ahooks api 写的, 有点不做人的样子对吧, 要先看过ts的相关使用才好阅读
const [state, setState] = useSetState<T extends Record<string, any>>(
  initialState: T = {} as T
): [T, (patch: Partial<T> | ((prevState: T) => Partial<T>)) => void]

// 使用
// 基础数据类型没有变化
 const [name, setName] = useSetState<string>('ruihuag')

// 引用类型的使用就不一样, 有一点点不一样
 const [user, setUser] = useSetState<User>({
   name: 'ruihuag',
   sex: 'man'
  }) 
// 若是useState, 修改需要
setUser({...user, name: 'peiyanh'})
// useSetState
setUser({name: 'peiyanh'})
```

- 源码

```tsx
export type SetState<S extends Record<string, any>> = <K extends keyof S>(
  state: Pick<S, K> | null | ((prevState: Readonly<S>) => Pick<S, K> | S | null),
) => void;

export const useSetState = <S extends Record<string, any>>(
  initialState: S | (() => S),
): [S, SetState<S>] => {
  const [state, setState] = useState<S>(initialState);

  const setMergeState = useCallback((patch) => {
    setState((prevState) => {
   // 主要的处理就在这里, 是不是和 用useState 几乎一致
      const newState = isFunction(patch) ? patch(prevState) : patch;
      return newState ? { ...prevState, ...newState } : prevState;
    });
  }, []);

  return [state, setMergeState];
};

```

### 简答钩子实现

- 若你想在setState前做的处理, 当时不想放到组件中书写, 也可以放到封装的hook中

```tsx
function App(){
 const [val, setVal] = useState<number>(1)
 function handleAdd(num:number):number{
  ...
 return num ** 2
 }
 return <div>
  {val}
  <button onclick={()=>{setVal(handleAdd(val))}}> add </button>
 </div>
}
```

- 简单封装一下

```tsx
// hook.ts
function useAddNum<T = number>(initialState:T):[T, {val:T}=>void]{
 const [val, _setVal] = useState<number>(1)
 function setVal(num:number):number{
  ...
  _setVal( num ** 2)
 }

 return [val, setVal]
}

// index.tsx
import { useAddNum } from './hook'
function App(){
 const [val, setVal] = useAddNum<number>(1)
 return <div>
  {val}
  <button onclick={()=>{setVal(val)}}> add </button>
 </div>
}
```

- 可以减少App的代码, 增加代码可读性, 方便定位方法主要逻辑, 入口的组件
- 可以根据自己的业务做对应修改
- 但是不要为了封装而封装

## react 使用 ts

- 类型名可以和组件名重复

### class 组件

```jsx
export default class Demo extends React.Component{
 render(){  
  const { name, sex } = props
  return <div/>
 }
}
```

- 加上ts

```tsx
interface Demo {
 name: string
 sex: 'man' | 'woman'
}

class Demo extends React.Component<Demo> {
 render(): React.ReactNode {
  const { name, sex } = this.props
  return <div>
   demo2
  </div>
 }
}

export default Demo
```

### function 组件

- 没有使用ts写法

```jsx
export default function Demo(props){
 const { name, sex } = props
 return <div/>
}
```

- 使用ts的写法

```tsx
interface Demo {
 name: string
 sex: 'man' | 'woman'
}

export default function Demo(props: Demo) {
 const { name, sex } = props
 return <div>Demo1</div>
}
```

```tsx
interface Demo {
 name: string
 sex: 'man' | 'woman'
}

const Demo:React.FC<Demo> = (props)=> {
 const { name, sex } = props
 return <div>Demo1</div>
}

export default Demo
```

## react 拓展

- 主要说一下, class组件通过组件嵌套使用hook

### class 使用 hook

- 结构: function 组件 > class 组件 或 class 组件 > function 组件
- 无论怎么样封装都是只能在function组件定义, 若class组件想要使用, 只能通过function组件间接使用

```tsx



export class Cdom extends React.Component<{ val: number, setVal: (val: number) => void }> {
 render(): React.ReactNode {

  const { val, setVal } = this.props

  return <div>
   Cdom val: {val}
   <button onClick={() => { setVal(val + 1) }}>add</button>
  </div>
 }
}


export default function Fdom() {
 const [val, setVal] = React.useState<number>(0)
 return <div>
  fDom value: {val}
  <Cdom
   val={val}
   setVal={setVal}
  />
 </div>
}
```

- 这就是核心思路, Cdom 组件负责事件触发以及 渲染, 定义hook还是在Fdom
- 也可以给useState封装成一个生成useState的方法, 但是不建议, 这样子有点过度封装
