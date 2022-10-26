# [react-window]()

> [源码](https://github.com/bvaughn/react-window)
> [api](https://react-window.vercel.app/#/examples/list/fixed-size)

1、不用全部加载出所有的DOM节点。默认只渲染可视区域及可视区域外的一个节点，此属性可自定义设置。
2、可用于处理大型数据列表。当使用在大型数据列表中，可避免因为数据的更新而导致大量的重新渲染。

## FixedSizeList（固定尺寸的列表）

```js

import { FixedSizeList } from 'react-window';
/**
    * 每个列表项的组件
    * @param index：列表项的下标；style：列表项的样式（此参数必须传入列表项的组件中，否则会出现滚动到下方出现空白的情况）
    **/
const Row = ({ index, style }) => (
  <div style={style}>Row {index}</div>
);

const Example = () => (
  <FixedSizeList
    height={150} // 列表可视区域的高度
    itemCount={1000} // 列表数据长度
    itemSize={35} // 列表行高
    width={300} //列表可视区域的宽度
  >
    {Row}
  </FixedSizeList>
);
```

## VariableSizeList （可变尺寸列表）

```jsx

import { VariableSizeList } from 'react-window';

const rowHeights = new Array(1000)
  .fill(true)
  .map(() => 25 + Math.round(Math.random() * 50));

const getItemSize = index => rowHeights[index]; // 此处采用随机数作为每个列表项的高度
 /**
    * 每个列表项的组件
    * @param index：列表项的下标；style：列表项的样式（此参数必须传入列表项的组件中，否则会出现滚动到下方出现空白的情况）
    **/
const Row = ({ index, style }) => (
  <div style={style}>Row {index}</div>
);

const Example = () => (
  <VariableSizeList
    height={150} // 列表可视区域的高度
    itemCount={1000} // 列表数据长度
    itemSize={getItemSize} // 设置列表项的高度
    layout= "vertical" // （vertical/horizontal） 默认为vertical，此为设置列表的方向
    width={300}
  >
    {Row}
  <VariableSizeList>
);
````

## 结合`react-virtualized-auto-sizer`使列表自适应当前页面的宽高

> 使用AutoSizer可是列表宽高为当前父组件的100%

import { FixedSizeList } from "react-window";
import AutoSizer from "react-virtualized-auto-sizer";
const Example = () => (
  <AutoSizer>
    {({ height, width }) => (
      <FixedSizeList
        className="List"
        height={height}
        itemCount={1000}
        itemSize={35}
        width={width}
      >
        {Row}
      </FixedSizeList>
    )}
  </AutoSizer>
);

## 常见问题

> 在使用VariableSizeList 会遇到列表项样式缓存没有被清除导致行高一直和第一次可视区域里展示的一样。可使用组件的属性`resetAfterIndex(index: number, shouldForceUpdate: boolean = true): void`来清除样式。

```jsx

class Example extends Component {
constructor(props) {
        super(props)
        this.myRef = React.createRef()
    }
......

render() {
......
  <VariableSizeList
    className='friends-list'
    height={66}
    itemCount={100}
    itemSize={64}
    itemSize={getItemSize}
    ref={this.myRef}
    width={260}>
    {Row}
  </VariableSizeList>
......
<button onClick={()=>{this.myRef.current.resetAfterIndex(0, false)}}>
    清除样式
</button>
......
}
}

```
