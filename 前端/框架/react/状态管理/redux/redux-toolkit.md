# Redux Toolkit

> 简化 `redux` 的使用

## Api

> - `configureStore() `
>   提供简化的配置选项和良好的默认值。它可以自动组合众多的reducers，添加用户提供的任何Redux中间件，默认情况下包括Redux -thunk(处理异步Action的中间件)，并支持使用Redux DevTools扩展。
> - `createReducer() `
>   创建reducer的action映射表而不必编写switch语句。自动使用immer库让你用正常的代码编写更简单的不可变更新，比如state.todos[3].completed = true。
> - `createAction() `
>   为给定的操作类型字符串生成action creator函数
> - `createSlice() `
>   根据传递的参数自动生成相应的actionCreator和reducer函数
> - `createAsyncThunk() `
>   接受action字符串和返回Promise的函数，并生成分派的thunk函数
> - `createEntityAdapter `
>   生成可重用的reducers和selectors来管理store中的数据, 执行CRUD操作
> - `createSelector() `
>   来自reselect库，被重新导出，用于state缓存，防止不必要的计算

## 安装使用

> `npx create-react-app my-app --template redux`

### 配置组件和redux的热重载

```js
import { configureStore } from '@reduxjs/toolkit'

import rootReducer from './rootReducer'

const store = configureStore({
  reducer: rootReducer
})

if (process.env.NODE_ENV === 'development' && module.hot) {
  module.hot.accept('./rootReducer', () => {
    const newRootReducer = require('./rootReducer').default
    store.replaceReducer(newRootReducer)
  })
}

export type AppDispatch = typeof store.dispatch

export default store
```



### 组件树热重载

```js
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from 'react-redux'

import store from './app/store'

import './index.css'

// 这里把树根节点的渲染逻辑提取到render函数中以在webpack检测到文件改变之后进行热更新
const render = () => {
  const App = require('./app/App').default

  ReactDOM.render(
    <Provider store={store}>
      <App />
    </Provider>,
    document.getElementById('root')
  )
}

render()

if (process.env.NODE_ENV === 'development' && module.hot) {
  module.hot.accept('./app/App', render)
}
```



### 使用`useSelector() `和`useDispatch() ` Hook来替代`connect() `

> - 传统的react应用在与redux进行连接时候是通过react-redux库的connect函数来传入mapState和mapDispatch函数来将redux中的state和action存储到组件的props中。
> - react-redux新版已经支持useSelector, useDispatch Hook， 我们可以使用它们替代connect的写法。通过它们我们可以在纯函数组件中获取到store中的值并做到监测变化

```js
import React, { useState } from "react";
import { useSelector, useDispatch } from "react-redux";
import {
  decrement,
  increment,
  incrementByAmount,
  incrementAsync,
  selectCount,
} from "./counterSlice";
import styles from "./Counter.module.css";

export default function Counter() {
  const count = useSelector(selectCount);
  const dispatch = useDispatch();
  const [incrementAmount, setIncrementAmount] = useState("2");

  return (
    <div>
      <div className={styles.row}>
        <button
          className={styles.button}
          aria-label="Increment value"
          onClick={() => dispatch(increment())}
        >
          +
        </button>
        <span className={styles.value}>{count}</span>
        <button
          className={styles.button}
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          -
        </button>
      </div>
      <div className={styles.row}>
        <input
          className={styles.textbox}
          aria-label="Set increment amount"
          value={incrementAmount}
          onChange={(e) => setIncrementAmount(e.target.value)}
        />
        <button
          className={styles.button}
          onClick={() =>
            dispatch(incrementByAmount(Number(incrementAmount) || 0))
          }
        >
          Add Amount
        </button>
        <button
          className={styles.asyncButton}
          onClick={() => dispatch(incrementAsync(Number(incrementAmount) || 0))}
        >
          Add Async
        </button>
      </div>
    </div>
  );
}
```

