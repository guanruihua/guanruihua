---
title: dva-basic
date: 2020-11-18 09:05:31
tags: 
- dva
- basic
---

# dva-basic

> 基于redux和redux-saga的数据流方案, 简化开发, dav还额外内置react-router和fetch

## dynamic(opts)

> opts
>
> - app:dva实例, 加载models时需要
> - models: 返回Promise数组的函数, Promise返回dva model
> - component: 返回Promise的函数, Promise返回React Component

```react
// 路由文件
import { Router, Route } from 'dva/router';
import IndexPage from './routes/IndexPage'
import TodoList from './routes/TodoList'

function RouterConfig({ history }) {
  return (
    <Router history={history}>
        <Route path="/" component={IndexPage} />
        <Route path='/todoList' components={TodoList}/>
    </Router>
  )
}
export default RouterConfig
```



改成



```react
import { Router, Switch, Route } from 'dva/router'
import dynamic from 'dva/dynamic'

function RouterConfig({ history, app }) {
  const IndexPage = dynamic({
    app,
    component: () => import('./routes/IndexPage'),
  })

  const Users = dynamic({
    app,
    models: () => [import('./models/users')],
    component: () => import('./routes/Users'),
  })

  return (
    <Router history={history}>
      <Switch>
        <Route exact path="/" component={IndexPage} />
        <Route exact path="/users" component={Users} />
      </Switch>
    </Router>
  )
}

export default RouterConfig
```

## mock

> 模拟数据, 模拟接口api的请求方式

### 在dva中的使用 1 

在mock文件夹下创建

```js
// product.js
module.exports = {
  "GET /api/product":{ "name" : "高粱" }
}

// user.js
module.exports = {
  "POST /api/login": (req, res) => {
  		res.send({
     		msg: "登录成功" 
      })
  	}
}
```

设置.roadhogrc.mock.js

```js
// 1: 一个一个引入
export default {
  ...require("./mock/product"),
 	...require("./mock/product") ,
  ....
}

// 2. 通过fs和path来引入mock文件夹下全部文件引入
const mock = {}
fs.readdirSync(path.join(__dirname + '/mock' )).forEach(function(file){
  if ( file.match(/\.js$/)) {
    Object.assign(mock, require('./mock/' + file ))
  }
})
export default mock
```

在service中定义请求方式

```jsx
import request from '../utils/request';
export function getProduct(){
  return request("/api/product")
}
```



在组件中使用

```jsx
import * as api from '../services/example';

comonentDidMout() {
  api.getProduct().then(res => console.log(res) )
}
```



### 在dva中的使用 2

1. 在根目录的mock目录下新建一个文件’mockData.js’,写入下面代码

```javascript
module.exports = {
  'GET /api/mockData': (request, response) => {
    console.log(request);
    response.send({
      msg: '登陆成功'
    })
  }
}
```

2. 在根目录下的`.roadhogrc.mock.js`文件中进行注册

```javascript
export default {
  ...require("./mock/testMock")
};
```

3. 在根目录下的src下的service目录下写一个请求方法

```javascript
// 注册mock接口
export function mockData() {
    return request('/api/mockData');
}
1234
```

4. 在页面中引入请求方法之后就能进行调用

```javascript
import * as apis from '../service/example'
apis.mockData()
.then((res) => {
  console.log(res);
})
12345
```

### 在umi中的使用

和dva中基本相同,只是不需要第二步

### 使用自己封装的请求方法进行调用(即不使用service)

不需要第3步,将第四步代码替换为

```javascript
fetch('/api/mockData')
.then(res => res.json())
.then(data => console.log(data))
.catch(err => ({ err }));
1234
```

### 续篇(response和request参数的说明)

