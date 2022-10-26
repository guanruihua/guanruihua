---
title: react-basis
date: 
tags:
	- react
	- basis
	- javascript
	- front-end
---

# React-basis

> 1. Facebook开源的一个js库
> 2. 一个用来动态构建用户界面的js库
> 3. 特点:
>    1. Declarative( 声明式编码) - 采用声明式范式, 可以轻松描述应用
>    2. Component-based(组件化编码) - 是代码更加容易得到复用 
>    3. 支持客户端与服务器渲染
>    4. 高效 - 通过对DOM的模拟, 最大限度减少与DOM的交互
>    5. 单向数据流 - 减少重复代码
> 4. 高效的原因:
>    1. 虚拟( virtual ) DOM, 不直接操作DOM
>    2. 高效的DOM Diff算法, 最小化重绘(减少页面更新的区域)

## JSX

> 1. 全称: JavaScript XML
> 2. react定义的一种类似XML的JS拓展语法: XML + JS
> 3. 作用 : 用来创建react虚拟DOM( 元素 )对象
> 4. 编码相关:
>    1. js中直接可以套标签, 标签套js需要放在{}中
>    2. 解析显示js数组时, 会中东遍历显示
> 5. 注意:
>    1. 标签必须要有结束
>    2. class属性必须改成className
>    3. 标签的style属性值必须为: `{{color:'red', width: 12}}`

## 组件化与组件化

1. 组件化
   1. 当应用的js都是以模块来编写的, 这个应用就是一个模块化的应用
   2. 就是把重负的部分提炼出来, 一个一个能使用
   3. 例如Dialog, 各种自定义UI组件, 能在项目或不同项目重复使用等
   4. 目的:复用, 解耦
   5. 依赖:组件之间低依赖, 比较独立
   6. 架构定位:纵向分层 (位于架构底层, 被其他成所依赖) 
2. 模块化
   1. 但应用是以多组件的方式实现功能, 改应用就是一该组件化的应用
   2. "业务框架" 或者"业务模块", 也可理解为"框架",
   3. 就是把功能进行划分, 将同一个类型的代码整合在一起, 所有模块的工鞥相对复杂, 都属于一个业务
   4. 使用: 按照项目功能需求划分成不同类型的业务框架(注册, 登录, 外卖,等)
   5. 目的: 隔离/封装(高内聚)
   6. 依赖: 模块之间有依赖的关系, 可通过路由器进行模块之间的耦合关系
   7. 架构定位: 横向分块(位于架构业务框架层 )
3. 两者区别

## 环境搭建

- 引用ReactCDN资源

```html
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script> 
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
```



- 通过npm(包管理),安装React

`$npm install --save react react `

`$npm install --save react react-dom`

- 利用脚手架create-react-app

`$ npm install -g create-react-app `

`$ create-react-app my-app `

`$ cd my-app/ $ npm start`

- 启动IP和端口号

`在node_modules/react-script/scripts/start.js中设置(60和61行) const DEFAULT_PORT = parseInt(process.env.PORT, 10) || 3000; const HOST = process.env.HOST || '0.0.0.0';`

## 项目根目录中的文件介绍

- **README.md** :这个文件主要作用就是对项目的说明，已经默认写好了一些东西，你可以简单看看。如果是工作中，你可以把文件中的内容删除，自己来写这个文件，编写这个文件可以使用Markdown的语法来编写。
- **package.json**: 这个文件是webpack配置和项目包管理文件，项目中依赖的第三方包（包的版本）和一些常用命令配置都在这个里边进行配置，当然脚手架已经为我们配置了一些了，目前位置，我们不需要改动。如果你对webpack了解，对这个一定也很熟悉。
- **package-lock.json**：这个文件用一句话来解释，就是锁定安装时的版本号，并且需要上传到git，以保证其他人再npm install 时大家的依赖能保证一致。
- **gitignore**: 这个是git的选择性上传的配置文件，比如一会要介绍的node_modules文件夹，就需要配置不上传。
- **node_modules**:这个文件夹就是我们项目的依赖包，到目前位置，脚手架已经都给我们下载好了，你不需要单独安装什么。
- **public**：公共文件，里边有公用模板和图标等一些东西。
- **src**： 主要代码编写文件，这个文件夹里的文件对我们来说最重要，都需要我们掌握。

### public文件夹介绍

这个文件都是一些项目使用的公共文件，也就是说都是共用的

- **favicon.ico**: 这个是网站或者说项目的图标，一般在浏览器标签页的左上角显示。
- **index.html**: 首页的模板文件
- **mainifest.json**：移动端配置文件

### src文件夹介绍

这个目录里边放的是我们开放的源代码，我们平时操作做最多的目录。

- **index.js**: 这个就是项目的入口文件
- **index.css**：这个是index.js里的CSS文件。
- **app.js**: 这个文件相当于一个方法模块，也是一个简单的模块化编程。
- **serviceWorker.js**:这个是用于写移动端开发的，PWA必须用到这个文件，有了这个文件，就相当于有了离线浏览的功能。



## 组件的介绍

### 入口文件的编写

写一个项目的时候一般要从入口文件进行编写的，在src目录下，index.js文件就是入口文件

```
import React from 'react' // 引入react
import ReactDOM from 'react-dom' // 引入react-dom
import App from './App' // 引入App模块
ReactDOM.render(<App />,document.getElementById('root')) // 将App模块渲染到了 root ID上面
复制代码
```

### App组件的编写

```jsx
import React from 'react'
// JSX语法
class App extends React.Component{
    render(){
        return (
            <div>
                我是APP组件
            </div>
        )
    }
}
export default App;
```

### React中JSX语法简介

