---
title: react-demo
date: 2020-09-22 21:42:44
tags:
	- react
	- demo
	- javascript
---

# react-demo

### 例子1 modal 简单实现

- Modal 方法组件 (使用副作用useEffect 实现禁止滚动)

```js
import React, { useEffect, useState } from 'react';
import { createPortal } from "react-dom";
import "./index.css"

const Modal = function (props) {
    const { children, visible, closeModal } = props;
    const [bodyOverFlow, setBodyOverFlow] = useState(false);
    function handleClick(e) {
        if (e.target === e.currentTarget) {
            closeModal();
        }
    }
    const node = createPortal(
        <div className='modalRoot' onClick={(e) => handleClick(e)}>{children}</div>,
        document.body
    );
    useEffect(() => {
        console.log("初始化")
        setBodyOverFlow(window.getComputedStyle(document.body).overflow);
    }, [])
    useEffect(() => {
        if(visible){
            document.body.style.cssText = 'width: calc(100% - 17px);overflow:hidden';
        }else{
            document.body.style.cssText = `overflow:${bodyOverFlow}`;
        }
    }, [visible,bodyOverFlow])
    return <React.Fragment>{visible && node}</React.Fragment>
}
export default React.memo(Modal);
```

- button 方法组件（hook - 使用了useState）

```js
import React, { useState } from 'react';
import Modal from "../modal";
import "./index.css";

const ButtonComponent = function () {
    /**
     * 定义visible状态
     */
    const [modalVisible, setModalVisible] = useState(false);
    const modalConfig = {
        visible: modalVisible,
        closeModal: () => {
            setModalVisible(false)
        }
    }
    const modalChildren = <div className={"container"}>
        <div className={"closeBtn"} onClick={() => setModalVisible(false)}></div>
    </div>
    return <div className={'mainContainer'}><button onClick={() => setModalVisible(true)}>点击打开</button><Modal {...modalConfig}>{modalChildren}</Modal></div>
}

export default React.memo(ButtonComponent);
```

- App.js

```js
import React from 'react';
import Button from "./component/button";
import './App.css';

export default class App extends React.Component {
  render() {
    return (
      <div className="App">
        <Button></Button>
      </div>
    );
  }
}
```

1. 这个例子只使用了hook中的useState,以及useEffect 较为简单
2. 使用了Portals 将modal的Dom节点放置在body的直接资子元素的位置

### 例子2 获取滚动条的宽度

- 使用场景

```
在打开modal层时 将body的overflow设置为了hidden 所以页面会往右边闪动。闪动的距离为滚动条的宽度
处理方式: 在body的width设置为calc(100%+滚动条的宽度)即可。
```

- 思路

```
通过给body动态添加一个dom节点，计算这个dom节点的offsetWidth和clientWidth的差(即滚动条的宽度)
然后通过dom.remove() 删除这个都节点
```

- useGetScrollBarWidth/index.js

```js
import React, { useEffect, useState } from 'react';

/**
 * 自定义hook - 获取滚动条的宽度
 * @returns {*[]}
 */
const useGetScrollBarWidth = function() {
  const [scrollBarWidth, setScrollBarWidth] = useState(0);
  useEffect(() => {
    const bodyEl = document.body;
    const tempEl = document.createElement('p');
    const styles = {
      width: '100px',
      height: '100px',
      overflowY: 'scroll'
    };
    for (let i in styles) {
      tempEl.style[i] = styles[i];
    }
    bodyEl.appendChild(tempEl);
    setScrollBarWidth(tempEl.offsetWidth - tempEl.clientWidth);
    tempEl.remove();
  }, []);

  return [scrollBarWidth];
};

export default useGetScrollBarWidth;
```

### 例子3 绑定监听事件

```js
import React, { useState, useEffect, useRef } from 'react';

const useEventListener = function(eventName, handler, element = document) {
  const savedhandler = useRef();
  /**
   * 当handler 发生变化时  修改 存储在savedhandler中的回调函数
   */
  useEffect(
    () => {
      savedhandler.current = handler;
    },
    [handler]
  );
  /**
   * 当eventName和element 发生变化时 调用
   */
  useEffect(
    () => {
      // 判断当前element是否支持addEventListern
      const isSupport = element && element.addEventListener;
      /**
       * 如果当前元素不支持 - 直接返回false 跳出函数
       */
      if (!isSupport) return false;

      /**
       * 将事件对象传入handler函数
       * @param e
       */
      const eventListern = e => {
        savedhandler.current(e);
      };

      element.addEventListener(eventName, eventListern);
      /**
       * UmMount时 - 清除监听器
       */
      return () => element.removeEventListener(eventName, eventListern);
    },
    [eventName, element]
  );
};
export default useEventListener;
```