```javascript
Request
    req.baseUrl 基础路由地址
    req.body post发送的数据解析出来的对象
    req.cookies 客户端发送的cookies数据
    req.hostname 主机地址 去掉端口号
    req.ip 查看客户端的ip地址
    req.ips 代理的IP地址
    req.originalUrl 对req.url的一个备份
    req.params 在使用/:id/:name 匹配params
    req.path 包含请求URL的路径部分
    req.protocol http 或https协议
    req.query 查询字符串解析出来的对象 username=zhangsan&password=123 { username:zhangsan }
    req.route 当前匹配的路由 正则表达式
    req.params 获取路由匹配的参数
    req.get 获取请求header里的参数
    req.is 判断请求的是什么类型的文件
    req.param(key名称) 用来获取某一个路由匹配的参数
 
 
Response
    res.headersSent 查看http响应是否响应了http头
    res.append(名称,value) 追加http响应头
    res.attachment(文件路径) 响应文件请求 
    res.cookie() 设置cookie
    
    res.setHeader('Content-Type','text/html;charset=utf8')
    res.append('Content-Type','text/html;charset=utf8')
    res.append('hehe','1008')
    res.append('haha','1008')
    res.attachment('./xx.zip') //Content-Disposition: attachment; filename="xx.zip"
    res.clearCookie(cookiename) 删除cookie
    res.cookie('zhangsan','lisi') 设置cookie
    res.cookie('zhangsan1','lisi2',{
        maxAge:900000,
        httpOnly:true,
        path: '/admin', 
        secure: true,
        signed:true
    })
    res.clearCookie('zhangsan')
 
    res.download(文件的path路径) 跟attachment类似 用来处理文件下载的 参数是文件地址
    res.end http模块自带的
    res.format()协商请求文件类型 format匹配协商的文件类型
    res.format({
        'text/plain': function(){
            res.send('hey');
        },
        
        'text/html': function(){
            res.send('<p>hey</p>');
        },
        
        'application/json': function(){
            res.send({ message: 'hey' });
        },
        
        'default': function() {
            // log the request and respond with 406
            res.status(406).send('Not Acceptable');
        }
    });
 
    res.get('key') 获取响应header数据
    res.json() 返回json数据 会自动设置响应header Content-type 为json格式 application/json
 
    res.json({
        xx:100
    })
 
    res.json({
        xx:100
    })
 
    jsonp 利用的就是浏览器加载其他服务器的文件不会存在跨域问题
    ajax请求就会有跨域问题
 
    res.setHeader('Content-Type','text/javascript;charsert=utf8')
    res.end(`typeof ${req.query.callback} == 'function' ? ${req.query.callback}({aa:100}):null`)
 
    res.jsonp({aaa:100})
 
 
    重定向 把访问的地址跳转到另一个地址上
    res.redirect(301,'/api/aes')
 
    express jade
    res.render('index',{title:"hehe",test:"23"})
    res.send('') 发送数据 可以是任意类型的数据
    res.sendFile() 发送文件的 
    res.sendStatus(200) 设置发送时的状态码
    res.set('Content-Type', 'text/plain') //设置响应header
    res.status(200) // 设置状态码
    res.type('') // 直接设置响应的文件类型
 
    res.type('pdf')
 
    res.send({aa:100})
    res.end('ok')
    res.end({aa:100})
 
    res.end('你好')
 
 
    res.end(req.get('Accept-Language'))
    res.json({
        is:req.is('text/html')
    })
 
    res.json({
        type:req.baseUrl,
        hostname:req.hostname,
        // ip:req.ip,
        // ips:req.ips,
        // route:req.route,
        ct:req.get('Accept'),
        cs:'22'
    })
```





## model对象

> 5个重要的属性: namespace, state, reducers, effects, subscriptions

#### namespace
> - namespace : model 的命名空间，同时也是他在全局 state 上的属性，只能用字符串，不支持通过`.`的方式创建多层命名空间
>

