---
title: vue-router
date: 2020-09-22 21:56:27
tags:
	- vue
	- router
	- javascript
	- node
	- front-end
---

# vue-rotuer

# vue-router-基础

> - [官网](https://router.vuejs.org/zh/)
>
> - [API](https://router.vuejs.org/zh/api/#router-link)

## router和route的区别

> route为当前router跳转对象里面可以获取name、path、query、params等

> router为VueRouter实例，想要导航到不同URL，则使用router.push方法

## 一个Router

### HTML

> - router-link, router-view, to

```html
<script src="https://unpkg.com/vue/dist/vue.js"></script>
<script src="https://unpkg.com/vue-router/dist/vue-router.js"></script>

<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- 使用 router-link 组件来导航. -->
    <!-- 通过传入 `to` 属性指定链接. -->
    <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
  </p>
  <!-- 路由出口 -->
  <!-- 路由匹配到的组件将渲染在这里 -->
  <router-view></router-view>
</div>
```

### JavaScript

```js
// 0. 如果使用模块化机制编程，导入Vue和VueRouter，要调用 Vue.use(VueRouter)

// 1. 定义 (路由) 组件。
// 可以从其他文件 import 进来
const Foo = { template: '<div>foo</div>' }
const Bar = { template: '<div>bar</div>' }

// 2. 定义路由
// 每个路由应该映射一个组件。 其中"component" 可以是
// 通过 Vue.extend() 创建的组件构造器，
// 或者，只是一个组件配置对象。
// 我们晚点再讨论嵌套路由。
const routes = [
  { path: '/foo', component: Foo },
  { path: '/bar', component: Bar }
]

// 3. 创建 router 实例，然后传 `routes` 配置
// 你还可以传别的配置参数, 不过先这么简单着吧。
const router = new VueRouter({
  routes // (缩写) 相当于 routes: routes
})

// 4. 创建和挂载根实例。
// 记得要通过 router 配置参数注入路由，
// 从而让整个应用都有路由功能
const app = new Vue({
  router
}).$mount('#app')

// 现在，应用已经启动了！
```

### Home.vue

> - 注入路由器后
>   - this.$router : 访问路由器
>   - this.$route : 访问当前路由

```vue
export default {
  computed: {
    username() {
      // 我们很快就会看到 `params` 是什么
      return this.$route.params.username
    }
  },
  methods: {
    goBack() {
      window.history.length > 1 ? this.$router.go(-1) : this.$router.push('/')
    }
  }
}
```

## 动态路由

> - $route.params
> - beforeRouteUpdate
> - this.$route.
> - this.$route.params.pathMatch

### 动态路由匹配

```js
const User = {
	template: 	'<div>{{$route.params.id}}</div>'
}

const router = new VueRouter({
  routes: [
    //动态路径参数 以冒号开头
    //`/user/foo`  和 `/user/bar`  都会映射到相同的路由
    { path: '/user/:id', component: User }
  ]
})
```

> - $route.params
>
>   | 模式                         | 匹配路径            | $route.params                        |
>   | ---------------------------- | ------------------- | ------------------------------------ |
>   | /user/:username              | /user/evan          | { username: 'evan' }                 |
>   | /user/:usrname/post/:post_id | /user/evan/post/123 | { username: 'evan', post_id: '123' } |

### 响应路由参数的变化

> - 当使用路由, 从 /user/foo 导航到 /user/bar , 原来的组件实例会被复用, 比起两个路由都渲染同个组件, 比起销毁再创建, 复用则显得更加高效
>
>   - 但是复用意味着生命周期钩子不会再被调用
>
>   - 复用组件可以使用watch(检测变化) $route 对象
>
>     - ```js
>       const User={
>       	template: `...`,
>         watch: {
>         	$route(to, from) {
>           	//对路由变化做出响应....
>           }
>         }
>       }
>       ```
>
>   - 2.2引入 beforeRouteUpdate 导航守卫
>
>     - ```js
>       const User = {
>       	template: '...',
>         beforeRouteUpdate(to, from, next) {
>           //next() ; 放行
>         }
>       }
>       ```

### 捕获所有路由 或 404 Not found 路由

> - 通配符 *  , pathMatch : 匹配的部分
>
>   - path: '  *  '  匹配所有路径
>
>   - path: ' /user-*' => 匹配 '/user-' 开头的任意路径
>
>   - ```js
>     // 给出一个路由 { path: '/user-*' }
>     this.$router.push('/user-admin')
>     this.$route.params.pathMatch // 'admin'
>     // 给出一个路由 { path: '*' }
>     this.$router.push('/non-existing')
>     this.$route.params.pathMatch // '/non-existing'
>     ```

## 嵌套路由

> - router-view

```html
<div id="app">
  <router-view></router-view>
</div>
```

```js
const User = {
  template: `
	<div class='user'>
		<h2>User {{ $route.params.id }}</h2>
		<router-view></router-view>
	</div>
	`
}

const router = new VueRouter({
  routes: [
    {
      path: '/user/:id', 
      component: User,
      children: [{
        // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
      },{
				 // 当 /user/:id/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
      }]
    }
  ]
})
```

## 编程式的导航

### router.push

> - router.push(lacation, onComplate?, onAbort?) 等同于 <router-link :to='....'>
> - params, query

```js
// 字符串
router.push('home')

// 对象
router.push({ path: 'home' })

// 命名的路由
router.push({ name: 'user', params: { userId: '123' }})

// 带查询参数，变成 /register?plan=private
router.push({ path: 'register', query: { plan: 'private' }})

const userId = '123'
router.push({ name: 'user', params: { userId }}) // -> /user/123
router.push({ path: `/user/${userId}` }) // -> /user/123
// 这里的 params 不生效
router.push({ path: '/user', params: { userId }}) // -> /user
```

### router.replace

> - `router.replace(location, onComplete?, onAbort?)`
> - 替换掉当前history, 不会向history添加新的记录
> - 例如一个用户资料到另一个, /users/1 => /users/2

```js
<router-link :to="..." replace></router-link> 
//等同于
router.replace(...)
```

### router.go(n)

> - 在history记录中向前 或 后退多少步
>   - 类似window.history.go(n)

```js
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)

// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)

// 如果 history 记录不够用，那就默默地失败呗
router.go(-100)
router.go(100)
```

## 命名路由

> - 通过一个名称来标识路由

```js
const router = new VueRouter({
  routes: [
    {
      path: '/user/:userId',
      name: 'user',
      component: User
    }
  ]
})

<router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>

router.push({ name: 'user', params: { userId: 123 }})
```

## 命名视图

> - 同时(同级)展示多个视图, 而且不是嵌套

```html
<router-view class="view one"></router-view>
<router-view class="view two" name="a"></router-view>
<router-view class="view three" name="b"></router-view>
//默认是default
```

```js
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]
})
```

### 嵌套命名视图

```
/settings/emails                                       /settings/profile
+-----------------------------------+                  +------------------------------+
| UserSettings               |                  | UserSettings            |
| +-----+-------------------------+ |                  | +-----+--------------------+ |
| | Nav | UserEmailsSubscriptions | |  +------------>       | | Nav | UserProfile      | |
| |    +-------------------------+ |                  | |    +--------------------+ |
| |    |                 | |                  | |    | UserProfilePreview | |
| +-----+-------------------------+ |                  | +-----+--------------------+ |
+-----------------------------------+                  +------------------------------+
```

> - Nav : 常规组件
> - UserSetting: 视图组件
> - `UserEmailsSubscriptions`,` UserProfile`, `UserProfilePreview` 是嵌套的视图组件

```html
<!-- UserSettings.vue -->
<div>
  <h1>User Settings</h1>
  <NavBar/>
  <router-view/>
  <router-view name="helper"/>
</div>
```

```js
{
  path: '/settings',
  // 你也可以在顶级路由就配置命名视图
  component: UserSettings,
  children: [{
    path: 'emails',
    component: UserEmailsSubscriptions
  }, {
    path: 'profile',
    components: {
      default: UserProfile,
      helper: UserProfilePreview
    }
  }]
}
```

## 重定向和别名

### 重定向

> - /a 重定向到 /b

```js
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: '/b' },
    { path: '/a', redirect: { name: 'foo' }},
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
  ]
})
```



### 别名

> - `/a` 的别名是 `/b`，意味着，当用户访问 `/b` 时，URL 会保持为 `/b`，但是路由匹配则为 `/a`，就像用户访问 `/a` 一样。

```js
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]
})
```

## 路由组件传参

> - $route会使之其对应的路由形成高度耦合, 从而使组件只能在某些URL上是使用, 限制其灵活性

### 取代$route的耦合

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]
})
```

使用props解耦

```js
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```



### 布尔模式

> props被设置成true, route.params将会被设置为组件属性

### 对象模式

> props是一个对象, 他会被按照原样设置为组件属性. 当props 是静态的时候有用

```js
const router = new VueRouter({
  routes: [
    { 
      path: '/promotion/from-newsletter', 
     	 component: Promotion, 
      props: { newsletterPopup: false } 
    }
  ]
})
```



### 函数模式

> 创建一个函数返回props, 这样子就可以将参数转换成另一种类型, 将静态值与基于路由的值结合等

```js
const router = new VueRouter({
	routes: [
    {
      paht: '/search',
      component: SearchUser, 
      props: route => ({ query: route.query.q })
    }
  ]
})
// 当URL 为/search?q=vue 会将{ query; 'vue' } 作为属性传递给 SearchUser 组件
```

## HTML5 History 模式

> - vue-router 默认hash模式 : 模拟一个完整的URL , 当URL改变时, 页面不后悔重新加载,
> - history 模式, URL就像正常的url, 例如 http://yoursite.com/user/id
>   - 需要后台正确的配置

```js
const router = new VueRouter({
  mode: 'history',
  routes: [
    { path: '*', component: NotFoundComponent }
  ]
})
```

# vue-rotuer-进阶

## 导航守卫

> - 参数或查询的改变不会进去或离开导航守卫
>   - 可以通过观察$route对象来应对这些变化, 或beforeRouteUpdate 的组件类守卫



### 全局前置守卫

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

> - to : Route : 即将要进入的目标
> - from : Rotue :  当前导航正要离开的路由
> - next: Function : 
>   - next() : 进行管道中的下一个钩子. 如果全部钩子执行网, 则导航状态就是confirmed( 确认 )
>   - next( false ): 中断当前的导航. 如果浏览器的URL改变了, 可能是用户手动或者浏览器后退按钮, URL地址会重置到from 路由对应的地址
>   - next('/') 或 next({ path: '/' })  : 跳转到一个不同的地址, 当前的导航被中断, 然后进行一个新的导航
>     - 且可以设置replace: true, name: 'home'等
>   - next(error) : (2.4.0+) 如果传入next 的参数是一个Error 实例, 则导航会被终止且该错误会被传递给rotuer.onError()注册过的回调

```js
router.beforeEach((to, from, next) => {
	if ( to.name !== 'Login' && !isAuthenticated ) next({ name: 'Login '}) 
	//如果用户未能验证用户 , 则 next 会被调用两次
  next()
})
```

### 全局解析守卫

> 2.3.0+
>
> - router.beforeResolve 注册一个全局守卫, 
>   - 和router.beforeEach类似
>   - 区别
>     - 在导航确认之前, 同时在所有组件内守卫和异步路由组件被解析之后, 解析守卫就被调用

### 全局后置钩子

```js
router.afterEach((to, from) =>{
	//	...
})  
```

### 路由独享的守卫

> 直接在路由配置直接定义beforeEnter守卫

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```



