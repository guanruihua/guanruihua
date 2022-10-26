---
title: vue-basis
date: 
tags: 
	- vue
	- basis
	- front-end
	- javascirpt
---

# Vue-basis

> 用户构建用户界面的渐进式框架, 与其他大象框架不同的是, Vue被设计为可以自底向上层应用, Vue的核心库值关视图, 便于与第三方库或基友的项目整合

`Object.freeze()`，这会阻止修改现有的 property，也意味着响应系统无法再*追踪*变化。



## 生命周期

<img src="vue-basis.assets/lifecycle.png" style="zoom: 50%;" />

### 创建阶段

> 1. `new Vue() `: `var vm = new Vue( { } );`表示创建一个Vue对象(实例)
> 2. `Init ( Events&Lifecycle ) `: 对象初始化, 在新建的对象身上,具备了一些生命周期相关的函数( 生命周期的钩子函数)和默认的事件,其他组件还没有创建(data, methods, filter等都没有创建出来)
>    - 执行`Init( Event&Lifecycle) `之后, 钩子函数都被创建出来, 马上调研生命周期函数`beforeCreate`, data和methods等组件都还没有创建出来
> 3. `Init( injections&reactivity )` : 对象初始化的后期阶段。执行生命周期函数`created`，data和methods都已经被初始化好了。
> 4. `Init`对象初始化完成阶段执行完毕后,通过对元素及其模板进行判断, 系统开始编辑模板, 将Vue代码的指令进行执行,然后再内存中生成一个编辑好的模板字符串, 最终改模板字符串渲染为内存中的DOM
>    - 只在内存中渲染好了模板, 并没有见模板挂载到页面中去, 该阶段完成后执行`beforeMount` 方法
> 5. `Create vm` : 该阶段是将内存中编译好的模板, 替换到浏览器的页面中,该阶段执行的是mounted方法
>    - 只要`mounted`方法执行完后, 就标识整个Vue对象已经初始化完毕,真实脱离创建阶段进入运行阶段, 可以开始操作页面上DOM节点

### 运行阶段

> 6. Virtual DOM: 该阶段会根据data中的最新数据,重新渲染出一份最新的DOM树, DOM树更新后悔把最新的DOM数重新渲染到页面中, 这时候完成了使用模型Model去渲染视图View
>    - 常使用两个函数beforeUpdate和updated, 会根据data数据变化, 可重复多次执行
>    - `beforeUpdate`方法执行的时候,页面显示的数据还是以前的数据, 但是data中保存的是更新后的
>    - `update`函数执行的时候, 页面和data的数据以及保持同步,都是最新的数据

### 对象销毁

> 7. `Teardown( 拆卸 )` : 对象销毁阶段, 当该对象实例运行完成后, 达到独享销毁的条件, 执行`beforeDestroy`函数, 该函数执行标志的这对象从运行状态进入到了销毁的阶段
>    - 当`beforeDestroy`函数执行的时候, 对象身上所有的组件 data, methods, filter, directive的等组件都还处于可用状态(对象只是步入到了销毁的阶段, 还没有销毁) , 该函数执行完毕后, 对象正式销毁.
> 8. `Destroy` : 对象已经销毁了,data, methodsfilter, directive等组件都不可用了

### 生命周期的钩子函数

beforeCreate

created

beforeMount

mounted

beforeUpdate

updated

beforeDestroy

destroyed



## 指定端口运行



- 在根目录新建配置文件`vue.config.js`;

- 配置文件内容

  ```js
  module.exports = {
  	devServer: {
  		// 项目运行时候的端口号
  		port: 8000
  	}
  };
  ```



## computed 和 watch

#### 计算属性computed：

- 支持缓存，只有依赖数据发生改变，才会重新进行计算
- 不支持异步，当computed内有异步操作时无效，无法监听数据的变化
- computed 属性值会默认走缓存，计算属性是基于它们的响应式依赖进行缓存的，也就是基于data中声明过或者父组件传递的props中的数据通过计算得到的值
- 如果一个属性是由其他属性计算而来的，这个属性依赖其他属性，是一个多对一或者一对一，一般用computed
- 如果computed属性属性值是函数，那么默认会走get方法；函数的返回值就是属性的属性值；在computed中的，属性都有一个get和一个set方法，当数据变化时，调用set方法。

