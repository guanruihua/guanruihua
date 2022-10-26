# Select

## Select自带的搜索功能很多时候需要结合后端的接口，输入一个关键字的时候会自动更新选择器的选项

> 选择器选项必须和每次更新的数据挂钩, 这个值可以通过state,也可以通过props拿到
>
> 再结合循环的方法例如map遍历渲染options

```jsx
  <Select
  mode="multiple"         //多选模式
  placeholder="请选择"
  filterOption={false}    //关闭自动筛选
  onSearch={this.handleSearch}
>
  {
    list.map((item, index) => (
      <Option key={index} value={item}>{item}</Option>
    ))
  }
</Select>
```

> 若`filterOption`默认为`true`, 当你输入内容时候,会先在已有选项里面寻找符合项, 无论是否找到，都会重新渲染Options,这样你接口请求的数据的渲染被覆盖了, 自然看不到结果了。所以需要把它关掉
