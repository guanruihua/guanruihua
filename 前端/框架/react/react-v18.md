# react v18

## Render

- `legacy` 模式： `ReactDOM.render(, rootNode)` (react17)
- `blocking` 模式： `ReactDOM.createBlockingRoot(rootNode).render()` (迁移到concurrent 模式的过渡模式)
- `concurrent` 模式： `ReactDOM.createRoot(rootNode).render()` (reac18, 使用react17 Api 会有警告)

### Spa

```tsx
// React 17
const root = document.getElementById('root')!;
ReactDOM.render(<App />, root, () => {
  console.log('渲染完成');
});

// React 18
const AppWithCallback: React.FC = () => {
  useEffect(() => {
    console.log('渲染完成');
  }, []);
  return <App />;
};
const root = document.getElementById('root')!;
ReactDOM.createRoot(root).render(<AppWithCallback />);
```

### SSR

```tsx
// React 17
import ReactDOM from 'react-dom';
 ReactDOM.hydrate(<App />, document.getElementById('root'));

// React 18
import ReactDOM from 'react-dom/client';
ReactDOM.hydrateRoot(document.getElementById('root'), <App />);
```

## lazy

- 动态加载组件
- 缩减bundle的体积
- 懒加载组件

```jsx
const LazyCom = lazy(()=>import('./com'))
```

## Suspense

- 在服务器上添加对Suspense支持, 并发渲染特性拓展
- 配合`lazy`使用, 可以实现到动态加载组件

```tsx
<Suspense fallback={<Loading/>}>
 <LazyCom />
</Suspense>
```

### ## Suspense 修改

```tsx
// React 17
const App = () => {
  return (
    <Suspense fallback={<Loading />}> // <--- 这个边界被使用，显示 Loading 组件
      <Suspense>                      // <--- 这个边界被跳过，没有 fallback 属性
        <Page />
      </Suspense>
    </Suspense>
  );
};

// React 18
const App = () => {
  return (
    <Suspense fallback={<Loading />}> // <--- 不使用
      <Suspense>                      // <--- 这个边界被使用，将 fallback 渲染为 null
        <Page />
      </Suspense>
    </Suspense>
  );
};

```

## flushSync

- 多个`setState`依然会执行, 可控制不需要精准控制的批量更新
- 减少==渲染次数==
- [详细说明](https://github.com/reactwg/react-18/discussions/21)

### 重新渲染两次

```tsx
import React, { useState } from "react"
import { flushSync } from 'react-dom'

export default function Demo() {
 console.log('App组件渲染了！');
 const [count1, setCount1] = useState(0);
 const [count2, setCount2] = useState(0);
 return (
  <button
   onClick={() => {
     setCount1(count => count + 1);
     setCount2(count => count + 1);
   }}
  >
   <div>count1： {count1}</div>
   <div>count2： {count2}</div>
  </button>
 );

}
```

```tsx
import React, { useState } from "react"
import { flushSync } from 'react-dom'

export default function Demo() {
 console.log('App组件渲染了！');
 const [count1, setCount1] = useState(0);
 const [count2, setCount2] = useState(0);
 return (
  <button
   onClick={() => {
    flushSync(() => {
     setCount1(count => count + 1);
     // setCount2(count => count + 1);
    });
    flushSync(() => {
     setCount2(count => count + 1);
    });
   }}
  >
   <div>count1： {count1}</div>
   <div>count2： {count2}</div>
  </button>
 );

}
```

### 重新渲染一次

```tsx
import React, { useState } from "react"
import { flushSync } from 'react-dom'

export default function Demo() {
 console.log('App组件渲染了！');
 const [count1, setCount1] = useState(0);
 const [count2, setCount2] = useState(0);
 return (
  <button
   onClick={() => {
    flushSync(() => {
     setCount1(count => count + 1);
     setCount2(count => count + 1);
    });
    // flushSync(() => {
    //  setCount2(count => count + 1);
    // });
   }}
  >
   <div>count1： {count1}</div>
   <div>count2： {count2}</div>
  </button>
 );

}
```

## React空组件返回值

- 只允许 `null` ( react17 )
- 允许`null`, `undifined` ( react8 )

## [Strict Mode](https://github.com/reactwg/react-18/discussions/96)

- 严格模式下 React组件渲染两次, 取消一次渲染的控制台日志( react17 )
- 取消该限制, 若安装 `React DevTools` 第二次渲染的日志显示灰色

## useId

- 用于客户端和服务端生成唯一ID
- 支持同一个组件在客户端和服务端生成相同的唯一的 ID，避免 hydration 的不兼容，这解决了在 React 17 及 17 以下版本中已经存在的问题。因为我们的服务器渲染时提供的 HTML 是无序的，useId 的原理就是每个 id 代表该组件在组件树中的层级结构。

```tsx
function NameFields() {
  const id = useId();
  return (
    <div>
      <label htmlFor={id + '-firstName'}>First Name</label>
      <div>
        <input id={id + '-firstName'} type="text" />
      </div>
      <label htmlFor={id + '-lastName'}>Last Name</label>
      <div>
        <input id={id + '-lastName'} type="text" />
      </div>
    </div>
  );
}

```

> 整个表单创建一个基本 ID，然后通过附加后缀从该表单派生更多 id，而不是为 N 个不同的 id 生成单独的钩子 N 次
> id 全局唯一, 后缀局部唯一, 及派生id全局唯一

## useSyncExternalStore

- 由 useMutableSource 改变而来，主要用来解决外部数据撕裂问题
- 一般由第三方状态管理库使用, 日常开发使用useState已经可以解决并发特性下撕裂问题
  - 比如`redux`: 自己再外部维护一个store对象, 脱离了React管理

## useInsertionEffect

- 只建议 `css-in-js` 库来使用
- 执行再`DOM`生成之后

```tsx
const useCSS = rule => {
  useInsertionEffect(() => {
    if (!isInserted.has(rule)) {
      isInserted.add(rule);
      document.head.appendChild(getStyleForRule(rule));
    }
  });
  return rule;
};

const App: React.FC = () => {
  const className = useCSS(rule);
  return <div className={className} />;
};

export default App;
```

## Concurrent Mode

- 并发模式
- 同步不可终端更新(React 17)
- 异步可中断更新(React 18 )

## transition

- `useDeferredValue`, 返回一个延迟响应的值, 让一个state延迟生效
- 和`useTransition`: 相似

```tsx
// useTransiton
const [isPending, startTransition] = useTransition();

const getList = async () => {
  const res: IRes = await request.get({...});
  const list = res?.Response?.Data;
  startTransition( () => { 
      setList(list as IDetail[]); 
  });
};
  
//useDeferredValue
const deferredList = useDeferredValue(list);
```

- 可以将原一个长任务, 分成多个多任务+一个长任务