```
JSX就是Javascript和XML结合的一种格式。React发明了JSX，
可以方便的利用HTML语法来创建虚拟DOM，当遇到<，JSX就当作HTML解析，
遇到{就当JavaScript解析.
```

### 组件和普通JSX语法区别

这个说起来也只有简单的一句话，就是你自定义的组件必须首写字母要进行大写，而JSX是小写字母开头的。

### JSX中使用三元运算符

在JSX中也是可以使用js语法的

```jsx
import React from 'react'
// JSX语法
class App extends React.Component{
    render(){
        return (
            <div>
                <div>{false ? '不显示我' : '显示的我'}</div>
                我是APP组件
            </div>
        )
    }
}
export default App;
```

### 组件外层包裹原则

1. react和vue组件模板最外层必须有且只有一个元素
2. 去除这个最外层包裹的标签, 使用 
   - `<React.Fragment> </React.Fragment>`
   - `<></>`

## 响应式设计和数据绑定

> react不建议你直接操作DOM元素, 而是通过数据进行驱动, 改变界面中的效果

## react基础

### 创建组件方法

#### 函数式定义的**无状态组件**

> 主要负责根据props来展示, 不涉及到要state状态的操作
>
> 1. 组件不会被实例化, 整体渲染性能得到提升
>    - 精简到成一个render方法的函数来实现, 无实例化过程, 就不需要分配多余的内存, 从而性能得到一定的提升
> 2. 组件不能访问this对象
> 3. 组件无法访问生命周期的方法
> 4. 无状态组件只能访问输入端饿props, 同样的props会得到同样的渲染结果, 不会有副作用

```jsx
function HelloComponent(props, /* context */) {
  return <div>Hello {props.name}</div>
}
ReactDOM.render(<HelloComponent name="Sebastian" />, mountNode) 
```



#### es5原生方式`React.createClass`定义的组件

> - 会自绑定函数导致不必要的性能开销, 增加代码过时的肯能性
> - 基本弃用

```jsx
var InputControlES5 = React.createClass({
    propTypes: {//定义传入props中的属性各种类型
        initialValue: React.PropTypes.string
    },
    defaultProps: { //组件默认的props对象
        initialValue: ''
    },
    // 设置 initial state
    getInitialState: function() {//组件相关的状态对象
        return {
            text: this.props.initialValue || 'placeholder'
        };
    },
    handleChange: function(event) {
        this.setState({ //this represents react component instance
            text: event.target.value
        });
    },
    render: function() {
        return (
            <div>
                Type something:
                <input onChange={this.handleChange} value={this.state.text} />
            </div>
        );
    }
});
InputControlES6.propTypes = {
    initialValue: React.PropTypes.string
};
InputControlES6.defaultProps = {
    initialValue: ''
};
```





#### es6形式的`extends React.Component`定义的组件

> - 目前最推荐的有状态的组件, 最终会取代`React.createClass`形式, 相对于`React.createClass`可以更好实现代码复用
> - 定义方法的命名规则: `handle+方法名`,   可以省略`function`关键字
>   - 还要在`constructor` 下进行注册 : `this.handleXXX = this.handleXXX.bind(this);`
>   - 或者直接 在组件中 `click= { ()=> this.hanhdleXXX }`

```jsx
class InputControlES6 extends React.Component {
    constructor(props) {
        super(props);
        // 设置 initial state
        this.state = {
            text: props.initialValue || 'placeholder'
        };
        // ES6 类中函数必须手动绑定
        this.handleChange = this.handleChange.bind(this);
    }
    handleChange(event) {
        this.setState({
            text: event.target.value
        });
    }
    render() {
        return (
            <div>
                Type something:
                <input onChange={this.handleChange}
               value={this.state.text} />
            </div>
        );
    }
}
InputControlES6.propTypes = {
    initialValue: React.PropTypes.string
};
InputControlES6.defaultProps = {
    initialValue: ''
};
```



#### React.ceateClass 和 React.Component区别

##### 函数this自绑定

>  React.ceateClass : 每一成员函数的this都有React自动绑定, 任何时候使用, 直接使用this.method即可, 函数中的this会被成功设置
>
> React.Component : 创建组件, 其成员函数不会自动绑定this, 需要开发者手动绑定, 否则this不嗯呢获取当前组件实例对象, 需要绑定

```jsx
const Contacts = React.createClass({  
  handleClick() {
    console.log(this); // React Component instance 可以成功获取到this对象
  },
  render() {
    return (
      <div onClick={this.handleClick}></div>
    );
  }
});
```



```jsx
class Contacts extends React.Component {  
  constructor(props) {
    super(props);
  }
  handleClick() {
    console.log(this); // null 获取不到
  }
  render() {
    return (
      <div onClick={this.handleClick}></div>
    );
  }
 
// 手动绑定
  constructor(props) {
    super(props);
    this.handleClick = this.handleClick.bind(this); //构造函数中绑定
  }
  <div onClick={this.handleClick.bind(this)}></div> //使用bind来绑定
  <div onClick={()=>this.handleClick()}></div> //使用arrow function来绑定
```



##### 组件属性类型propTypes及其默认props属性defaultProps配置不同

> - `React.createClass`在创建组件时，有关组件props的属性类型及组件默认的属性会作为**组件实例的属性**来配置，其中defaultProps是使用`getDefaultProps`的方法来获取默认组件属性的
>
> - `React.Component`在创建组件时配置这两个对应信息时，他们是作为**组件类的属性**，不是组件实例的属性，也就是所谓的**类的静态属性**来配置的