### 使用useEffect Hook来执行异步逻辑



```js
export const IssuesListPage = ({
  org,
  repo,
  page = 1,
  setJumpToPage,
  showIssueComments
}: ILProps) => {
  const [issuesResult, setIssues] = useState<IssuesResult>({
    pageLinks: null,
    pageCount: 1,
    issues: []
  })
  const [numIssues, setNumIssues] = useState<number>(-1)
  const [isLoading, setIsLoading] = useState<boolean>(false)
  const [issuesError, setIssuesError] = useState<Error | null>(null)

  const { issues, pageCount } = issuesResult

  useEffect(() => {
    async function fetchEverything() {
      async function fetchIssues() {
        const issuesResult = await getIssues(org, repo, page)
        setIssues(issuesResult)
      }  
async function fetchIssueCount() {
    const repoDetails = await getRepoDetails(org, repo)
    setNumIssues(repoDetails.open_issues_count)
  }

  try {
    await Promise.all([fetchIssues(), fetchIssueCount()])
    setIssuesError(null)
  } catch (err) {
    console.error(err)
    setIssuesError(err)
  } finally {
    setIsLoading(false)
  }
}

setIsLoading(true)

fetchEverything()
  }, [org, repo, page])

  // omit rendering
}
```


### createAsyncThunk的使用

> 参数
> 		rtk提供的生成thunk action creator的工具函数
> 参数:
>
> 1. type: actionType字符串(如users/requestStatus)， rtk会会基于此生成以下三个action creator	
>
>    `fulfilled: 'users/requestStatus/fulfilled' `
>
>    `rejected: 'users/requestStatus/rejected' `		
>
>    `pending: 'users/requestStatus/pending`	
>
> - payloadCreator
>   一个回调函数，它应该返回一个包含一些异步逻辑结果的promise
>   payloadCreator的参数有两个:
>
>   - arg
>     dispatch thunk action creator 时候参入的参数值，如ids等需要参与AJAX的值
>
>   - thunkAPI对象
>     一个对象，包含通常传递给Redux thunk函数的所有参数，以及其他选项
>     - dispatch store的dispatch函数
>     - getState store的getState函数
>     - extra 调用configureStore配置store时候传递给thunk middleware的额外参数
>     - requestId当次请求的唯一表示串
>     - signal取消标志， 如果应用有其他地方标记这个请求应该取消则为true
>     - rejectWithValue工具函数, 用于返回一个可以自定义payload被reject的Promise
>
>     
>
> - options对象
>
>   - condition: 一个回调，如果需要，可用于跳过payload creator函数逻辑执行
>   - dispatchConditionRejection： 如果condition()返回false，则默认行为是根本不分派任何动作。如果您仍然希望在thunk被取消时发送一个“rejected”操作，将此标志设置为true。







### createAsyncThunk函数的返回值

返回一个标准的Redux thunk action creator。thunk动作创建器函数将为pending, fulfilled, rejected情况提供普通action creator，并将其作为嵌套字段附加。
如上面的fetchUserById例子:
通过调用createAsyncThunk会生成四对action, action creator

fetchUserById.pending，一个action creator，它分派一个’users/fetchByIdStatus/pending’操作
fetchUserById.fulfilled 一个分派’users/fetchByIdStatus/ fulfilled’动作的action creator
fetchUserById.rejected: 一个分派’users/fetchByIdStatus/rejected’动作的action creator
要在reducer中处理这些action，请使用对象键表示法或“构建器回调”表示法引用createReducer或createSlice中的action creator。

```js
const reducer1 = createReducer(initialState, {
  [fetchUserById.fulfilled]: (state, action) => {}
})

const reducer2 = createReducer(initialState, builder => {
  builder.addCase(fetchUserById.fulfilled, (state, action) => {})
})

const reducer3 = createSlice({
  name: 'users',
  initialState,
  reducers: {},
  extraReducers: {
    [fetchUserById.fulfilled]: (state, action) => {}
  }
})

const reducer4 = createSlice({
  name: 'users',
  initialState,
  reducers: {},
  extraReducers: builder => {
    builder.addCase(fetchUserById.fulfilled, (state, action) => {})
  }
})
```



