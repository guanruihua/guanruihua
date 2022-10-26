---
title: react_router
date: 2020-11-26 15:05:10
tags:
- react
- router
---

# react_router

## router路由传参数

## 路由配置方法

### 标签

```jsx
import { IndexRoute } from 'react-router'
 
const Dashboard = React.createClass({
  render() {
    return <div>Welcome to the app!</div>
  }
})
 
React.render((
  <Router>
    <Route path="/" component={App}>
      <IndexRoute component={Dashboard} />
      <Route path="about" component={About} />
      <Route path="inbox" component={Inbox}>
        <Route path="messages/:id" component={Message} />
      </Route>
    </Route>
  </Router>
), document.body)
```

### 对象

```jsx
const routeConfig = [
  { path: '/',
    component: App,
    indexRoute: { component: Dashboard },
    childRoutes: [
      { path: 'about', component: About },
      { path: 'inbox',
        component: Inbox,
        childRoutes: [
          { path: '/messages/:id', component: Message }
        ]
      }
    ]
  }
]
 
React.render(<Router routes={routeConfig} />, document.body)
```

### useParams

> `useParams` : 通过对`url`进行取值

```jsx
import { BrowserRouter as Router, Link, Route, Switch, useParams } from 'react-router-dom';

function RouterParams() {
  let { id,name } = useParams()
  console.log(useParams());
  return (<>
      <h1>ID: {id}</h1>
      <h1>Name: {name}</h1>
  </>)
}

function App() {
  return (
    <Router>
      <div>
        <h2>Params </h2>
        <Link to="/aa/ac">aa</Link>
        <Link to="/bb/bc">bb</Link>
        <Switch>
          <Route path="/:id/:name" children={<RouterParams />} />
        </Switch>
      </div>
    </Router>
  )
}
```

|        | id   | name |
| ------ | ---- | ---- |
| 点击aa | aa   | ac   |
| 点击bb | bb   | bc   |

## API

### Hooks

> - useHistory : 允许访问可能用于导航的历史记录
> - useLocation : 返回当前URL的位置对象,
> - useParams : 返回参数键/值对的对象
> - useRouteMatch : 尝试以与匹配方式匹配当前URL, 主要访问匹配数据而不实际呈现
>   - 不采用任何参数, 返回当前匹配对象`<Route>`
>   - 采用单个参数, 与matchPath 的参数相同,

### BrowerRouter

> - `basename : string`  => 所有位置基本URl
> - `forRefresh : bool` =>    是否强制刷新刷新
> - `getUserConfirmation : func` =>  用于确认导航的函数
> - `keyLength : number` => location.key 默认6
> - `children : node`
>   - 要呈现的子元素
>   - `React<16` : 必须使用单个元素, 因为渲染方法不能返回多个元素, 如果需要多个元素, 可以尝试将他们包装在一个额外的元素

### HashRouter

>- `basename : string`
>- `getUserConfirmation: func`
>- `hashType:string`
>- `chilren:node`

### Link

> - `to: string`
>
>   - ```jsx
>     <Link to="/courses?sort=name" />
>     ```
>
> - `to :object`
>
>   - ```jsx
>     <Link
>       to={{
>         pathname: "/courses",
>         search: "?sort=name",
>         hash: "#the-hash",
>         state: { fromDashboard: true }
>       }}
>     />
>     ```
>
> - `to: func`
>
>   - ```jsx
>     <Link to={location => ({ ...location, pathname: "/courses" })} />
>     <Link to={location => `${location.pathname}?sort=name`} />
>     ```
>
> - `replace:bool`
>
>   - 取代当前堆栈中的当前链接, 不是添加一个
>
> - `innerRef: func`
>
>   - ```jsx
>     <Link
>       to="/"
>       innerRef={node => {
>         // `node` refers to the mounted DOM element
>         // or null when unmounted
>       }}
>     />
>     ```
>
> - `innerRef: RefObject`
>
>   - ```jsx
>     let anchorRef = React.createRef()
>     <Link to="/" innerRef={anchorRef} />
>     ```
>
> - `component:React.Component`
>
>   - ```jsx
>     const FancyLink = React.forwardRef((props, ref) => (
>       <a ref={ref} {...props}>💅 {props.children}</a>
>     ))
>     <Link to="/" component={FancyLink} />
>     ```

### NavLink

> - `activeClassName: string`
> - `activeStyle:objecct`
> - `exact:bool`
> - `strict: bool`
> - `isActive:func`
> - `location: object`
> - `aria-current: string`