```jsx
const TodoItem = React.createClass({
    propTypes: { // as an object
        name: React.PropTypes.string
    },
    getDefaultProps(){   // return a object
        return {
            name: ''    
        }
    }
    render(){
        return <div></div>
    }
})

class TodoItem extends React.Component {
    static propTypes = {//类的静态属性
        name: React.PropTypes.string
    };

    static defaultProps = {//类的静态属性
        name: ''
    };

    ...
}
```





##### 组件初始化状态state的配置不同

> `React.createClass`创建的组件，其状态state是通过`getInitialState`方法来配置组件相关的状态；
> `React.Component`创建的组件，其状态state是在`constructor`中像初始化组件属性一样声明的。

```jsx
const TodoItem = React.createClass({
    // return an object
    getInitialState(){ 
        return {
            isEditing: false
        }
    }
    render(){
        return <div></div>
    }
})
class TodoItem extends React.Component{
    constructor(props){
        super(props);
        this.state = { // define this.state in constructor
            isEditing: false
        } 
    }
    render(){
        return <div></div>
    }
}
```





##### Mixins的支持不同

> [`Mixins`](https://facebook.github.io/react/docs/reusable-components-zh-CN.html#mixins)(混入)是面向对象编程OOP的一种实现，其作用是为了复用共有的代码，将共有的代码通过抽取为一个对象，然后通过`Mixins`进该对象来达到代码复用。具体可以参考[React Mixin的前世今生](http://www.w3ctech.com/topic/1599)。
>
> `React.createClass`在创建组件时可以使用`mixins`属性，以数组的形式来混合类的集合。
>
> `React.Component` : 不支持Mixins, React开发者社区提供一个全新的方式来取代`Mixins`,那就是**Higher-Order Components**，具体细节可以参考[这篇文章](https://leozdgao.me/chushi-hoc/)

```jsx
var SomeMixin = {  
  doSomething() {

  }
};
const Contacts = React.createClass({  
  mixins: [SomeMixin],
  handleClick() {
    this.doSomething(); // use mixin
  },
  render() {
    return (
      <div onClick={this.handleClick}></div>
    );
  }
});
```



### jsx代码注释

```jsx
 {/* 正确注释的写法 */}
 { // 正确注释的写法 }
```



### 组件的三大属性

#### state

```jsx
1. 组件被称为"状态机", 页面的显示是根据组件的state属性的数据来显示
2. 初始化指定:
    constructor() {
      super()
      this.state = {
        stateName1 : stateValue1,
        stateName2 : stateValue2
      }
    }
3. 读取显示: 
    this.state.stateName1
4. 更新状态-->更新界面 : 
    this.setState({stateName1 : newValue})
```



#### props

> 所有组件标签的属性的集合对象
> 给标签指定属性, 保存外部数据(可能是一个function)
> 在组件内部读取属性: this.props.propertyName
> 作用: 从目标组件外部向组件内部传递数据

```jsx
对props中的属性值进行类型限制和必要性限制
Person.propTypes = {
  name: React.PropTypes.string.isRequired,
  age: React.PropTypes.number.isRequired
}
扩展属性: 将对象的所有属性通过props传递
  <Person {...person}/>
```

##### 父传子

```jsx
// 父组件
<childItem myName={this.state.myName}/>

// 子组件
<div>{this.props.myName}</div>}
```



##### 子传父

> 1. 先在父组件定义可以改变数据的方法pranentChange(且要注册), 和数据myName
> 2. 子组件通过`this.props.myName`拿到数据, 且要在constructor注册或绑定一下父组件的方法

```jsx
// 子组件
class childItem extends React.Component{
    constructor (props) {
        super(props)
        this.changeParent = this.changeParent.bind(this)
    }
    return (
        <React.Fragment>
            <div onClick={this.changeParent}>{this.props.myName}</div>
        </React.Fragment>
    )
    changeParent () {
        // 调用父组件传递过来的方法
        this.props.pranentChange()
    }
}
export default childItem;
```



```jsx
// 父组件

// 引入子组件
import childItem from './childItem'
// JSX语法
class parentItem extends React.Component{
    constractor (props) {
        super(props)
        this.state = {
            myName: '只会番茄炒蛋'
        }
        this.pranentChange = this.pranentChange.bind(this)
    }
    return (
        <React.Fragment>
            {/* 使用了子组件 */}
            <childItem 
                pranentChange={this.pranentChange}
                myName={this.state.myName}
            />
        </React.Fragment>
    )
    pranentChange () {
        this.setState({
            myName: '番茄炒蛋少放糖'
        })
    }
}
export default parentItem;
```



#### PropTypes检验传递值

> 在传输数据中,最好要加入数据校验
>
> 加入校验, 不按照校验以后, 会有warning警告

```jsx
// 子组件
import React from 'react'
// 引入效验
import PropTypes from 'prop-types'
// JSX语法
class childItem extends React.Component{
    constructor (props) {
        super(props)
        this.changeParent = this.changeParent.bind(this)
    }
    return (
        <React.Fragment>
            <div onClick={this.changeParent}>{this.props.myName}</div>
        </React.Fragment>
    )
    changeParent () {
        // 调用父组件传递过来的方法
        this.props.pranentChange()
    }
}
childItem.prorTypes = {
    myName: PropTypes.string, // 效验传入的内容必须是一个字符串
    pranentChange: PropTypes.func // 效验传入的内容必须是一个function
}
export default childItem;
```

##### 必传值的校验---isRequired

> 不传递带有isRequired的数据, 就会报错

```jsx
childItem.prorTypes = {
    // 效验传入的内容必须是一个字符串并且必须传入值
    newName:PropTypes.string.isRequired, 
}
```



##### 使用默认值---defaultProps

> 设置默认值

```jsx
childItem.defaultProps = {
	// 子组件使用父组件传递的值, 但是他自己又个默认的值
  newName: '啦啦啦',
}
```







#### ref

> 组件内包含ref属性的标签元素的集合对象
> 给操作目标标签指定ref属性, 打一个标识
> 在组件内部获得标签对象: this.refs.refName(只是得到了标签元素对象)
> 作用: 找到组件内部的真实dom元素对象, 进而操作它



```jsx
import React from 'react'
// JSX语法
class parentItem extends React.Component{
    constractor (props) {
        super(props)
        this.state = {
            myName: '只会番茄炒蛋'
        }
        this.inputChange = this.inputChange.bind(this)
    }
    return (
        <React.Fragment>
            <input 
                value={this.state.myName}
                onChange={this.inputChange}
            ></input>
        </React.Fragment>
    )
    inputChange (e) {
        console.log(e.target.value)// 获取input输入的值
    }
}
export default parentItem;
```



```jsx
import React from 'react'
// JSX语法
class parentItem extends React.Component{
    constractor (props) {
        super(props)
        this.state = {
            myName: '只会番茄炒蛋'
        }
        this.inputChange = this.inputChange.bind(this)
    }
    return (
        <React.Fragment>
            <input 
                value={this.state.myName}
                onChange={this.inputChange}
                ref={input => {this.input = input}}
            ></input>
        </React.Fragment>
    )
    inputChange (e) {
        console.log(this.input.value)
    }
}
export default parentItem;
```



### 基础

1. react的入口

``` js
// 无状态函数
function MyCompontent(props) {
	return <h1>组件</h1>
}
// 复杂组件
class MyComponent extends React.Componet  {
  render() {
    return <h1>组件</h1>
  }
}
// 渲染组件标签
ReactDOM.render(
    element,
  document.getElementById('root')
);  
/*
等到虚拟DOM对象, 将虚拟DOM 并解析为真实DOM, 插入到指定的页面元素中
*/
```

element就是通过 ReactDOM.render() 的方法来将其渲染到页面上

2. 自己创建element

```js
const element = (
	<h1>
		Hello, {formatName(user)}!
	</h1>
);
```

3. JSX代表Object
   下面两段代码完全一样

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

4. 计时器的使用

```js
setInterval(tick, 1000);///每一秒调用一次tick方法
```

5. 组件&Props 

   -  函数定义

      ```js
      function welcome(props){ return (<h1>{props.name}</h1>)}
      ```

   -  类定义组件

      ```js
      class welcome extends React.Component{
          render(){
              return (<h1>{this.props.name}</h1>);
          }
      }
      ```

   -  组件渲染

   ```js
       const element = <div />;
       const element = <welcome name = "guanruihua" />
   ```

   -  组合组件

   ```js
       function Welcome(props) {
         return <h1>Hello, {props.name}</h1>;
       }
       
       function App() {
         return (
           <div>
             <Welcome name="Sara" />
             <Welcome name="Cahal" />
             <Welcome name="Edite" />
           </div>
         );
       }
       
       ReactDOM.render(
         <App />,
         document.getElementById('root')
       );
   ```

   - 提取组件

   ```jsx
   //方法一
   function Comment(props) {
     return (
       <div className="Comment">
         <div className="UserInfo">
           <img className="Avatar"
             src={props.author.avatarUrl}
             alt={props.author.name}
           />
           <div className="UserInfo-name">
             {props.author.name}
           </div>
         </div>
         <div className="Comment-text">
           {props.text}
         </div>
         <div className="Comment-date">
           {formatDate(props.date)}
         </div>
       </div>
     );
   }
   //提取组件
   function Avatar(props) {
     return (
       <img className="Avatar"
         src={props.user.avatarUrl}
         alt={props.user.name}
       />
     );
   }
   
   
   //方法二
               
       
       function Comment(props) {
         return (
           <div className="Comment">
             <div className="UserInfo">
               <Avatar user={props.author} />
               <div className="UserInfo-name">
                 {props.author.name}
               </div>
             </div>
             <div className="Comment-text">
               {props.text}
             </div>
             <div className="Comment-date">
               {formatDate(props.date)}
             </div>
           </div>
         );
       }
       
       
       
       function UserInfo(props) {
         return (
           <div className="UserInfo">
             <Avatar user={props.user} />
             <div className="UserInfo-name">
               {props.user.name}
             </div>
           </div>
           );
       }
   ```

### 引入本地图片

```js
import img from './img/1.png'
<img src={img} />

<img src={require('./img/1.png') alt= "" />
//用作背景图
style  = {{background:'url($require('./img/1.png')})'}}
```

### css

```js
//基于class--(className)
<style>.title{...}</title>

<div className="title"></div>

//基与inner css行间样式(json)
<div style = {{color:'red'}}></div>

//原型链和全局变量
var color={color:'red'}  
<div style={color}>react全局行间样式</div>

App.prototype.col={
    color:pink  
}

<div style={this.col}>原型样式</div>

```

## 动画react-transition-group

### 安装

```visual basic
$ npm install react-transition-group -S
```

> 三大核心库
>
> - Transition
> - CSSTransition
> - TransitionGroup



### 使用CSSTransition

```jsx
import { CSSTranstion } from 'react-transition-group';

render() {
	return (
  	 <div>
      <CSSTransition 
        in={this.state.isShow}   //用于判断是否出现的状态
        timeout={2000}           //动画持续时间
        classNames="boss-text"   //className值，防止重复
        >
        <div>BOSS级人物-孙悟空</div>
      </CSSTransition>
      <div>
        <button onClick={this.toToggole}>召唤Boss</button>
      </div>
    </div>
  );
}
```

- xxx-enter: 进入（入场）前的CSS样式；
- xxx-enter-active:进入动画直到完成时之前的CSS样式;
- xxx-enter-done:进入完成时的CSS样式;
- xxx-exit:退出（出场）前的CSS样式;
- xxx-exit-active:退出动画知道完成时之前的的CSS样式。
- xxx-exit-done:退出完成时的CSS样式。

```css
.input {border:3px solid #ae7000}

.boss-text-enter{
    opacity: 0;
}
.boss-text-enter-active{
    opacity: 1;
    transition: opacity 2000ms;

}
.boss-text-enter-done{
    opacity: 1;
}
.boss-text-exit{
    opacity: 1;
}
.boss-text-exit-active{
    opacity: 0;
    transition: opacity 2000ms;

}
.boss-text-exit-done{
    opacity: 0;
}
```

#### unmountOnExit 属性

> 加上这个, 元素退场, 会自动吧DOM 也删除

```jsx
 <CSSTransition 
   in={this.state.isShow}   //用于判断是否出现的状态
   timeout={2000}           //动画持续时间
   classNames="boss-text"   //className值，防止重复
   unmountOnExit
   >
  <div>BOSS级人物-孙悟空</div>
</CSSTransition>
```



### 使用TransitionGroup



```jsx
import { CSSTransition , TransitionGroup } from 'react-transition-group'


// 原code
<ul ref={(ul)=>{this.ul=ul}}>
    <TransitionGroup>
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </TransitionGroup>
</ul> 
  
// add TransitionGroup
  
<ul ref={(ul)=>{this.ul=ul}}>
    <TransitionGroup>
        <CSSTransition
            timeout={1000}
            classNames='boss-text'
            unmountOnExit
            appear={true}
            key={index+item}
        >
            <li>1</li>
        </CSSTransition>
        <CSSTransition
            timeout={1000}
            classNames='boss-text'
            unmountOnExit
            appear={true}
            key={index+item}
        >
            <li>2</li>
        </CSSTransition>
    </TransitionGroup>
</ul> 
```





## 列表& keys



### 使用map()函数可以让数组中每一项翻倍

```
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map((number) => number * 2);
//即[2,4,6,8,10]
```

1. #### 渲染多个组件

```js
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
  <li>{number}</li>
);

ReactDOM.render(
  <ul>{listItems}</ul>,
  document.getElementById('root')
);
//生成了一个1到5的数字列表
```

2. #### 基础列表组件

```js
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li = {number.toString()}>//给每一个列表分配一个key
        {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
//输出一个无序列表
```

3. #### Keys(可以在DOM中的某些元素被增加或删除的时候帮助React识别哪些元素发生了变化)

```js
<li key={number.toString()}>
    {number}
</li>

//给每个元素都带有一个独一无二的key
const todoItems = todos.map((todo) =>
<li key={todo.id}>
    {todo.text}
</li>

//当元素没有确定的id时，你可以使用他的序列号索引index作为key  
const todoItems = todos.map((todo, index) =>
  // Only do this if items have no stable IDs
  <li key={index}>
    {todo.text}
  </li>
);
```

4. #### 用Keys提取组件(元素的key只有在它和它的兄弟节点对比时才有意义)

```js
function ListItem(props) {
  // 对啦！这里不需要指定key:
  return <li>{props.value}</li>;
}

function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    // 又对啦！key应该在数组的上下文中被指定
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

5. #### 元素的Key再他的兄弟元素之间应该唯一(不需要是全局唯一的)

```js
function Blog(props) {
  const sidebar = (
    <ul>
      {props.posts.map((post) =>
        <li key={post.id}>
          {post.title}
        </li>
      )}
    </ul>
  );
  const content = props.posts.map((post) =>
    <div key={post.id}>
      <h3>{post.title}</h3>
      <p>{post.content}</p>
    </div>
  );
  return (
    <div>
      {sidebar}
      <hr />
      {content}
    </div>
  );
}

const posts = [
  {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
  {id: 2, title: 'Installation', content: 'You can install React from npm.'}
];
ReactDOM.render(
  <Blog posts={posts} />,
  document.getElementById('root')
);
```

6. #### 在jsx中嵌入map()

```jsx
//声明了一个单独的listItems变量并将其包含在JSX中
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <ListItem key={number.toString()}
              value={number} />
  );
  return (
    <ul>
      {listItems}
    </ul>
  );
}


//JSX允许在大括号中嵌入任何表达式，所以我们可以在map()中这样使用：
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}