### 处理thunk的返回结果

调用thunks时可能返回一个值。一个常见的用例是:从thunk返回一个promise，从组件中分派thunk，然后等待promise被解析，然后再做额外的工作:

```js
const onClick = () => {
  dispatch(fetchUserById(userId)).then(() => {
    // do additional work
  })
}
```


由createAsyncThunk生成的thunks将总是返回一个已解析的承诺，其中包含已实现的操作对象或被拒绝的操作对象，视情况而定。

调用逻辑可能希望将这些操作视为最初的promise内容。redux toolkit导出一个unwrapResult函数，该函数可用于从操作中提取负载或错误，并适当地返回或抛出结果

```js
import { unwrapResult } from '@reduxjs/toolkit'

// in the component
const onClick = () => {
  dispatch(fetchUserById(userId))
    .then(unwrapResult)
    .then(originalPromiseResult => {})
    .catch(serializedError => {})
}
```


如果您需要定制被reject操作的内容，您应该自己捕获任何错误，然后使用thunkAPI返回的rejectWithValue。执行return rejectWithValue(errorPayload)将导致被reject的操作将该值作为action.payload使用

```js
const updateUser = createAsyncThunk(
  'users/update',
  async (userData, { rejectWithValue }) => {
    const { id, ...fields } = userData
    try {
      const response = await userAPI.updateById(id, fields)
      return response.data.user
    } catch (err) {
      // Use `err.response.data` as `action.payload` for a `rejected` action,
      // by explicitly returning it using the `rejectWithValue()` utility
      return rejectWithValue(err.response.data)
    }
  }
)
```



### 请求的取消

#### 请求前取消

如果您需要在调用负载创建器之前取消一个thunk，您可以在负载创建器之后提供一个条件回调选项。回调函数将接收thunk参数和一个带有{getState, extra}的对象作为参数，并使用它们来决定是否继续。如果执行应该被取消，条件回调函数应该返回false

```js
const fetchUserById = createAsyncThunk(
  'users/fetchByIdStatus',
  async (userId, thunkAPI) => {
    const response = await userAPI.fetchById(userId)
    return response.data
  },
  {
    condition: (userId, { getState, extra }) => {
      const { users } = getState()
      const fetchStatus = users.requests[userId]
      if (fetchStatus === 'fulfilled' || fetchStatus === 'loading') {
        // Already fetched or in progress, don't need to re-fetch
        return false
      }
    }
  }
)
```

#### 请求时取消

如果你想在它完成之前取消运行的thunk，你可以使用dispatch返回的promise的abort方法

```js
import { fetchUserById } from './slice'
import { useAppDispatch } from './store'
import React from 'react'

function MyComponent(props) {
  const dispatch = useAppDispatch()
  React.useEffect(() => {
    // Dispatching the thunk returns a promise
    const promise = dispatch(fetchUserById(props.userId))
    return () => {
      // `createAsyncThunk` attaches an `abort()` method to the promise
      promise.abort()
    }
  }, [props.userId])
}
```


使用thunkAPI.signal取消网络请求
现代浏览器的fetch api已经提供了对中止信号的支持

```js
import { createAsyncThunk } from '@reduxjs/toolkit'

const fetchUserById = createAsyncThunk(
  'users/fetchById',
  async (userId, thunkAPI) => {
    const response = await fetch(`https://reqres.in/api/users/${userId}`, {
      signal: thunkAPI.signal,
    })
    return await response.json()
  }
)
```

#### 检查取消状态

你可以用这个中止属性定期检查thunk是否已中止，并在这种情况下停止代价高昂的长时间运行的工作



```js
import { createAsyncThunk } from '@reduxjs/toolkit'

