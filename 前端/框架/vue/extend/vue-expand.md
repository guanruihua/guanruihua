---
title: vue-expand
date: 2020-09-22 21:49:35
tags:
	- vue
	- expand
	- javascript
	- front-end
---



# vue-expand

## 设计项目结构



> 1. 路由层 `router` 
> 2. 静态文件层 `assets` 
> 3. 页面结构层 `views` 
> 4. 组件结构层 `components` 
> 5. 全局状态管理层 `store` 
> 6. 功能逻辑处理层 `util` 
> 7. 常量管理层 `constants` 
> 8. 在 `Vue` 项目中还可以引入更多的配置如混入层 `mixins` 、过滤层 `filtters` 等。



### 数据请求 methods中 OR actions中

一般项目中对于数据请求的方式都是基于 `methods` 钩子或其他生命周期钩子中调用请求方法，也存在一些项目中是通过发送一个 `disptach` 异步请求方法在 `actions` 中调用请求函数。使用后者的说法是便于统一管理请求接口，并对请求返回的数据进行统一的管理。
综合以上两种做法，可以优化项目中的请求方式，若请求接口发出后返回的数据需要在多个页面或多个不同的组件中共享和使用，则推荐在需要发请求的函数中 `dispath` 触发，在 `actions` 中发送请求，返回的数据保存在全局状态管理 `state` 中。
`methods` 中发送请求方式：

```javascript
getGraphicCode () {
  let vm = this;
  api.login.getCheckCode({
    type: '2'
  }).then(res => {
    if (res.code === '000') {
      vm.graphicCode = 'data:image/png;base64,' + res.data.img;
      vm.imgId = res.data.imgId;
    } else {
      vm.$message.error(res.msg);
    }
  })
}
```

`actions` 中发送请求方式：

```javascript
findAllRoles({ commit }) {
  return new Promise((resolve, reject) => {
    api.systemAccount.findAllRoles().then(response => {
      if (response.code === "000" && response.success) {
        commit(MUTATIONS_TYPE.AllROLES, response.data)
        resolve(response);
      } else {
        reject(new Error(response.code, response.msg))
      }
    })
  })
},
```



### 登录与权限管理

`token` 验证是目前大部分前后端分离的 `Web` 项目做登录验证比较常见的方法。前端通过发送账号和密码或账号和验证码给到后端后，后端验证通过会返回一个唯一的 `token` 作为该用户的登录凭证，在之后的每个请求当中，请求头中都需带上这个 `token` 作为后端的登录校验。 `token` 有过期的机制，可以在请求拦截中做逻辑判断处理，若当前时间接近了过期时间，则通过更新 `token` 的接口请求更新 `token` ，在之后的请求中带上新的 `token` 。以此循环，若用户过长时间无操作，则可认为用户为离线状态，在用户之后的第一次请求时，由于 `token` 已经过期，访问后端接口会发生错误，根据后端返回的错误状态码作为判断，将系统定向至登录页面。
通过带有 `token` 请求头的请求方法，后端可以判断到是哪一个用户，前端也可以通过获取权限接口获得该用户的权限列表，根据权限列表做一份路由映射表，如果后端返回的数据结构与前端的路由设置的数据结构不同，此时还需编写此映射路由的业务功能函数。如果该用户拥有此路由权限，则通过在全局路由监控中 `router.beforeEach` 进行 `router` 中的 `addRoutes` 方法将有权限的路由配置添加到路由当中，侧边栏也可根据路由列表中的 `meta` 字段中关键字的判断进行相应的渲染。如果权限的颗粒度小到一个按钮，则可根据后端返回的权限列表映射出的权限参数，通过 `v-if` 进行判断该功能组件是否渲染。
在路由管理中通过 `router.beforeEach` 钩子中判断当前的路由权限是否为空，是的话则可执行获取权限路由的接口：

```javascript
store.dispatch("getUserInfoAndAuthorityInfo").then(res => {
  // 根据后端返回的路由权限格式转成前端路由配置格式
  const rolesRoute = setAsyncRouterMap(res.menuList, asyncRouterMap, mainChildrenAsyncRoutes)
  store.commit(Vue.VUEX_TYPES.ROLESROUTE, rolesRoute);
  // 添加路由
  router.addRoutes(rolesRoute);
  next({ ...to })
}).catch(() => {
  Message.error("验证失败")
  next('/login')
})
```



### 常量枚举值管理

