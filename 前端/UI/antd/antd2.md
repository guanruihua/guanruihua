---
title: antd2
date: 2020-11-23 19:33:27
tags:
- antd
- react
- ui
- front-end
---

# antd2

> [Ant Design - 一套企业级 UI 设计语言和 React 组件库](https://2x.ant.design/components/pagination-cn/)

## Pagination分页

> 采用分页的形式分隔长列表，每次只加载一个页面

## Select

### 添加搜索过滤功能

> `showSearch
> optionFilterProp="children"`

```jsx
<Select
 showSearch
 optionFilterProp="children"
>
 {options.map((item,index)=><Option key={index} value={item.id}{item.name}</Option>)}
</Select>
```

## DatePicker日期选择框

> defaultValue: 是moment对象