### 组件内的守卫

> - beforeRouteEnter
> - beforeRouteUpdate (2.2+)
> - beforeRouteLeave

```js
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 可以通过next(vm => {/* 通过`vm` 访问实例 */})
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}
```

### 完整导航解析流程

> 1. 导航被触发
> 2. 在失活的组件调用 beforeRouteLeave 守卫
> 3. 调用全局的 beforeEach 守卫
> 4. 在重用的组件里面调用 beforeRouteUpdate 守卫 (2.2+)
> 5. 在路由配置里调用 beforeEnter
> 6. 解析异步路由组件
> 7. 在被激活的组件里面调用 beforeRouteEnter
> 8. 调用全局的 beforeResolve 守卫(2.5+)
> 9. 导航被确认
> 10. 调用全局afterEach 钩子
> 11. 触发DOM 更新
> 12. 调用 beforeRouteEnter 守卫中传给 next 的回调函数, 创建好的组件实例会作为调用函数的参数传入

## 路由元信息

> 定义路由的时候可以通过配置meta字段

```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { requiresAuth: true }
        }
      ]
    }
  ]
})
```



```js
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})
```



## 过渡特效

```html
//使用动态的 transition name
<transition :name="transitionName">
  <router-view></router-view>
</transition>

// 接着在父组件内
// watch $route 决定使用哪种过渡
watch: {
  '$route' (to, from) {
    const toDepth = to.path.split('/').length
    const fromDepth = from.path.split('/').length
    this.transitionName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
  }
}
```

