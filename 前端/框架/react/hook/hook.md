# React-hook

> - 在不编写class的情况下使用state以及其他特性
>
> - 只能在非状态组件使用

## 基础Hook

### useState

> `const [ stateName,  setStateName ] = useState(initialState)`
>
> 用于解决非状态组件修改非状态组件的变量不会重新渲染的问题

惰性初始化

```js
const [ state, setState ] = useState( ()=> {
  const initialState = someExpensiveComputaion( props );
  return initialState;
})
```

### useEffect

> 相当`componentDidMout`, `componentDidUpdate`

传递一个空数组`([])` 作为第二参数, 这里可以添加需要监听的参数, 只有参数发生改变才会触发;

```js
useEffect( ()=> {
  ...
}, []);
```

```js
// 不建议这样子使用, doSomething 使用 someProp
function Example1({ someProp }){
  function doSomething() {
    console.log( someProp )
  }

  useEffect( () =>{
    doSomething();
  }, []);

  return <p>{{ someProp }}</p>
}

// 建议使用effect 仅仅用到了someProp
function Example2({ someProp }){
  useEffect( ()=> {
    function doSomething(){
      console.log(someProp)
    }
    doSomething();
  }, [someProp]);

  return <p>{{ someProp }}</p>
}
```

### useContext && createContext

> - 用来处理多层级传递数据的方法
> - 在传统方式, 跨组件传递数据需要通过props往下传

```ts
// 返回值{ Provider, Consumer }
const ThemeContext = React.createContext();
```

```tsx
import React  from 'react';
import './App.css';
//创建context
const numberContext = React.createContext();
//它返回一个具有两个值的对象
//{Provider ， Consumer}
function App(){
  //使用Provider为所有子孙提供value值
  return (
    <numberContext.Provider value={12}>
        <div>
        <ShowAn />
        </div>
    </numberContext.Provider>
  )
}

function ShowAn(){
  //使用Consumer从上下文获取value
  return(
    <numberContext.Consumer>
      {value=><div>the answer is {value}</div>}
    </numberContext.Consumer>
  )
}
export default App;
```

```js
import React, { useContext, useState } from 'react';

const ThemeContext = React.createContext(0);

const Son = () => {
 const value = useContext(ThemeContext)
 console.log(value);
 return(
  <p>son</p>
 )
}

const ContextComponent = () => {
 return (
  <Son/>
 )
} 

const ContextPage = () => {
 const [ count, setCount] = useState(1);
 return(
  <div>
   <ThemeContext.Provider value={count}>
    <ContextComponent />
   </ThemeContext.Provider>
   <button onClick={()=> setCount(count + 1)}>
    Click me
   </button>
  </div>
 )
}


export default ContextPage;
```

## 额外Hook

### useReducer

> - 和useState类似
> - 比useState更加适用场景:例如state逻辑处理较复杂且包含多个子值, 或者下一个state依赖于之前的state等

#### 基础用法

```js
import { useReducer } from 'react';

// state 是原来的 状态, action:dispatch传输的参数
// return 的值就是要更新成新的state的值
const reducer = (state, action) => {
 switch(action.type) {
  case 'increment':
   return { count: state.count + 1 };
  case 'decrement':
   return { count: state.count - 1 };
  default:
   throw new Error();
 }
}

const UseReducer = () => {
 let initialState = {
  count: 0,
 }
 const [ state, dispatch ] = useReducer(reducer, initialState );

 return (
  <div>
   <div>useReducer Count:{state.count}</div>
   <button onClick={ ()=> { dispatch({ type: 'decrement'}); }}>useReducer 减少</button>
   <button onClick={ ()=>{ dispatch({ type: 'increment'}); }}>useReducer 增加</button>
  </div>
 )
}

export default UseReducer;
```

#### 惰性初始化

```js
const initCount =0 
const init = initCount => {
  return { count: initCount }
}
const reducer = (state, action)=>{
  switch(action.type){
    case 'increment':
      return {count: state.count + 1}
    case 'decrement':
      return {count: state.count - 1}
    case 'reset':
      return init(action.paylod || 0)
    default:
      throw new Error();
  }
}
const UseReducer = () => {
  const [state, dispatch] = useReducer(reducer,initCount,init)

  return (
    <div className="App">
      <div>useReducer Count:{state.count}</div>
      <button onClick={()=>{dispatch({type:'decrement'})}}>useReducer 减少</button>
      <button onClick={()=>{dispatch({type:'increment'})}}>useReducer 增加</button>
      <button onClick={()=>{dispatch({type:'reset',paylod:10 })}}>useReducer 重置为10</button>
    </div>
  );
}
export default UseReducer;

```