```



## 表单

> 获取到表单输入的数据 : event.tatget.value

### 受控组件

```jsx
class NameForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('A name was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input type="text" value={this.state.value}  
          onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```


```
//使用”受控组件”,每个状态的改变都有一个与之相关的处理函数。这样就可以直接修改或验证用户输入。例如，我们如果想限制输入全部是大写字母
handleChange(event) {
  this.setState({value: event.target.value.toUpperCase()});
}
```

### textarea标签

#### `<textarea>`会用value属性来代替

```jsx
class EssayForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: 'Please write an essay about your favorite DOM element.'
    };

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('An essay was submitted: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <textarea value={this.state.value} onChange={this.handleChange} />
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

### select标签

```js
class FlavorForm extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: 'coconut'};

    this.handleChange = this.handleChange.bind(this);
    this.handleSubmit = this.handleSubmit.bind(this);
  }

  handleChange(event) {
    this.setState({value: event.target.value});
  }

  handleSubmit(event) {
    alert('Your favorite flavor is: ' + this.state.value);
    event.preventDefault();
  }

  render() {
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Pick your favorite La Croix flavor:
          <select value={this.state.value} onChange={this.handleChange}>
            <option value="grapefruit">Grapefruit</option>
            <option value="lime">Lime</option>
            <option value="coconut">Coconut</option>
            <option value="mango">Mango</option>
          </select>
        </label>
        <input type="submit" value="Submit" />
      </form>
    );
  }
}
```

### file input 标签

```jsx
<input type="file">   
//value是只读
//是一个非受控组件
```

##### 多个输入的解决方法

```jsx
//当你有处理多个受控的input元素时，  //你可以通过给每个元素添加一个name属性，来让处理函数根据 event.target.name的值来选择做什么。
class Reservation extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      isGoing: true,
      numberOfGuests: 2
    };

    this.handleInputChange = this.handleInputChange.bind(this);
  }

  handleInputChange(event) {
    const target = event.target;
    const value = target.type === 'checkbox' ? target.checked : target.value;
    const name = target.name;

    this.setState({
      [name]: value
    });
  }

  render() {
    return (
      <form>
        <label>
          Is going:
          <input
            name="isGoing"
            type="checkbox"
            checked={this.state.isGoing}
            onChange={this.handleInputChange} />
        </label>
        <br />
        <label>
          Number of guests:
          <input
            name="numberOfGuests"
            type="number"
            value={this.state.numberOfGuests}
            onChange={this.handleInputChange} />
        </label>
      </form>
    );
  }
}
```





## react-webpack前言

### 安装webpack

1. 在所要安装目录输入npm -v和node -v查看配置是否有问题  
2. 输入npm init -y创建package.json
3. 输入npm install webpack --save-dev
4. 安装webpack-cli
5. npm install --global webpack
6. npm install --global webpack-cli  
7. 最后用webpack -v查看版本



### 安装插件

1. html-webpack-plugin
2. 安装指令:npm i --save-dev html-webpack-plugin


### 配置文件

```
"dev" : "webpack --mode development",//b不会压缩js代码
"build": "webpack --mode production",//压缩js代码
```

//ISC  

```
"build": "webpack --mode production --config scripts/webpack.config.js",  
```

## State&生命周期

>props:只读
>
>state:可读写

### 组件的生命周期



- 挂载期:一个组件实例初次被创建的过程
- 更新期:组件在创建后再次渲染的过程
- 卸载期:组件在使用完后被销毁的过程
- 组件的挂载
  - constructor:构造函数,初始化状态
  - getInitialState:设置状态机
  - getDefaultProps:获取默认的props
  - componentWillMount:首次渲染前执行
  - render:渲染组件
  - componentDidMount:render渲染后执行的操作
- 组件的更新
  - componentWillRexeiveProps:当父组件更新子组件的state
  - shouldComponentUpdate:决定组件state或props的改变是否需要重新渲染
  - componentWillUpdate:重新渲染props或state前
  - componentDidUpdate:重新渲染props或state后
- 错误处理
  - static getDerivedStateFromError() : 在后代组件抛出错误后被调用
  - componentDidCatch() : 会在"提交" 阶段被调用



<img src="https://user-gold-cdn.xitu.io/2019/9/5/16d004e82c3c92bb?imageView2/0/w/1280/h/960/format/webp/ignore-error/1" style="zoom: 33%;" />

```jsx
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(
    element,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

### 使Clock组件真正可重用和封装  

```jsx
function Clock(props) {
  return (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {props.date.toLocaleTimeString()}.</h2>
    </div>
  );
}

function tick() {
  ReactDOM.render(
    <Clock date={new Date()} />,
    document.getElementById('root')
  );
}

setInterval(tick, 1000);
```

### 将函数转换为类


```basis
1. 创建一个名称扩展为 React.Component 的ES6 类
2. 创建一个叫做render()的空方法
3. 将函数体移动到 render() 方法中
4. 在 render() 方法中，使用 this.props 替换 props
5. 删除剩余的空函数声明  
```


```
class Clock extends React.Component {
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.props.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
```

#### 使用类就允许我们使用其它特性，例如局部状态、生命周期钩子

### 为类添加局部变量

```jsx
//2. 添加一个类构造函数来初始化状态 this.state  
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
//1. 在 render() 方法中使用 this.state.date 替代 this.props.date 
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}
//3. 从 <Clock /> 元素移除 date 属性：
ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);