## 数据获取

### 导航完成后获取数据

```html
<template>
  <div class="post">
    <div v-if="loading" class="loading">
      Loading...
    </div>

    <div v-if="error" class="error">
      {{ error }}
    </div>

    <div v-if="post" class="content">
      <h2>{{ post.title }}</h2>
      <p>{{ post.body }}</p>
    </div>
  </div>
</template>
```



```js
export default {
  data () {
    return {
      loading: false,
      post: null,
      error: null
    }
  },
  created () {
    // 组件创建完后获取数据，
    // 此时 data 已经被 observed 了
    this.fetchData()
  },
  watch: {
    // 如果路由有变化，会再次执行该方法
    '$route': 'fetchData'
  },
  methods: {
    fetchData () {
      this.error = this.post = null
      this.loading = true
      // replace getPost with your data fetching util / API wrapper
      getPost(this.$route.params.id, (err, post) => {
        this.loading = false
        if (err) {
          this.error = err.toString()
        } else {
          this.post = post
        }
      })
    }
  }
}
```

### 导航完成前获取数据



```js
export default {
  data () {
    return {
      post: null,
      error: null
    }
  },
  beforeRouteEnter (to, from, next) {
    getPost(to.params.id, (err, post) => {
      next(vm => vm.setData(err, post))
    })
  },
  // 路由改变前，组件就已经渲染完了
  // 逻辑稍稍不同
  beforeRouteUpdate (to, from, next) {
    this.post = null
    getPost(to.params.id, (err, post) => {
      this.setData(err, post)
      next()
    })
  },
  methods: {
    setData (err, post) {
      if (err) {
        this.error = err.toString()
      } else {
        this.post = post
      }
    }
  }
}
```





