---
title: createElement
datae: 2020-12-10 10:18:28
tags:
- react
- expand
---

# React.createElement



> 参数:
> 1. 必填,  传入类HTML标签的名称
> 2. 选填,  属性, 例如:className
> 3. 选填,  子节点, eg: 要显示的文本内容

```jsx
import React, { Component } from 'react'
// 3 
function Content3 () {
  return React.createElement('div', null, 'content3')
}

class Index extends Component {
  render() {
    let child1 = React.createElement('li', null, 'one' )
    let child2 = React.createElement('li', null, 'two')
    let content = React.createElement('ul', { className : 'teststyle'}, child1, child2) //1 
    let content2 = React.createElement('ul', { className : 'teststyle'}, [child1, child2]) //2
    return (
      <div>
        Index
        {content}
        { content2 }
        <Content3 />
      </div>
    )
  }
}

export default Index;
```

![Snipaste_2020-12-10_10-35-35](https://gitee.com/grh-gitee/picgo/raw/master/Snipaste_2020-12-10_10-35-35.png)