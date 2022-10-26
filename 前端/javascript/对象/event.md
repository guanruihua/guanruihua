## Event对象

### event对象常见方法

> 1. event.preventDefault(): 取消事件的默认动作
> 2. event.stopoPropagation(): 阻止事件冒泡
> 3. event.stopImmediatePropagation(): 
>    1. 阻止剩下的事件处理程序被执行,	
>    2. 如果一个元素绑定了三个事件, 其中有一个调用了该方法, 其他两个事件将不会被执行