### MemoryRouter

> - `initialEntries: array`
>
>   - ```js
>     <MemoryRouter
>      initialEnriies={["/one", "/two", { pathname: "/three"}]}
>      initialIndex={1} //在数组中初始位置索引
>     >
>         <App />
>     </MemoryRotuer>
>     ```
>
>
>
> - `initialIndex: number`
>
> - `getUserConfirmation: func`
>
> - `keyLength: number`
>
> - `children: node`

### Redirect

> - `to : string`
>
>   - ```jsx
>     <Redirect to="/somewhere/else" />
>     ```
>
> - `to: object`
>
>   - ```jsx
>     <Redirect
>       to={{
>         pathname: "/login",
>         search: "?utm=your+face",
>         state: { referrer: currentLocation }
>       }}
>     />
>     ```
>
> - `push: bool`
>
>   - 重新加入一个history, 而不是取代当前的
>
>   - ```jsx
>     <Redirect push to="/somewhere/else" />
>     ```
>
> - `from: string`
>
>   - 重定向`old-path` 跳转到`new-path`
>
>   - ```jsx
>     <Switch>
>       <Redirect from="/old-path" to="/new-path" />
>       <Route path="/new-path">
>         <Place />
>       </Route>
>     </Switch>
>     ```
>
> - `exact : bool`
>
>   - true : 完全匹配
>
>   - |  path  | location.pathname |  exact  | matches? |
>     | :----: | :---------------: | :-----: | :------: |
>     | `/one` |    `/one/two`     | `true`  |    no    |
>     | `/one` |    `/one/two`     | `false` |   yes    |
>
> - `strict: bool`
>
>   - |  path   | location.pathname | matches? |
>     | :-----: | :---------------: | :------: |
>     | `/one/` |      `/one`       |    no    |
>     | `/one/` |      `/one/`      |   yes    |
>     | `/one/` |    `/one/two`     |   yes    |
>
> - `sensitive : bool`
>
>   - 区分大小写
>
>   - |  path  | location.pathname | sensitive | matches? |
>     | :----: | :---------------: | :-------: | :------: |
>     | `/one` |      `/one`       |  `true`   |   yes    |
>     | `/One` |      `/one`       |  `true`   |    no    |
>     | `/One` |      `/one`       |  `false`  |   yes    |

### Route

> - Route render methods
>
>   - `<Route component/>`
>   - `<Route render/>`
>   - `<Route children> fucntion`
>
> - Route props
>
>   - match
>   - location
>   - history
>
> - component `<Route path="/user/:username" component={User} />`
>
> - `render: func`
>
> - `children: func`
>
> - `path: string | string[]` :
>
>   ```jsx
>   <Route path={["/users/:id", "/profile/:id"]}>
>     <User />
>   </Route>
>   ```
>
> - `exact : bool`
>
>   - true : 完全匹配
>
>   - |  path  | location.pathname |  exact  | matches? |
>     | :----: | :---------------: | :-----: | :------: |
>     | `/one` |    `/one/two`     | `true`  |    no    |
>     | `/one` |    `/one/two`     | `false` |   yes    |
>
> - `strict: bool`
>
>   - |  path   | location.pathname | matches? |
>     | :-----: | :---------------: | :------: |
>     | `/one/` |      `/one`       |    no    |
>     | `/one/` |      `/one/`      |   yes    |
>     | `/one/` |    `/one/two`     |   yes    |
>
> - `sensitive : bool`
>
>   - 区分大小写
>
>   - |  path  | location.pathname | sensitive | matches? |
>     | :----: | :---------------: | :-------: | :------: |
>     | `/one` |      `/one`       |  `true`   |   yes    |
>     | `/One` |      `/one`       |  `true`   |    no    |
>     | `/One` |      `/one`       |  `false`  |   yes    |

### Router

> - 是一个低级接口, 一般需要用一个高级Router代替
>   - `<BrowserRouter>`
>   - `<HashRouter>`
>   - `<MenoryRouter>`
>   - `<NativeRouter>`
>   - `<StaticRouter>`
> - 属性
>   - `history: object`
>   - `children:node`

```js
import React from "react";
import ReactDOM from "react-dom";
import { Router } from "react-router";
import { createBrowserHistory } from "history";

const history = createBrowserHistory();

ReactDOM.render(
  <Router history={history}>
    <App />
  </Router>,
  node
);
```