#### state
> - state : reducer的初始值, 优先级低于传输给dva()的``opts.initialState`
>
>   - ```tsx
>     const app = dva({
>       // 初始化state
>       initialState: { count: 1 },
>     });
>     app.model({
>       namespace: 'count',
>       // 初始化state
>       state: 0,
>     })
>     
>     // 在app.start()后state.count为1
>     ```
>

#### reducers

> - reducers : 以key/ value格式定义reducer, 用于处理同步操作, **唯一可以修改state的地方**
>
> - reducres中的  return 可以覆盖state原有的数据
>
> - 不能实现异步操作
>
>   - 格式: `(state, action) => newState` 或 `[(state, action) => newState, enhancer]`
>
>   - ```jsx
>     namespace: 'todo',
>       state: {
>         list: []
>       },
>       // reducers 写法
>       reducers: {
>         save(state, { payload: { list } }) {
>           return { ...state, list }
>         }
>       }
>     ```



#### effects 

> - 以`key/value` 格式定义effect, 用于处理异步操作的业务逻辑,  
>
> - 可以实现异步操作, 
>
> - **不直接修改state**,可以通过put来触发reducers来修改modul中的state
>
> - 由action触发, 可以触发action, 可以和服务器交互, 可以获取全局state的数据等
>
> - 格式:  `*(action, effects) => void` 或 `[*(action, effects) => void, { type }]`
>   - type类型: 
>     - takeEvery
>     - takeLatest
>     - throttle
>     - watcher
>   - effects
>     - put : 用于触发 action , 常常用于通过触发reducer的方法来修改state
>       - `yield put({ type: 'todos/add', payload: 'Learn Dva'})`
>     - call : 用于调用异步逻辑, 支持Promise
>       - `const result = yield call(fetch, '/todos')`
>       - 第一个参数: 调用的函数
>       - 第二个参数: 你要传输的参数
>     - select : 用于从state里获取数据
>       - `const todos = yield select(state => state.todos)`
>   
> - 错误处理
>
>   - 全局错误处理
>
>     - effects和subscriptions的抛错全部会走onError hook，所以可以在onError里统一处理错误
>
>       ```jsx
>       const app = dva({
>       	onError(e, dispatch) {
>       		console.log(e.message);
>       	},
>       });
>       
>       ```
>
>       
>
>   - 本地错误处理
>
>     - 对某些effects的错误进行特殊处理，需要在effect内部加try catch。
>
>       ```jsx
>       app.model({
>       	effects: {
>       		*addRemote() {
>       			try {
>       				// Your Code Here
>       			} catch(e) {
>       				console.log(e.message);
>       			}
>       		},
>       	},
>       });
>       // try{}catch{}finally{}
>       ```
>
>       

```jsx
// effects 写法
effects: {
    *addTodo({ payload: value }, { call, put, select }) {
      // 模拟网络请求
      const data = yield call(todoService.query, value)
      console.log(data)
      let tempList = yield select(state => state.todo.list)
      let list = []
      list = list.concat(tempList)
      const tempObj = {}
      tempObj.title = value
      tempObj.id = list.length
      tempObj.finished = false
      list.push(tempObj)
      yield put({ type: 'save', payload: { list }})
    },
    *toggle({ payload: index }, { call, put, select }) {
      // 模拟网络请求
      const data = yield call(todoService.query, index)
      let tempList = yield select(state => state.todo.list)
      let list = []
      list = list.concat(tempList)
      let obj = list[index]
      obj.finished = !obj.finished
      yield put({ type: 'save', payload: { list } })
    },
    *delete({ payload: index }, { call, put, select }) {
      const data = yield call(todoService.query, index)
      let tempList = yield select(state => state.todo.list)
      let list = []
      list = list.concat(tempList)
      list.splice(index, 1)
      yield put({ type: 'save', payload: { list } })
    },
    *modify({ payload: { value, index } }, { call, put, select }) {
      const data = yield call(todoService.query, value)
      let tempList = yield select(state => state.todo.list)
      let list = []
      list = list.concat(tempList)
      let obj = list[index]
      obj.title = value
      yield put({ type: 'save', payload: { list } })
    }
  }

// 组件中调用
this.props.dispatch({
	type: '[namespaceName]/[effectsfuntionName]',
  payload: '[要传输的数据]'
})
```

#### subscriptions

> - 主要在model中实现事件监听, 可以监听路由变化, 鼠标, 键盘, 服务器, 状态变化等
> - 以key/vallue格式定义subscriptions, subscriptions是订阅, 用于订阅一个数据源, 然后需要dispatch相应的action
> - 在app.start()被执行时, 数据源可以是当前时候, 服务器的websocket连接, keyboard输入, geolocation变化, history路由变化等
> - 格式`({ dispatch, history } , done ) => unlistenFunction`
> - 取消数据订阅: 使用app.unmode(), subscription必须返回unlisten方法



```jsx
// subscriptions 写法
subscriptions: {
    setup({ dispatch, history }) {
      // 监听路由的变化，请求页面数据
      return history.listen(({ pathname, search }) => {
        const query = queryString.parse(search)
        let list = []
        if (pathname === 'todoList') {
          dispatch({ type: 'save', payload: { list } })
        }
      })
    }
  }
```



路由监听

```jsx
 subscriptions: {
   setup ({dispatch, history}) {
     history.listen(( pathname ) => {
       console.log(pathname)
     })
   }
 },
```

点击事件

```js
 subscriptions: {
     onClick ({dispatch}) {
        document.addEventListener('click',() => {   //这里表示当鼠标点击时就会触发里面的dispatch命令，这里的save就是reducers中的方法名
          dispatch (type:"save")
        })
     }),
 },

```





#### connect

> 将路由的state传输给当前组件



```js
import { connect } from 'dva';
class IndexPage extends React.Component{
  render(){
    console.log(this.props)// 可以拿到state.product里面的数据
    return( 
   		... 	
    )
  }
}