```

### 将生命周期方法添加到类中

###### 挂载:每当Clock组件第一次加载到DOM中的时候，我们都想生成定时器

###### 卸载:每当Clock生成的这个DOM被移除的时候，我们也会想要清除定时器  

```
//生命周期钩子


componentDidMount() {
    this.timerID = setInterval(
        () => this.tick(),
        1000
    );
}
//卸载计时器
componentWillUnmount() {
    clearInterval(this.timerID);
}
```

#### state

1. 只可以使用setState()来更新state
2. 状态更新可能是异步的(多个setState()可以调用合并成一个来提高性能)
3. 状态更新合并,调用setState()可以将你提供的对象合并到当前状态 x

## 事件处理

```jsx
//事件命名方式为驼峰命名法
<button onClick={activateLasers}>
  Activate Lasers
</button>

```

### preventDefault事件的使用

###### 在 React 中另一个不同是你不能使用返回 false 的方式阻止默认行为

```
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();//防止发生默认事件
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```

###### 向事件处理程序传递参数

```
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

1. 上述两种方式是等价的，分别通过 arrow functions 和  Function.prototype.bind 来为事件处理函数传递参数。  
2. 通过箭头函数的方式,事件对象必须显式的传递
3. 通过bind方式,事件对象以及更加多的参数会被隐式地进行传递