#### 侦听属性watch：

- 不支持缓存，数据变，直接会触发相应的操作；
- watch支持异步；
- 监听的函数接收两个参数，第一个参数是最新的值；第二个参数是输入之前的值；
- 当一个属性发生变化时，需要执行对应的操作；一对多；
- 监听数据必须是data中声明过或者父组件传递过来的props中的数据，当数据变化时，触发其他操作，函数有两个参数：

> immediate：组件加载立即触发回调函数执行

```js
watch: {
  firstName: {
    handler(newName, oldName) {
      this.fullName = newName + ' ' + this.lastName;
    },
    // 代表在wacth里声明了firstName这个方法之后立即执行handler方法
    immediate: true
  }
}
```

> deep: deep的意思就是深入观察，监听器会一层层的往下遍历，给对象的所有属性都加上这个监听器，但是这样性能开销就会非常大了，任何修改obj里面任何一个属性都会触发这个监听器里的 handler

```js
watch: {
  obj: {
    handler(newName, oldName) {
      console.log('obj.a changed');
    },
    immediate: true,
    deep: true
  }
}
```

优化：我们可以使用字符串的形式监听

```
watch: {
  'obj.a': {
    handler(newName, oldName) {
      console.log('obj.a changed');
    },
    immediate: true,
    // deep: true
  }
}
```

这样Vue.js才会一层一层解析下去，直到遇到属性a，然后才给a设置监听函数。



### **prop 验证，和默认值**

我们在父组件给子组件传值的时候，可以指定该props的默认值及类型，当传递数据类型不正确的时候，vue会发出警告

```
props: {
    visible: {
        default: true,
        type: Boolean,
        required: true
    },
},
```

## 指令

### vue自带指令

1. ### v-cloak: 

   - 解决插值表达式的闪烁问题

   - 网速等问题可能显示出 双花括号 ,加载完毕后才会转换成动态赋予的值

   ```js
   先定义好样式
   [v-cloak]{
   	display:none;
   }
   再在标签中引用
   <p v-cloak >{{str}}</p> // 不适用v-cloak可能会出现{{}}
   ```

   

2. ### v-text: 

   1. ```js
      <p v-text="str"></p>
      ```

   2. 可以插入内容

3. ### v-html:

   1. ```js
      <p v-text="str"></p>
      ```

   2. 不建议使用,容易受到xss攻击

   3. 可以插入html的片段,可以得到浏览器的解析

4. 插值表达式 与 v-text/v-html的区别

   1. 对元素中已经存在的值, 只有插值表达式能够将原有的值保留, 在原有的已经存在的值的基础上添加动态数据
   2. v-text/v-html不能够保留标签原有的的内容,不会出现页面闪烁的问题

5. v-bind: 

   1. 单向数据绑定

   2. 将data中的信息动态赋予给标签的属性或者变量值中

   3. 只能实现数据的单向绑定, 从模型(M) 绑定到视图(V),使用VM将数据去渲染视图,但是我们无法通过该形式实现数据的双向绑定

   4. 简写":"

   5. ```js
      <p v-bind:title="str"></p>
      <p v-bind:title="str+'拼接'""></p>
      ```

   6. 使用class演示

      ```js
      :class = "[class1, class2, class3...]"
      
      三目运算符操作以上数组
      :class="[boolean? 'trueClass' : 'falseClass', ...]"
      
      使用对象(json)来表达以上三目运算符的操作
      :class = "[{'style_class':flag}]"
      
      ```

   7. 常用的使用方式

      1. 直接为元素的属性进行绑定操作
      2. 使用简化后的方式 , 将v-bind去除, 直接使用: 用来厎元素中的额外属性进行绑定

6. v-on: 

   1. 简写: "@"
   2. v-on: click = "fun"相当于原生的onclick = "fun()"

7. v-model: 

   1. 双向数据绑定
   2. 只能运用在表单元素中
   3. 和data数据是实现双向绑定
   4. `v-model = "str"`