const mapStateProps = (state) => {
  return {
    productList:state.product
  }
}
// 将state.product的数据传输给IndexPage组件
export default connect(mapStateProps)(IndexPage);
```





## router

### withRouter

> 这样子就不用父组件传输过来history对象, `this.props.history`也不会为空了

```js
import { withRouter } from "dva/router";

class Product extends React.Component{
		... 
    this.props.history.push("/");// 实现跳转到主页'/'
    ...
}
export default withRouter(Product);   
```



### routerRedux

```js
import { routerRedux } from 'dva/router';
class Product extends React.Component{
		... 
    this.props.history.dispatch(routerRedux.push("/"));// 实现跳转到主页'/'
    ...
}
export default Product; 

```







## 使用dva框架和直接使用redux写法的区别

#### 用 redux

##### *actions.js 文件*

```tsx
export const REQUEST_TODO = 'REQUEST_TODO';
export const RESPONSE_TODO = 'RESPONSE_TODO';
const request = count => ({type: REQUEST_TODO, payload: {loading: true, count}});
const response = count => ({type: RESPONSE_TODO, payload: {loading: false, count}});
export const fetch = count => {
  return (dispatch) => {
    dispatch(request(count));
    return new Promise(resolve => {
      setTimeout(() => {
        resolve(count + 1);
      }, 1000)
    }).then(data => {
      dispatch(response(data))
    })
  }
}
```

##### *reducer.js 文件*



```dart
import { REQUEST_TODO, RESPONSE_TODO } from './actions';

export default (state = {
  loading: false,
  count: 0
}, action) => {
  switch (action.type) {
    case REQUEST_TODO:
      return {...state, ...action.payload};
    case RESPONSE_TODO:
      return {...state, ...action.payload};
    default:
      return state;
  }
}
```

##### *app.js 文件*



```jsx
import React from 'react';
import { bindActionCreators } from 'redux';
import { connect } from 'react-redux';

import * as actions from './actions';

const App = ({fetch, count, loading}) => {
  return (
    <div>
      {loading ? <div>loading...</div> : <div>{count}</div>}
      <button onClick={() => fetch(count)}>add</button>
    </div>
  )
}

function mapStateToProps(state) {
  return state;
}

function mapDispatchToProps(dispatch) {
  return bindActionCreators(actions, dispatch)
}

export default connect(mapStateToProps, mapDispatchToProps)(App)
```

##### *index.js 文件*



```jsx
import { render } from 'react-dom';
import { createStore, applyMiddleware } from 'redux';
import { Provider } from 'react-redux'
import thunkMiddleware from 'redux-thunk';

import reducer from './app/reducer';
import App from './app/app';

const store = createStore(reducer, applyMiddleware(thunkMiddleware));

render(
  <Provider store={store}>
    <App/>
  </Provider>
  ,
  document.getElementById('app')
)
```

#### 使用dva

##### *model.js 文件*



```tsx
export default {
  namespace: 'demo',
  state: {
    loading: false,
    count: 0
  },
  reducers: {
    request(state, payload) {
      return {...state, ...payload};
    },
    response(state, payload) {
      return {...state, ...payload};
    }
  },
  effects: {
    *'fetch'(action, {put, call}) {
      yield put({type: 'request', loading: true});

      let count = yield call((count) => {
        return new Promise(resolve => {
          setTimeout(() => {
            resolve(count + 1);
          }, 1000);
        });
      }, action.count);

      yield put({
        type: 'response',
        loading: false,
        count
      });
    }
  }
}
```

##### *app.js 文件*



```jsx
import React from 'react'
import { connect } from 'dva';

const App = ({fetch, count, loading}) => {
  return (
    <div>
      {loading ? <div>loading...</div> : <div>{count}</div>}
      <button onClick={() => fetch(count)}>add</button>
    </div>
  )
}

function mapStateToProps(state) {
  return state.demo;
}

function mapDispatchToProps(dispatch) {
  return {
    fetch(count){
      dispatch({type: 'demo/fetch', count});
    }
  }
}

export default connect(mapStateToProps, mapDispatchToProps)(App)
```

##### *index.js 文件*



```jsx
import dva from 'dva';
import model from './model';
import App from './app';
const app = dva();
app.use({});
app.model(model);
app.router(() => <App />);
app.start();
```

我们通过上面两种不同方式来实现一个异步的计数器的代码结构发现：

1. **使用 redux 需要拆分出`action`模块和`reducer`模块**
2. **dva将`action`和`reducer`封装到`model`中，异步流程采用Generator处理**