```jsx
class Popper extends React.Component{
    constructor(){
        super();
        this.state = {name:'Hello world!'};
    }
    
    preventPop(name, e){    //事件对象e要放在最后
        e.preventDefault();
        alert(name);
    }
    
    render(){
        return (
            <div>
                <p>hello</p>
                {/* Pass params via bind() method. */}
                <a href="https://reactjs.org"  
                onClick={this.preventPop.bind(this,this.state.name)}>Click</a>
            </div>
        );
    }
}
```

## 条件渲染

### 通过if或条件运算符来创建表示当前状态的元素

1. 元素变量:用变量的方式来存储元素,有条件地渲染组件的一部分
2. 与运算符(&&)

```
{unreadMessages.length > 0 &&<h2>You have {unreadMessages.length} unread messages.</h2>  
//如果条件是 true，&& 右侧的元素就会被渲染，如果是 false，React 会忽略并跳过它。
```

3. 三目运算符 condition ? true : false
4. 防止组件渲染(让 render 方法返回 null 而不是它的渲染结果即可实现组件隐藏,即使被其他组件渲染)  
   ``` if(true) return null;```

## Redux数据管理

四个重要的概念

- Dispatcher:处理动作的一个分发器,四Flux引用程序中的数据流的中心枢纽,主要是将收到的行为分发给Store

  - Store:对数据进行管理