### StaticRouter

> - `basename:string`
> - `location:`
>   - `string`
>   - `object`
> - `context:object`
> - `children: node`

### Switch

> - `location:object`
> - `children:node`

### generatePath

> - 用于生成路由的URL
> - `pattern: string`
> - `params:object`
>
> ```js
> generatePath("/user/:id/:entity(posts|comments)", { id: 1 });
> ```

### history

> ‎管理 JavaScript 中的会话历史记录。
>
> ‎`history`‎ 以下术语：‎
>
> - ‎"浏览器历史记录" - 特定于 DOM 的实现，在支持 HTML5 历史记录 API 的 Web 浏览器中非常有用‎
> - ‎"哈希历史记录" - 针对旧版 Web 浏览器的 DOM 特定实现‎
> - ‎"内存历史记录" - 内存历史记录实现，在测试和非 DOM 环境（如 React Native）中非常有用‎
>
> `history`‎对象通常具有以下属性和方法：‎
>
> - `length`‎- （数字） 历史记录堆栈中的条目数‎
> - `action`‎- （字符串） 当前操作 （或‎`PUSH``REPLACE``POP`)
> - location（对象） 当前位置。可能具有以下属性：‎
>   - `pathname`‎- （字符串） URL 的路径‎
>   - `search`‎- （字符串） URL 查询字符串‎
>   - `hash`‎- （字符串） URL 哈希片段‎
>   - `state`‎- （对象）特定位置状态，例如 当此位置被推送到堆栈上时。仅在浏览器和内存历史记录中可用。‎`push(path, state)`
> - `push(path, [state])`‎- （函数） 将新条目推送到历史记录堆栈‎
> - `replace(path, [state])`‎- （函数） 替换历史记录堆栈上的当前条目‎
> - `go(n)`‎- （函数） 按条目移动历史记录堆栈中的指针‎`n`
> - `goBack()`‎- （功能） 等效于‎`go(-1)`
> - `goForward()`‎- （功能） 等效于‎`go(1)`
> - `block(prompt)`‎- （功能） 阻止导航（‎[‎请参阅历史记录文档‎](https://github.com/ReactTraining/history/blob/master/docs/blocking-transitions.md))

```jsx
import { createBrowserHistory } from "history";
const customHistory = createBrowserHistory();
ReactDOM.render(<Router history={customHistory} />, node);
```

### location

> 返回当前app在哪里

一个Location对象

```js
{
  key: 'ac3df4', // not with HashHistory!
  pathname: '/somewhere',
  search: '?some=search-string',
  hash: '#howdy',
  state: {
    [userDefined]: true
  }
}
```

> - [Route component](https://reactrouter.com/web/api/Route/component) as `this.props.location`
> - [Route render](https://reactrouter.com/web/api/Route/render-func) as `({ location }) => ()`
> - [Route children](https://reactrouter.com/web/api/Route/children-func) as `({ location }) => ()`
> - [withRouter](https://reactrouter.com/web/api/withRouter) as `this.props.location`

### match

> - `params` - (object) 从URL解析所得到的键/值对相对应的动态路径的部分
> - `isExact` - (boolean) 如果匹配整个URL`true`
> - `path` - (string) 路径模式用于匹配。 有助于建立嵌套的 `<Route>`
> - `url` - (string) 匹配URL的一部分。 有助于建立嵌套的`<Link>`

> - [Route component](https://reactrouter.com/web/api/Route/component) as `this.props.match`
> - [Route render](https://reactrouter.com/web/api/Route/render-func) as `({ match }) => ()`
> - [Route children](https://reactrouter.com/web/api/Route/children-func) as `({ match }) => ()`
> - [withRouter](https://reactrouter.com/web/api/withRouter) as `this.props.match`
> - [matchPath](https://reactrouter.com/web/api/matchPath) as the return value
> - [useRouteMatch](https://reactrouter.com/web/api/hooks/useroutematch) as the return value

### matchPath

> - pathname : 匹配路径名
>   - 如果在与节点的服务器上使用node.js , 则为`req.path`
> - props
> - returns

### withRotuer

> - 可以通过高阶组件访问历史记录的属性和最接近的`<Route>`的匹配项, 每单呈现, 都会将更新或传递给包装的组件
> - 包装组件的所有非反应特定静态方法和属性将自动复制到"已连接"组件

### Prompt

> - 从核心提示符重新导出