const readStream = createAsyncThunk(
  'readStream',
  async (stream, { signal }) => {
    const reader = stream.getReader()
let done = false
let result = ''

while (!done) {
  if (signal.aborted) {
    throw new Error('stop the work, this has been aborted!')
  }
  const read = await reader.read()
  result += read.value
  done = read.done
  }
  return result
  }
)
```


你也可以调用signal.addEventListener(‘abort’， callback)在调用promise.abort()时通知thunk内部的逻辑。例如，这可以与axios CancelToken一起使用

```js
import { createAsyncThunk } from '@reduxjs/toolkit'
import axios from 'axios'

const fetchUserById = createAsyncThunk(
  'users/fetchById',
  async (userId, { signal }) => {
    const source = axios.CancelToken.source()
    signal.addEventListener('abort', () => {
      source.cancel()
    })
    const response = await axios.get(`https://reqres.in/api/users/${userId}`, {
      cancelToken: source.token,
    })
    return response.data
  }
)
```



### createEntityAdapter

生成一组预构建的reducer和selector的函数，用于对包含特定类型数据对象实例的规范化状态结构执行CRUD操作。这些reducer函数可以作为case reducer传递给createReducer和createSlice。它们也可以作为createReducer和createSlice内部的“突变”助手函数
实例

```js
import {
  createEntityAdapter,
  createSlice,
  configureStore
} from '@reduxjs/toolkit'

// Since we don't provide `selectId`, it defaults to assuming `entity.id` is the right field
const booksAdapter = createEntityAdapter({
  // Keep the "all IDs" array sorted based on book titles
  sortComparer: (a, b) => a.title.localeCompare(b.title)
})

const booksSlice = createSlice({
  name: 'books',
  initialState: booksAdapter.getInitialState({
    loading: 'idle'
  }),
  reducers: {
    // Can pass adapter functions directly as case reducers.  Because we're passing this
    // as a value, `createSlice` will auto-generate the `bookAdded` action type / creator
    bookAdded: booksAdapter.addOne,
    booksLoading(state, action) {
      if (state.loading === 'idle') {
        state.loading = 'pending'
      }
    },
    booksReceived(state, action) {
      if (state.loading === 'pending') {
        // Or, call them as "mutating" helpers in a case reducer
        booksAdapter.setAll(state, action.payload)
        state.loading = 'idle'
      }
    },
    bookUpdated: booksAdapter.updateOne
  }
})

const {
  bookAdded,
  booksLoading,
  booksReceived,
  bookUpdated
} = booksSlice.actions

const store = configureStore({
  reducer: {
    books: booksSlice.reducer
  }
})

// Check the initial state:
console.log(store.getState().books)
// {ids: [], entities: {}, loading: 'idle' }

const booksSelectors = booksAdapter.getSelectors(state => state.books)

store.dispatch(bookAdded({ id: 'a', title: 'First' }))
console.log(store.getState().books)
// {ids: ["a"], entities: {a: {id: "a", title: "First"}}, loading: 'idle' }

store.dispatch(bookUpdated({ id: 'a', changes: { title: 'First (altered)' } }))
store.dispatch(booksLoading())
console.log(store.getState().books)
// {ids: ["a"], entities: {a: {id: "a", title: "First (altered)"}}, loading: 'pending' }

store.dispatch(
  booksReceived([
    { id: 'b', title: 'Book 3' },
    { id: 'c', title: 'Book 2' }
  ])
)

console.log(booksSelectors.selectIds(store.getState()))
// "a" was removed due to the `setAll()` call
// Since they're sorted by title, "Book 2" comes before "Book 3"
// ["c", "b"]

console.log(booksSelectors.selectAll(store.getState()))
// All book entries in sorted order
// [{id: "c", title: "Book 2"}, {id: "b", title: "Book 3"}]
```