- View:React组件,主要负责View层

- Action:提供给Dispatcher,传递数据给Store

<img src="https://images.gitee.com/uploads/images/2020/0520/090214_f65ca3e8_6545143.png" style="zoom:50%;" />

## Babel

### Babel安装

`npm install babel-bli -g`

//eg：es6.js 

```js
let numbers  = [1,2,3] 
let doubleNumbers = numbers.map((number)  => number *2); 
console.log(doubleNumbers);
```

编译`babel es6.js -o compiled.js`(要先配置文件.babelrc)

```
.babelrc {    
	"presets":[],    
	"plugins":[] 
}
```



安装预设`npm install --save-dev babel-preset-es2015`

3. 然后把preset添加到配置文件中

```xml
.babelrc {    
	"presets":['es2015'],    
	"plugins":[] 
}
```

- 再次运行，打开compiled.js文件
- 安装object-rest-spread插件
- 
  1. npm install babel-plugin-transform-object-rest-spraed --save-dev
  2. 添加到配置文件

//.babelrc {    "presets":['es2015'],    "plugins":['transform-object-rest-spraed'] }

对象展开符的代码

let mike = {name:'mike',age:40}; mike = {..mike, sex, 'male' };

## 状态提升

> - 主要概念 :   将多个组件需要的共享状态提升到它们最近的父组件上, 在父组件上改变这个状态后, 然后通过props分发给只组件
>
> - 实现过程 : 一般是通过将父组件的方法传给子组件, 然后通过子组件来触发, 修改父组件的state, 然后再通过props分发给子组件

### 温度计算器

```jsx
import React from 'react'
// 展示温度有没有超过一百度
function BoilingVerdict(props) {
  return (props.celsius >= 100 ? <p>The water would boil.</p>:<p>The water would not boil</p>)
}

const scaleNames = {
  c : 'Celsius',
  f : 'Fahrenheit', 
}
// 定义子组件
// 1. 主要负责展示温度
// 2. 通过修改input的数据来触发父组件的方法来修改父组件的state, 然后再通过props传输给子组件, 然后达到修改温度的效果
class TemperatureInput extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      temperature : ''
    } 
    this.handleChanage = this.handleChanage.bind(this);
  }
  handleChanage (e) {
    // this.setState({temperature: e.target.value })
    this.props.onTemperatureChange(e.target.value )
  }
  render() {
    // const temperature = this.state.temperature
    const temperature = this.props.temperature
    const scale = this.props.scale
    return (
      <fieldset>
        <legend>Enter temperature in {scaleNames[scale]} </legend>
        <input value={temperature} onChange={this.handleChanage} />
      </fieldset>
    )
  }
}
// 父组件
// 1. 用于展示两个子组件
// 2. 定义修改state的方法, 通过props来传输给子组件来调用
// 3. 通过调用温度转换的方法, 给两个子组件传输的不同的变量值
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      temperature: '',
      scale: 'c',
    }
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this)
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this)
  }
  handleCelsiusChange(temperature) {
    this.setState({scale: 'c' , temperature})
  }
  handleFahrenheitChange(temperature) {
    this.setState({ scale: 'f', temperature })
  }
  render() {
    const {temperature, scale } = this.state;
    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius ) : temperature; 
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit ) : temperature ;
    return (
      <div className = {styles.layout}>
        <TemperatureInput 
          temperature = {celsius}
          onTemperatureChange = {this.handleCelsiusChange}
          scale = "c" /><br/>
        <TemperatureInput
          temperature = {fahrenheit} 
          onTemperatureChange = {this.handleFahrenheitChange}
          scale = "f" /><br/>
        <BoilingVerdict celsius = {celsius} />
      </div>
    )
  }
}
// 华氏度 => 摄氏度
function toCelsius(fahrenheit) {
  return (fahrenheit - 32 ) * 5 / 9;
}
// 摄氏度 => 华氏度
function toFahrenheit(celsius) {
  return (celsius * 9 / 5 ) + 32;
}

// temperature : 需要转换的温度
// convert : 转换温度实收需要用到的函数
// 判断是否为空
// 数据处理
function tryConvert(temperature, convert) {
  const input = parseFloat(temperature)
  if(Number.isNaN(input)) {
    return '';
  }
  const output = convert(input)
  // Math.round : 四舍五入
  const rounded = Math.round( output * 1000 ) / 1000;
  return rounded.toString();
}

export default Calculator;
```