## 滚动行为

> - 只支持history.pushState 的浏览器中可用

```js
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // savePosition 当且仅当 popsate导航到 (浏览器 的前进/后退 按钮触发) 时才可用
    // return 期望滚动到哪个的位置 
       //{ x: number, y: number }
				//{ selector: string, offset? : { x: number, y: number }} (offset 只在 2.6.0+ 支持)
  }
})
```



```js
//异步滚动
scrollBehavior (to, from, savedPosition) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({ x: 0, y: 0 })
    }, 500)
  })
}
```



## 路由懒加载

> - 当路由被访问的时候才加载对应组件, 这样子就更加高效

结合Vue的异步组件和Webpack 的代码分割功能

```
const Foo =() => Promise.resolve({ /* 组件定义对象 */});
//Webpack 2中, 动态import 语法来一定代码分块点(split point)
import('./Foo.vue')
```

> 注: 使用Babel 需要添加syntax-dynamic-import 插件, 才能使 Babel 可以正确地解析语法



```
const Foo = ()  => import('./Foo.vue')


const router = new VueRouter({
	routes: [
		{ path: '/foo ', component: Foo }
	]
})
```

### 把组件按组分块

有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用 [命名 chunk](https://webpack.js.org/guides/code-splitting-require/#chunkname)，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)。

```js
const Foo = () => import(/* webpackChunkName: "group-foo" */ './Foo.vue')
const Bar = () => import(/* webpackChunkName: "group-foo" */ './Bar.vue')
const Baz = () => import(/* webpackChunkName: "group-foo" */ './Baz.vue')
```

Webpack 会将任何一个异步模块与相同的块名称组合到相同的异步块中。