8. v-for

   > - key: 提供一个有效的辨识指定元素
   > - key属性值只能为数值或字符串
   > - key一定要v-bind绑定
   > - 值必须是记录中的唯一标识

   1. 遍历字符串数组
      - `<p v-for="(item, index) in arrayList" :key = index>{{item}}</p>`
   2. 遍历对象数组
      - `<p v-for="(item, index) in arrayList" :key = index>{{item.id}}---{{item.name}}</p>`
   3. 遍历对象的属性和属性值
      - `(value[属性值], key(属性名)) in 对象`
      - ``<p v-for="(value, key, index) in arrayList" :key = index>{{value}}</p>``
   4. 遍历整形数字
      - ``<p v-for="index in 10" :key = index>{{index}}</p>``

9. v-show ,v-if , v-else-if, v-else

   - v-xxx="true":展示元素内容(创建该元素) false: 去除该元素
   - v-if="true" : 创建条件元素   false : 去除该元素
     - 每次切换true都会重新创建元素, 降低元素的效率
   - v-show="true" :  展现条件元素   false: 隐藏该元素
     - 频繁切换boolean建议使用

### 自定义全局指令

> - 自定义全局指令: `Vue.directive()`
> - 定义时候不用加"v-", 使用时候才用
> - `bind, inserted, update`是vue对象生命周期相关函数
>   - bind:操作元素样式(css)
>   - inserted: 操作元素行为(js)
>   - update:元素更新

```js
<div id="app">
	<input type = "text" v-dt/>
</div>

Vue.directive("dt",{
  //绑定时, 就可以触发
  //还没有插入到dom
  //binding 是传递过来的数据, 即v-dt="数据"
  bind: function(el,binding){
  	el.style.color = "red";
  },
  //inserted函数, 表示元素插入到dom中,
  inserted: function(el){
  
	},
  //表示元素更新时候触发, 可以随时触发
	update:function(el){
    
  }
})

var vm = new Vue({
	el: "#app",
	data: {
		str : "str"
	}
})
```

## 修饰符

### 键盘修饰符

在JavaScript事件中除了前面所说的事件，还有键盘事件，也经常需要监测常见的键值。在Vue中允许`v-on`在监听键盘事件时添加关键修饰符。记住所有的`keyCode`比较困难，所以Vue为最常用的键盘事件提供了别名：

- **`.enter`**：回车键
- **`.tab`**：制表键
- **`.delete`**：含`delete`和`backspace`键
- **`.esc`**：返回键
- **`.space`**: 空格键
- **`.up`**：向上键
- **`.down`**：向下键
- **`.left`**：向左键
- **`.right`**：向右键



### 鼠标修饰符

鼠标修饰符用来限制处理程序监听特定的滑鼠按键。常见的有：

- **`.left`**：鼠标左键
- **`.middle`**：鼠标中间滚轮
- **`.right`**：鼠标右键

### 修饰键

可以用如下修饰符开启鼠标或键盘事件监听，使在按键按下时发生响应：

- **`.ctrl`**
- **`.alt`**
- **`.shift`**
- **`.meta`**



### 事件修饰符

a.stop : 阻止事件的冒泡机制(点击内层事件会触发外层的点击事件)

a.prevent: 阻止默认行为

c.capture : 事件会优先触发,如果加在最外层的事件会往里冒泡

d.self : 阻止自身冒泡行为的行为,(它不会真正阻止冒泡行为)

```text
案例: div>div>button
1. 在内层加入.stop修饰符
	点击button,只会触发事件冒泡到内层事件
2. 在内层加入.self修饰符
	点击button,在事件冒泡过程,不会触发.self的事件
```

e.once : 只会触发一次事件处理函数,

	>  .once 需要结合.prevent来使用
	>
	>  语法: @click.prevent.once 

#### 自定义私有指令

> - 在指定vm对象中定义, 值针对vm对象描述的元素生效的指令,
> - 其他vm对象不能使用该指令