### Memo

> 当父组件重新渲染时, **子组件也会重新渲染**, 即使子组件的props和state都没有发生改变

```js
import { memo, useState } from 'react';

// 子组件
const Child = ({count}) => {
  console.log(`${count} ChildComp...`);
  return (<div>ChildComp...</div>);
};

const ChildComp = memo(Child)
// 父组件
const Parent = () => {
  const [count, setCount] = useState(0);

  return (
    <div>
      <div>hello world {count}</div>
      <button onClick={() => { setCount(count => count + 1); }}>点击增加</button>
      <ChildComp count = {count}/>
    </div>
  );
};

export default Parent;
```

- React.memo与PureComponent的区别：
  - 服务对象不同：PureComponent 服务与类组件，React.memo既可以服务于类组件，也可以服务与函数式组件，useMemo服务于函数式组件（后续讲到）
  - 针对的对象不同：PureComponent针对的是props和state，React.memo只能针对props来决定是否渲染

- `memo(ChildComponent, callback:(pre, next)=>void)`
  - `memo`:返回 true 组件不渲染 ， 返回 false 组件重新渲染
  - `shouldComponentUpdate`: 返回 true 组件渲染 ， 返回 false 组件不渲染

### useMemo

> 目的: 减少子组件数据没有改变也发生重新绘制
>
> useMemo
>
> - 第一个参数: 是个函数, 返回对象只想同一个引用, 不会创建新对象
> - 第二个参数: 是个数组, 只有数组的边来跟发生改变时候, 第一个参数的函数才会返回一个新的对象
>
> memo:　只要父组重绘，子组件也会发生重绘

```js
import React, { memo, useMemo, useState } from 'react';

// 子组件
const ChildComp = ({ info }) => {
 console.log( info )
 const { name, age, count } = info;
  console.log('ChildComp...' + `${count}---${age}---${name}`);
  return (<div>ChildComp...</div>);
};

const MemoChildComp = memo(ChildComp);

// 父组件
const Parent = () => {
  const [count, setCount] = useState(0);
  const [name] = useState('jack');
  const [age] = useState(11);
  
  // 使用 useMemo 将对象属性包一层
  const info = useMemo(() => ({ name, age, count }), [name, age, count]);

  return (
    <div>
      <div>hello world {count}</div>
      <button onClick={() => { setCount(count => count + 1); }}>点击增加</button>
      <MemoChildComp info={info}/>
    </div>
  );
};

export default Parent;
```

### useCallback

> - 当给子组件添加方法的时候, 当只触发了該方法也会导致子组件重绘
>
> - 给該方法用useCallback包含就不会导致子组件重新渲染( 和useMemo 类似 )

```js
import React, { memo, useCallback, useMemo, useState } from 'react';

// 子组件
const ChildComp = (props) => {
  console.log('ChildComp...');
  return (<div>ChildComp...</div>);
};

const MemoChildComp = memo(ChildComp);

// 父组件
const Parent = () => {
  const [count, setCount] = useState(0);
  const [name] = useState('jack');
  const [age] = useState(11);
  const info = useMemo(() => ({ name, age }), [name, age]);
  const changeName = useCallback( () => {
    console.log('输出名称...');
  },[]);

  return (
    <div className="App">
      <div>hello world {count}</div>
      <div onClick={() => { setCount(count => count + 1); }}>点击增加</div>
      <MemoChildComp info={info} changeName={changeName}/>
    </div>
  );
};

export default Parent;
```

### useRef

> - 就像是一个变量, 类似this, 他就像一个盒子, 可以存放任何东西,  createRef每次渲染都会返回一点新的引用, 而useRef每次都会返回相同的引用

```js
import { useRef, useEffect, useState  } from 'react'

const Page1 = () => {
 const myRef = useRef(null);
 useEffect( ()=> {
  myRef?.current?.focus();
 })

 return (
  <div>
   <span>UseRef:</span>
   <input ref={myRef} type="text" />
  </div>
 )
}

const Page2 = () => {
 const myRef2 = useRef(0);
 const [count, setCount] = useState(0)
 useEffect( ()=> {
  myRef2.current = count;
 })
 function handleClick() {
  setTimeout(()=> {
   console.log(count); 
   console.log(myRef2.current);
  })
 }
 return (
  <div>
   <button onClick={()=> setCount(count + 1)}>点击count</button>
   <button onClick={()=> handleClick() }>查看</button>
  </div>
 )
}
```

