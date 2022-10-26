---
title: api
date: 2020-11-20 20:53:49
tags: 
- api
- dva
---

# dva_api

## [#](https://dvajs.com/api/#输出文件)输出文件

### [#](https://dvajs.com/api/#dva)dva

默认输出文件。

### [#](https://dvajs.com/api/#dva-router)dva/router

默认输出 [react-router](https://github.com/ReactTraining/react-router) 接口， [react-router-redux](https://github.com/reactjs/react-router-redux) 的接口通过属性 routerRedux 输出。

比如：

```js
import { Router, Route, routerRedux } from 'dva/router';
```

### [#](https://dvajs.com/api/#dva-fetch)dva/fetch

异步请求库，输出 [isomorphic-fetch](https://github.com/matthew-andrews/isomorphic-fetch) 的接口。不和 dva 强绑定，可以选择任意的请求库。

### [#](https://dvajs.com/api/#dva-saga)dva/saga

输出 [redux-saga](https://github.com/yelouafi/redux-saga) 的接口，主要用于用例的编写。（用例中需要用到 effects）

### [#](https://dvajs.com/api/#dva-dynamic)dva/dynamic

解决组件动态加载问题的 util 方法。

比如：

```js
import dynamic from 'dva/dynamic';

const UserPageComponent = dynamic({
  app,
  models: () => [
    import('./models/users'),
  ],
  component: () => import('./routes/UserPage'),
});
```

`opts` 包含：

- app: dva 实例，加载 models 时需要
- models: 返回 Promise 数组的函数，Promise 返回 dva model
- component：返回 Promise 的函数，Promise 返回 React Component

## [#](https://dvajs.com/api/#dva-api)dva API

### [#](https://dvajs.com/api/#app-dva-opts)`app = dva(opts)`

创建应用，返回 dva 实例。(注：dva 支持多实例)

`opts` 包含：

- `history`：指定给路由用的 history，默认是 `hashHistory`
- `initialState`：指定初始数据，优先级高于 model 中的 state，默认是 `{}`

如果要配置 history 为 `browserHistory`，可以这样：

```js
import createHistory from 'history/createBrowserHistory';
const app = dva({
  history: createHistory(),
});
```

另外，出于易用性的考虑，`opts` 里也可以配所有的 [hooks](https://dvajs.com/api/#appusehooks) ，下面包含全部的可配属性：

```js
const app = dva({
  history,
  initialState,
  onError,
  onAction,
  onStateChange,
  onReducer,
  onEffect,
  onHmr,
  extraReducers,
  extraEnhancers,
});
```

### [#](https://dvajs.com/api/#app-use-hooks)`app.use(hooks)`

配置 hooks 或者注册插件。（插件最终返回的是 hooks ）

比如注册 [dva-loading](https://github.com/dvajs/dva-loading) 插件的例子：

```js
import createLoading from 'dva-loading';
...
app.use(createLoading(opts));
```

`hooks` 包含：

#### [#](https://dvajs.com/api/#onerror-err-dispatch)`onError((err, dispatch) => {})`

`effect` 执行错误或 `subscription` 通过 `done` 主动抛错时触发，可用于管理全局出错状态。

注意：`subscription` 并没有加 `try...catch`，所以有错误时需通过第二个参数 `done` 主动抛错。例子：

```js
app.model({
  subscriptions: {
    setup({ dispatch }, done) {
      done(e);
    },
  },
});
```

如果我们用 antd，那么最简单的全局错误处理通常会这么做：

```js
import { message } from 'antd';
const app = dva({
  onError(e) {
    message.error(e.message, /* duration */3);
  },
});
```

#### [#](https://dvajs.com/api/#onaction-fn-fn)`onAction(fn | fn[])`

在 action 被 dispatch 时触发，用于注册 redux 中间件。支持函数或函数数组格式。

例如我们要通过 [redux-logger](https://github.com/evgenyrodionov/redux-logger) 打印日志：

```js
import createLogger from 'redux-logger';
const app = dva({
  onAction: createLogger(opts),
});
```

#### [#](https://dvajs.com/api/#onstatechange-fn)`onStateChange(fn)`

`state` 改变时触发，可用于同步 `state` 到 localStorage，服务器端等。

#### [#](https://dvajs.com/api/#onreducer-fn)`onReducer(fn)`

封装 reducer 执行。比如借助 [redux-undo](https://github.com/omnidan/redux-undo) 实现 redo/undo ：

```js
import undoable from 'redux-undo';
const app = dva({
  onReducer: reducer => {
    return (state, action) => {
      const undoOpts = {};
      const newState = undoable(reducer, undoOpts)(state, action);
      // 由于 dva 同步了 routing 数据，所以需要把这部分还原
      return { ...newState, routing: newState.present.routing };
    },
  },
});
```

#### [#](https://dvajs.com/api/#oneffect-fn)`onEffect(fn)`

封装 effect 执行。比如 [dva-loading](https://github.com/dvajs/dva-loading) 基于此实现了自动处理 loading 状态。

#### [#](https://dvajs.com/api/#onhmr-fn)`onHmr(fn)`

热替换相关，目前用于 [babel-plugin-dva-hmr](https://github.com/dvajs/babel-plugin-dva-hmr) 。

#### [#](https://dvajs.com/api/#extrareducers)`extraReducers`

指定额外的 reducer，比如 [redux-form](https://github.com/erikras/redux-form) 需要指定额外的 `form` reducer：

```js
import { reducer as formReducer } from 'redux-form'
const app = dva({
  extraReducers: {
    form: formReducer,
  },
});
```

#### [#](https://dvajs.com/api/#extraenhancers)`extraEnhancers`

指定额外的 [StoreEnhancer](https://github.com/reactjs/redux/blob/master/docs/Glossary.md#store-enhancer) ，比如结合 [redux-persist](https://github.com/rt2zz/redux-persist) 的使用：

```js
import { persistStore, autoRehydrate } from 'redux-persist';
const app = dva({
  extraEnhancers: [autoRehydrate()],
});
persistStore(app._store);
```

### [#](https://dvajs.com/api/#app-model-model)`app.model(model)`

注册 model，详见 [#Model](https://dvajs.com/api/#model) 部分。

### [#](https://dvajs.com/api/#app-unmodel-namespace)`app.unmodel(namespace)`

取消 model 注册，清理 reducers, effects 和 subscriptions。subscription 如果没有返回 unlisten 函数，使用 `app.unmodel` 会给予警告⚠️。

### [#](https://dvajs.com/api/#app-replacemodel-model)`app.replaceModel(model)`

> 只在app.start()之后可用

替换model为新model，清理旧model的reducers, effects 和 subscriptions，但会保留旧的state状态，对于HMR非常有用。subscription 如果没有返回 unlisten 函数，使用 `app.unmodel` 会给予警告⚠️。

如果原来不存在相同namespace的model，那么执行`app.model`操作

### [#](https://dvajs.com/api/#app-router-history-app-routerconfig)`app.router(({ history, app }) => RouterConfig)`

注册路由表。

通常是这样的：

```js
import { Router, Route } from 'dva/router';
app.router(({ history }) => {
  return (
    <Router history={history}>
      <Route path="/" component={App} />
    </Router>
  );
});
```

推荐把路由信息抽成一个单独的文件，这样结合 [babel-plugin-dva-hmr](https://github.com/dvajs/babel-plugin-dva-hmr) 可实现路由和组件的热加载，比如：

```js
app.router(require('./router'));
```

而有些场景可能不使用路由，比如多页应用，所以也可以传入返回 JSX 元素的函数。比如：

```js
app.router(() => <App />);
```

### [#](https://dvajs.com/api/#app-start-selector)`app.start(selector?)`

启动应用。`selector` 可选，如果没有 `selector` 参数，会返回一个返回 JSX 元素的函数。

```js
app.start('#root');
```

那么什么时候不加 `selector`？常见场景有测试、node 端、react-native 和 i18n 国际化支持。

比如通过 react-intl 支持国际化的例子：

```js
import { IntlProvider } from 'react-intl';
...
const App = app.start();
ReactDOM.render(<IntlProvider><App /></IntlProvider>, htmlElement);
```

## [#](https://dvajs.com/api/#model)Model

model 是 dva 中最重要的概念。以下是典型的例子：

```js
app.model({
  namespace: 'todo',
  state: [],
  reducers: {
    add(state, { payload: todo }) {
      // 保存数据到 state
      return [...state, todo];
    },
  },
  effects: {
    *save({ payload: todo }, { put, call }) {
      // 调用 saveTodoToServer，成功后触发 `add` action 保存到 state
      yield call(saveTodoToServer, todo);
      yield put({ type: 'add', payload: todo });
    },
  },
  subscriptions: {
    setup({ history, dispatch }) {
      // 监听 history 变化，当进入 `/` 时触发 `load` action
      return history.listen(({ pathname }) => {
        if (pathname === '/') {
          dispatch({ type: 'load' });
        }
      });
    },
  },
});
```

model 包含 5 个属性：

### [#](https://dvajs.com/api/#namespace)namespace

model 的命名空间，同时也是他在全局 state 上的属性，只能用字符串，不支持通过 `.` 的方式创建多层命名空间。

### [#](https://dvajs.com/api/#state)state

初始值，优先级低于传给 `dva()` 的 `opts.initialState`。

比如：

```js
const app = dva({
  initialState: { count: 1 },
});
app.model({
  namespace: 'count',
  state: 0,
});
```

此时，在 `app.start()` 后 state.count 为 1 。

### [#](https://dvajs.com/api/#reducers)reducers

以 key/value 格式定义 reducer。用于处理同步操作，唯一可以修改 `state` 的地方。由 `action` 触发。

格式为 `(state, action) => newState` 或 `[(state, action) => newState, enhancer]`。

详见： https://github.com/dvajs/dva/blob/master/packages/dva-core/test/reducers.test.js

### [#](https://dvajs.com/api/#effects)effects

以 key/value 格式定义 effect。用于处理异步操作和业务逻辑，不直接修改 `state`。由 `action` 触发，可以触发 `action`，可以和服务器交互，可以获取全局 `state` 的数据等等。

格式为 `*(action, effects) => void` 或 `[*(action, effects) => void, { type }]`。

type 类型有：

- `takeEvery`
- `takeLatest`
- `throttle`
- `watcher`

详见：https://github.com/dvajs/dva/blob/master/packages/dva-core/test/effects.test.js

### [#](https://dvajs.com/api/#subscriptions)subscriptions

以 key/value 格式定义 subscription。subscription 是订阅，用于订阅一个数据源，然后根据需要 dispatch 相应的 action。在 `app.start()` 时被执行，数据源可以是当前的时间、服务器的 websocket 连接、keyboard 输入、geolocation 变化、history 路由变化等等。

格式为 `({ dispatch, history }, done) => unlistenFunction`。

注意：如果要使用 `app.unmodel()`，subscription 必须返回 unlisten 方法，用于取消数据订阅。