## 组合&继承

###### 包含关系

1. 通过组件方式传出子元素

```jsx
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}
```

2. 通过其他组件来嵌套JSX来传递组件

```jsx
function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```

3. 特殊实例

```jsx
function Dialog(props) {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        {props.title}
      </h1>
      <p className="Dialog-message">
        {props.message}
      </p>
    </FancyBorder>
  );
}

function WelcomeDialog() {
  return (
    <Dialog
      title="Welcome"
      message="Thank you for visiting our spacecraft!" />
  );
}
```



## React.lazy

> 注意:
>
> `React.lazy` 和 Suspense 技术**还不支持服务端渲染。**如果你想要在使用服务端渲染的应用中使用，我们推荐 [Loadable Components](https://github.com/gregberge/loadable-components) 这个库。它有一个很棒的[服务端渲染打包指南](https://loadable-components.com/docs/server-side-rendering/)。

```
import OtherComponent from './OtherComponent';
```

```
const OtherComponent = React.lazy(() => import('./OtherComponent'));
```



## Emit

> 用于兄弟组件中通信
>
> 事件总线机制

bus.js

```jsx
import { EventEmitter } from 'events'
export default new EventEmitter();
```



index.js

```jsx
import Bus from './bus';
import React, { Component } from 'react'

class Index extends Component {
  state = {
    bus: '000',
  }
  render() {
    Bus.addListener('handleChangeData', (msg) => {
      this.setState({
        bus: msg,
      })
    })
    return (
      <div>
        Index{this.state.bus}
        <button onClick = {()=>Bus.emit('handleChangeData', 'btnData')}>bus</button>//可以放到其他组件进行触发
      </div>
    )
  }
}

export default Index;
```



## EventEmitter

> 自定义订阅类

### 定义

```js
// 发布订阅类
class EventEmitter {
  _event = {}

  // on 函数用于绑定
  // 注册事件监听; type 事件类型，handle 处理函数
  on(eventName, handle) {
      let listeners = this._event[eventName];
      if (!listeners || !listeners.length) {
          this._event[eventName] = [handle];
          return;
      }
      listeners.push(handle);
  }
  // off 用于移除
  // 移除事件监听；type 事件类型，handle 处理函数
  off(eventName, handle) {
      let listeners = this._event[eventName];
      this._event[eventName] = listeners.filter(l => l !== handle);
      // this._event[eventName]=[]
  }
  // emit 用于分发消息; // 触发一个事件
  emit(eventName, ...args) {
      const listeners = this._event[eventName];
      if (listeners && listeners.length) {
          for (const l of listeners) {
              l(...args);
          }
      }
  }
}
const event = new EventEmitter;
export default event;
```

### 使用

```jsx
event.on('drink', (person) => {
      console.log(person + '喝水')
})
event.on('eat', (person) => {
  console.log(person + '吃东西')
})
event.emit('drink', '我') // 我喝水
event.emit('drink', '我') // 我喝水
event.emit('eat', '其它人') // 其它人吃东西
event.emit('eat', '其它人') // 其它人吃东西
event.emit('buy', '其它人') //其它人买东西
event.emit('buy', '其它人') //这里不会再次触发buy事件，因为once只能触发一次
event.off('eat') //移除eat事件
event.emit('eat', '其它人') //这里不会触发eat事件，因为已经移除了
```



## react-demo-井字棋案例

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';


function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}

class Board extends React.Component {
  renderSquare(i) {
    return (
      <Square
        value={this.props.squares[i]}
        onClick={() => this.props.onClick(i)}
      />
    );
  }

  render() {
    return (
      <div>
        <div className="board-row">
          {this.renderSquare(0)}
          {this.renderSquare(1)}
          {this.renderSquare(2)}
        </div>
        <div className="board-row">
          {this.renderSquare(3)}
          {this.renderSquare(4)}
          {this.renderSquare(5)}
        </div>
        <div className="board-row">
          {this.renderSquare(6)}
          {this.renderSquare(7)}
          {this.renderSquare(8)}
        </div>
      </div>
    );
  }
}

class Game extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      history: [
        {
          squares: Array(9).fill(null)
        }
      ],
      stepNumber: 0,
      xIsNext: true
    };
  }

  handleClick(i) {
    const history = this.state.history.slice(0, this.state.stepNumber + 1);
    const current = history[history.length - 1];
    const squares = current.squares.slice();
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    squares[i] = this.state.xIsNext ? "X" : "O";
    this.setState({
      history: history.concat([
        {
          squares: squares
        }
      ]),
      stepNumber: history.length,
      xIsNext: !this.state.xIsNext
    });
  }

  jumpTo(step) {
    this.setState({
      stepNumber: step,
      xIsNext: (step % 2) === 0
    });
  }

  render() {
    const history = this.state.history;
    const current = history[this.state.stepNumber];
    const winner = calculateWinner(current.squares);

    const moves = history.map((step, move) => {
      const desc = move ?
        'Go to move #' + move :
        'Go to game start';
      return (
        <li key={move}>
          <button onClick={() => this.jumpTo(move)}>{desc}</button>
        </li>
      );
    });

    let status;
    if (winner) {
      status = "Winner: " + winner;
    } else {
      status = "Next player: " + (this.state.xIsNext ? "X" : "O");
    }

    return (
      <div className="game">
        <div className="game-board">
          <Board
            squares={current.squares}
            onClick={i => this.handleClick(i)}
          />
        </div>
        <div className="game-info">
          <div>{status}</div>
          <ol>{moves}</ol>
        </div>
      </div>
    );
  }
}

// ========================================

ReactDOM.render(<Game />, document.getElementById("root"));

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}

```