在项目当中对关键的常量枚举值进行管理是非常有必要的。比如在项目当中后端用某个状态码 `1` 代表账号为启用状态，如果在项目当中多次使用 `===1` 去判断账号是否为启用状态，当需要更改这个状态码的时候，对于前端来说是一件十分麻烦的事情，所以可以通过把 `1` 赋值给一个常量，在项目代码中引用这个常量，如果需要更改状态码的时候，则直接改变这个赋值给常量枚举值的状态码即可，常量的配置也可提醒开发人员此参数不可轻易修改，便于项目的维护和统一管理。一般常量枚举值的管理写在 `constants` 层中，常量的变量名使用大写字母编写。
状态枚举值得配置如下：

```javascript
/**
 * 账号状态对照表
 * "0" 未启用 NOTUSED_CODE
 * "1" 已启用 ENABLE_CODE
 * "2" 已停用 DISABLE_CODE
 */

const NOTUSED_CODE = "0";
const ENABLE_CODE = "1";
const DISABLE_CODE = "2";

const ACCOUNT_TYPE = {
  [NOTUSED_CODE]: "未启用",
  [ENABLE_CODE]: "已启用",
  [DISABLE_CODE]: "已停用"
};

export default Object.freeze({
  NOTUSED_CODE,
  ENABLE_CODE,
  DISABLE_CODE,
  ACCOUNT_TYPE
});
```



### 组件设计