```js
<div id="app">
	<input type = "text" v-dt/>
</div>

Vue.directive("dt",{
  //绑定时, 就可以触发
  //还没有插入到dom
  //binding 是传递过来的数据, 即v-dt="数据"
  bind: function(el,binding){
  	el.style.color = "red";
  },
  //inserted函数, 表示元素插入到dom中,
  inserted: function(el){
  
	},
  //表示元素更新时候触发, 可以随时触发
	update:function(el){
    
  }
})

var vm = new Vue({
	el: "#app",
	data: {
		str : "str"
	},
  directives: {
  	"dt": {
    	bind: function(el,binding){}
     inserted: function(el){}
  		update: function(el){}
    }
  }
})
```



## 过滤器

> - 就是通过输入数据,能够及时对数据进行处理并返回一个数据结果的简单函数
> - 复用性
> - 系统会先查找私有再找全局过滤器

### 全局过滤器

> - 所有VM对象都能共此案使用的过滤器
>
> - ```js
>   单个过滤器的使用
>   <p>{{str | myfilter}}</p>
>   <script>
>     Vue.filter("myfilter", function(value){
>   		....
>       return outValue;
>   	})
>   var vm = new Vue({
>     el: "app",
>     data: {
>       str: "aaa"
>     }
>   	  
>   })
>   </script>
>     
>   连续使用多个过滤器
>   <p>{{str | myfilter1 | myfilter2 | ... }}</p>
>   ```

### 私有过滤器

> - 在VM对象定义过滤器
>
> - 只能在vm对象中作用
>
> - ```js
>   <p>{{str | myfilter}}</p>
>   <script>
>     Vue.filter("myfilter", function(value){
>   		....
>       return outValue;
>   	})
>   var vm = new Vue({
>     el: "app",
>     data: {
>       str: "aaa"
>     },
>     filters : {
>       myfilter1 : function(value){
>       		....
>           return outValue;
>       	}
>     }
>   	  
>   })
>   </script>
>   ```

## vue对ajax的支持

### vue-resource

#### get

this.$http.get("请求路径" , "请求参数").then( "回调函数" )//拿数据data.body.key

this.$http.get("请求路径" , "请求参数").then(function (data){...}) )

#### post

this.$http.get("请求路径?name=data" , "请求参数").then(function (data){...}) )

this.$http.get("请求路径" ,  {name: data } ,{ emulateJSON : true }).then(function (data){...}) )

### axios

> 一个基于Promise的HTTP请求客户端, 用来发出请求, 官方推荐

#### get

```js
axios({
	method : "get", 
  url : "url",
  params : {"name":"data"},//传递参数
}).then(function(result){
  console.log(result.data.key)
})

//使用箭头函数
axios({
	method : "get", 
  url : "url"
}).then(result=>{
  console.log(result.data.key)
})

axios.get("url", {params : { "name" : "data"}).then(result => {
	console.log(result.data.key);                 
})
```

#### post

```js
//使用箭头函数
axios({
	method : "post", 
  url : "url",
  params : { "key" : "data" }
}).then(result=>{
  console.log(result.data.key)
})

axios.post("url", "key = data".then(result => {
	console.log(result.data.key);                 
})
```

## 跨域请求的处理

jsonp来处理

```js
this.$http.jsonp("http://localhost: 8080/projectName/getData.do").then(result=>{
  data = result.body
})
```

## Vue动画

<img src="vue-basis.assets/transition.png" style="zoom:50%;" />

Enter(信息进入阶段)

v-enter : 进去前

v-enter-active :  进入过程

v-enter-to : 进去后



Leave(信息离开阶段)

v-leave : 离开前

v-leave-active : 离开过程

v-leave-to :离开后

```html
<transition>
  <p v-show="flag" >aaa</p>
</transition>
<style>
  .v-enter, .v-leave-to{
    opacity: 0;
    transform: opacity;
  }
  .v-enter-active, .v-leave-active{
    transition: all 0.8s ease;
  }
</style>
```

通过name来给指定元素添加特效

```html
<transition name="tran1">
  <p v-show="flag" >aaa</p>
</transition>
<style>
  .tran1-enter, .tran1-leave-to{
    opacity: 0;
    transform: opacity;
  }
  .tran1-enter-active, .tran1-leave-active{
    transition: all 0.8s ease;
  }
</style>
```



## 其他

@keyup = "fn" : 敲键盘之后就会触发

@change = "fn" : 数据改变就会触发

