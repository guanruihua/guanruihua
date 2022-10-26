---
title: React, Redux, React-Redux
time: 2020-11-18 11:35:55
tags:
- redux
- react
- react-redux
---

# React, Redux, React-Redux

> React : 负责组件的UI界面渲染
>
> Redux : 数据处理中心
>
> React-Redux : 连接组件和数据中心, 就是和react和redux联系起来

## react生命周期

1. `getDefaultProps()`

设置默认的props，es6中用 `static dufaultProps={}` 设置组件的默认属性。在整个生命周期只执行一次。

2. `getInitialState()`

在使用es6的class语法时是没有这个钩子函数的，可以直接在constructor中定义this.state。此时可以访问this.props。

3. `componentWillMount()` 

    ajax数据的拉取操作，定时器的启动。

组件初始化时调用，以后组件更新不调用，整个生命周期只调用一次，此时可以修改state。

4. `render()`

React最重要的步骤，创建虚拟dom，进行diff算法，更新dom树都在此进行。此时就不能更改state了。

5. `componentDidMount()`    动画的启动，输入框自动聚焦

组件渲染之后调用，可以通过this.getDOMNode()获取和操作dom节点，只调用一次。

二、在更新时也会触发5个钩子函数：

2. `componentWillReceivePorps(nextProps)`

组件初始化时不调用，组件接受新的props时调用。不管父组件传递给子组件的props有没有改变，都会触发。

3. `shouldComponentUpdate(nextProps, nextState)`

React性能优化非常重要的一环。组件接受新的state或者props时调用，我们可以设置在此对比前后两个props和state是否相同，如果相同则返回false阻止更新，因为相同的属性状态一定会生成相同的dom树，这样就不需要创造新的dom树和旧的dom树进行diff算法对比，节省大量性能，尤其是在dom结构复杂的时候。不过调用this.forceUpdate会跳过此步骤。

4. `componentWillUpdate(nextProps, nextState)`

组件初始化时不调用，只有在组件将要更新时才调用，此时可以修改state

5. `render()`

不多说

6. `componentDidUpdate()`

组件初始化时不调用，组件更新完成后调用，此时可以获取dom节点。

三、卸载钩子函数

7. `componentWillUnmount()`   定时器的清除

组件将要卸载时调用，一些事件监听和定时器需要在此时清除。

<img src="https://images.gitee.com/uploads/images/2020/1118/114419_53fa0c37_7984151.png" style="zoom:67%;" />

## Redux

> 1. 唯一数据源
> 2. 状态只读
> 3. 数据改变指定通过纯函数(reducer)完成

### 核心api

> store, reducer, action

#### store

![](https://images.gitee.com/uploads/images/2020/1118/114823_9f073716_7984151.png)

> getState():  存储的数据, 状态树
>
> dispatch(action): 分发action, 并返回action, 这是唯一能改变store中数据的方式
>
> subscrible(listener) : 注册一个监听者, store发生变化时候被调用



#### reducer

<img src="https://images.gitee.com/uploads/images/2020/1118/115210_465f7e83_7984151.png" style="zoom:67%;" />