前端项目当中可以把展示组件分为两部分，分别为页面组件和功能组件。对于页面组件，常用于展现页面的整体内容，承担着业务逻辑的正常运行，与业务比较有强的耦合性。功能组件是用于展现和处理某一单一或某一模块的功能，功能组件并不关心页面的业务逻辑，充当着一个函数的作用，只要有输入便有对应的输出，并可在多个页面组件或功能组件中被调用。综上，在设计页面组件的时候，不仅应该考虑该组件能够正常的完成业务的功能，还要考虑其是否能够脱离业务成为一个功能组件，对于内容比较多的页面组件，可以在其同级目录下新建多个子页面组件共同构建。在设计功能组件时，需考虑组件的布局、逻辑、视图，功能组件的设计难度在于其要考虑到满足不断更新的需求变化，可扩展性，灵活性是设计的一大挑战。
页面组件目录格式如下：
![image.png](https:////p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/22a3e0791dd74af5838f4359dca00746~tplv-k3u1fbpfcp-zoom-1.image)

### 必要的开发文档或注释

项目的开发文档可编写为md文件格式存放于项目的根目录，一份好的开发文档能够对项目的背景进行介绍，说明项目的结构和开发的步骤，更有利于其他开发人员参与或接手项目。对于项目当中使用到的与业务功能耦合的逻辑函数，较为复杂的，编写函数的介绍以及使用方法，做好边界条件判断，示范输入数据以及对应的输出结果，可在项目中新建 `docs` 文件夹存放开发过程中的使用文档。对于非复杂功能的业务逻辑函数或非业务逻辑函数，可直接在定义函数之前编写注释，说明函数作用功能，以及对应的输入和输入参数的类型。

> 每次的开发过程都可当做是一个学习和总结经验的过程，对比以往的代码，我们可以思考代码结构是否能设计得更加完善，逻辑函数是否清晰且考虑边界条件，性能是否可以更加的优化。



## 常用webpack配置

### **vue-lic3脚手架（vue.config.js）**

#### publicPath

类型：String

默认：'/'

部署应用包时的基本 URL。默认情况下，Vue CLI会假设你的应用是被部署在一个域名的根路径上，例如https://www.my-app.com/。如果应用被部署在一个子路径上，你就需要用这个选项指定这个子路径。例如，如果你的应用被部署在https://www.my-app.com/my-app/，则设置publicPath为/my-app/

这个值也可以被设置为空字符串 ('') 或是相对路径 ('./')，这样所有的资源都会被链接为相对路径，这样打出来的包可以被部署在任意路径，也可以用在类似 Cordova hybrid 应用的文件系统中。

#### productionSourceMap

类型：boolean

moren：true

不允许打包时生成项目来源映射文件，在生产环境下可以显著的减少包的体积

> 注 Source map的作用：针对打包后的代码进行的处理，就是一个信息文件，里面储存着位置信息。也就是说，转换后的代码的每一个位置，所对应的转换前的位置。有了它，出错的时候，除错工具将直接显示原始代码，而不是转换后的代码。这无疑给开发者带来了很大方便

#### assetsDir

放置生成的静态资源 (js、css、img、fonts) 的 (相对于 outputDir 的) 目录,默认是'',

#### indexPath

指定生成的 index.html 的输出路径(相对于outputDir)。也可以是一个绝对路径。默认是'index.html'

#### lintOnSave

是否在每次保存时使用eslint检查，这个对语法的要求比较严格，对自己有要求的同学可以使用

#### css

```js
css: {
    //是否启用css分离插件，默认是true，如果不启用css样式分离插件，打包出来的css是通过内联样式的方式注入至dom中的，
    extract: true,
    sourceMap: false,//效果同上
    modules: false,// 为所有的 CSS 及其预处理文件开启 CSS Modules。
    // 这个选项不会影响 `*.vue` 文件。
  },
```

#### devServer

本地开发服务器配置

```js
devServer: { 
    //配置开发服务器
    host: "0.0.0.0",
    //是否启用热加载，就是每次更新代码，是否需要重新刷新浏览器才能看到新代码效果
    hot: true,
    //服务启动端口
    port: "8080",
    //是否自动打开浏览器默认为false
    open: false,
    //配置http代理
    proxy: { 
      "/api": { //如果ajax请求的地址是http://192.168.0.118:9999/api1那么你就可以在jajx中使用/api/api1路径,其请求路径会解析
        // http://192.168.0.118:9999/api1，当然你在浏览器上开到的还是http://localhost:8080/api/api1;
        target: "http://192.168.0.118:9999",
        //是否允许跨域，这里是在开发环境会起作用，但在生产环境下，还是由后台去处理，所以不必太在意
        changeOrigin: true,
        pathRewrite: {
            //把多余的路径置为''
          "api": ""
        }
      },
      "/api2": {//可以配置多个代理，匹配上那个就使用哪种解析方式
        target: "http://api2",
        // ...
      }
    }
},
```

#### pluginOptions

这是一个不进行任何 schema 验证的对象，因此它可以用来传递任何第三方插件选项，例如：

```js
{
    //定义一个全局的less文件，把公共样式变量放入其中，这样每次使用的时候就不用重新引用了
    'style-resources-loader': {
      preProcessor: 'less',
      patterns: [
        './src/assets/public.less'
      ]
    }
}
```

#### chainWebpack

是一个函数，会接收一个基于 webpack-chain 的 ChainableConfig 实例。允许对内部的 webpack 配置进行更细粒度的修改。例如：

```js
chainWebpack(config) { 
//添加一个路径别名 假设有在assets/img/menu/目录下有十张图片，如果全路径require("/assets/img/menu/img1.png")
//去引入在不同的层级下实在是太不方便了，这时候向下方一样定义一个路劲别名就很实用了
    config.resolve.alias
      //添加多个别名支持链式调用
      .set("assets", path.join(__dirname, "/src/assets"))
      .set("img", path.join(__dirname, "/src/assets/img/menu"))
      //引入图片时只需require("img/img1.png");即可
}
```

## 组件传值

#### 父传子

通过props传递

```
父组件： <child value = '传递的数据' />

子组件: props['value'],接收数据,接受之后使用和data中定义数据使用方式一样
```

#### 子传父

在父组件中给子组件绑定一个自定义的事件，子组件通过$emit()触发该事件并传值。

```
父组件： <child @receive = 'receive' />

 子组件: this.$emit('receive','传递的数据')
```

#### 兄弟组件传值

- 通过中央通信 let bus = new Vue()

> A：methods :{ 函数{bus.$emit(‘自定义事件名’，数据)} 发送

> B：created （）{bus.$on(‘A发送过来的自定义事件名’，函数)} 进行数据接收

- 通过vuex



## **v-for key的作用**

当Vue用 v-for 正在更新已渲染过的元素列表是，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue将不是移动DOM元素来匹配数据项的改变，而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。

为了给Vue一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性。key属性的类型只能为 string或者number类型。

key 的特殊属性主要用在Vue的虚拟DOM算法，在新旧nodes对比时辨识VNodes。如果不使用 key，Vue会使用一种最大限度减少动态元素并且尽可能的尝试修复/再利用相同类型元素的算法。使用key，它会基于key的变化重新排列元素顺序，并且会移除 key 不存在的元素。

## **Vue的双向数据绑定原理**

vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。主要分为以下几个步骤：

> 1、需要observe的数据对象进行递归遍历，包括子属性对象的属性，都加上setter和getter这样的话，给这个对象的某个值赋值，就会触发setter，那么就能监听到了数据变化

> 2、compile解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图

> 3、Watcher订阅者是Observer和Compile之间通信的桥梁，主要做的事情是: ①在自身实例化时往属性订阅器(dep)里面添加自己 ②自身必须有一个update()方法 ③待属性变动dep.notice()通知时，能调用自身的update()方法，并触发Compile中绑定的回调，则功成身退。

> 4、MVVM作为数据绑定的入口，整合Observer、Compile和Watcher三者，通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果。





## vue-loader

> vue文件加载器, 跟template/js/style转换为js模块



## Vue-typeScript

### 环境搭建

```shell
vue create typescript-app & cd typescript-app & npm run serve
```

### Main

#### 基于类的组件

```typescript
//Typescript code
<script lang="ts">//这里指定是typescript
import { Component, Vue } from "vue-property-decorator"
@Component
export default class HelloWorld extends Vue {
  //coding...
}
//显式地使用name属性
@Component({
  name: 'HelloWorld',
})
</script>


//Javascript code
<script>
  export default {
	name: 'helloWorld',
}
</script> 
```

#### 引入组件

````javascript
//typescript
<template>
  <div class="main">
    <Project/>
  </div>
</template>
<script lang='ts'>
	import { Component, Vue } from 'vue-property-decorator'
	import Project from '@/components/Project.vue'
	@Component({
    commponents: {
      Project
    }
  })
export default class HelloWorld extends Vue{
  //coding...
}
</script>


//javascript
<template>
  <div class = "main">
    <Project/>
  </div>
</template>
<script>
  import Project from '@/components/Project.vue'
	export defaulf {
    name : 'HelloWordl',
    components : {
      Project
    }
  }	
</script>
````

#### Data, props, computed, methods, watchers, emit

##### Data

```javascript
//Typescript
@Component
export default class HelloWorld extends Vue{
  private msg : string = 'guanruihua'
	private list : Array<object> = [
    {
      name : 'ruihua',
      age : '23',
    },
    {
      name : 'ruihua', 
      age : '30',
    }
  ]
}

//javascript
export default {
  data(){
    return {
      msg : 'guanruihua',
      list : [
        {
          name : 'ruihau',
          age : '23',
        },
        {
          name : 'ruihua', 
          age : '30'
        }
      ]
    }
  }
}
```

##### props

```javascript
//typescript
import { Component, Prop, Vue } from 'vue-property-decorator'
@Component
export default class HelloWorld extends Vue {
  @Prop() readonly msg!:string
  @Prop({default : 'ruihua' }) readonly name : string
  @Prop({required: true }) readonly age : number
  @Prop(String) readonly address : string
  @Prop({ require: false, type: String, default: 'Developer'}) readonly job: string
}

//javascript
props:{
  msg,
  name: {
    default: 'ruihua'
  },
  age: {
  	require: true,
  },
  address: {
  	type: String
  },
  job: {
  	require: false,
    type: String,
    default: 'Developer'
  }
    
}
```

#### Computed

```javascript
//Typescript
export default class HelloWorld extends Vue {
	get fullName() :string {
		return this.firstName + '' + this.lastName
  }
	set fullName(newValue: string){
    let names = newValue.split(' ')
    this.firstName = names[0]
    this.lastName = names[ names.lenght - 1 ]
  }
}
//javascript
export default{
	fullName() {
  	return this.firstName + '' + this.lastName
  }
}

export default{
	fullName() {
    get: function(){
    	return this.firstName + '' + this.lastName  
    }
  	set: function( newValue ){
      let names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[ names.lenght - 1 ]
    }
  }
}
```

#### methods

```javascript
//Typescript
export default class HelloWorld extends Vue {
	public Fn() : void {
    console.log( this.addNum(4, 2) )
  }
	public addNum( num1: number, num2: number): number {
    return num1 + num2
  }
}

//javascript
export default {
  methods: {
  	Fn() {
    	console.log( this.addNum(4, 2))
    },
    addNum(num1, num2){
      return num1 + num2
    }
  }
}
```

#### wetchers

```javascript
//typescript
@Watch('name')//监听的变量名称
nameChange(newVal: string){
  this.name = newVal
}

@Watch('project', { 
  immediate: true, deep: true 
})
projectChanged(newVal: Person, oldVal: Person) {
  // do something
}
//javascript
watch: {
	person: {
  	handler: 'projectChanged',
		immediate: true,
     deep: true
  }
},
  methods:{
  	projectChanged:(new Val, oldVal){
      //do something
    }
  }
```