### useImperativeHandle

> - 使用场景：通过 ref 获取到的是整个 dom 节点，通过 useImperativeHandle 可以控制只暴露一部分方法和属性，而不是整个 dom 节点。
>
> - `useImperativeHandle(ref, createHandle, [deps])`
>
> - ref：定义 current 对象的 ref createHandle：一个函数，返回值是一个对象，即这个 ref 的 current
>
> - 对象 [deps]：即依赖列表，当监听的依赖发生变化，useImperativeHandle 才会重新将子组件的实例属性输出到父组件
>
> - ref 的 current 属性上，如果为空数组，则不会重新输出。
> - 直接转发 ref 是将 React.forwardRef 中函数上的 ref 参数直接应用在了返回元素的 ref 属性上，其实父、子组件引用的是同一个 ref 的 current 对象，官方不建议使用这样的 ref 透传，而使用 useImperativeHandle 后，可以让父、子组件分别有自己的 ref，通过 React.forwardRef 将父组件的 ref 透传过来，通过 useImperativeHandle 方法来自定义开放给父组件的 current

```jsx
import React, { useCallback, useRef, useImperativeHandle } from 'react';

// 实现 ref 的转发
const FancyButton = React.forwardRef((props, ref) => (
 <div>
  <input ref={ref} type="text" />
  <button>{props.children}</button>
 </div>
));

// 父组件中使用子组件的 ref
function App() {
 const ref = useRef();
 const handleClick = useCallback(() => ref.current.focus(), [ref]);

 return (
  <div>
   <FancyButton ref={ref}>Click Me</FancyButton>
   <button onClick={handleClick}>获取焦点</button>
  </div>
 )
}

const FancyInput = React.forwardRef((props, ref) => {
 const inputRef = useRef();
 useImperativeHandle(ref, () => ({
  focus: () => {
   inputRef.current.focus();
  }
 }));

 return <input ref={inputRef} type="text" />
});

const App2 = props => {
 const fancyInputRef = useRef();

 return (
  <div>
   <FancyInput ref={fancyInputRef} />
   <button
    onClick={() => fancyInputRef.current.focus()}
   >父组件调用子组件的 focus</button>
  </div>
 )
}

// eslint-disable-next-line import/no-anonymous-default-export
export default () => {
 return (
  <div><App /><App2 /></div>
 )
}
```

### useLayoutEffect

> 其函数签名与 useEffect 相同，但它会在所有的 DOM 变更之后同步调用 effect，这里不再举例。
>
> - useLayoutEffect 和平常写的 Class 组件的 componentDidMount 和 componentDidUpdate 同时执行；
> - useEffect 会在本次更新完成后，也就是第 1 点的方法执行完成后，再开启一次任务调度，在下次任务调度中执行 useEffect；

```js
/*
 * @Author: ruihuag 
 * @Date: 2021-06-01 15:59:09 
 * @Last Modified by: ruihuag
 * @Last Modified time: 2021-06-01 16:00:18
 * @desciption: 
 就是useLayoutEffect 的执行时机要早于 useEffect，useLayoutEffect的执行在类组件生命周期前，useEffect 的执行在类组件生命周期后，官方的建议是要求我们尽量使用useEffect，以避免阻塞视觉更新，如果是将代码从类组件重构为 React Hooks，并且使用 useEffect出现问题，再考虑使用 useLayoutEffect，服务端渲染时使用useLayoutEffect 会触发警告。
 */

import React, { useState, useEffect, useLayoutEffect } from 'react';

function Com() {
 useEffect(() => {
  console.log('useEffect 执行...');
  return () => {
   console.log('useEffect 销毁...');
  }
 });

 useLayoutEffect(() => {
  console.log('useLayoutEffect 执行...');
  return () => {
   console.log('useLayoutEffect 销毁...');
  }
 });

 return (
  <div>
   {console.log('Com 渲染')}
   <h2>Com1</h2>
  </div>
 )
}

const App = props => {
 const [count, setCount] = useState(0)
 return (
  <div>
   <Com />
   {count}
   <button onClick={() => setCount(count + 1)}>count + 1</button>
  </div>
 )
}

export default App;
```
