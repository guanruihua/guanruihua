# antd4



## Form

> initialValue: 和antd2的使用差别, 使用function组件, 只会挂载第一次初始化的值, 如果通过mobx管理的数据, 不会挂载修改后的数据, 只会挂载第一次的数据, 可以通过useForm , form.setFielsValue(valueObj) 后重置数据