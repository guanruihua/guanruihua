## DOM事件

### 事件级别

> DOM0: element.onclick = function(){}
>
> DOM2: element.addEventListener('click', function(){}, false)
>
> DOM3: element.addEventListener('keyup', function(){}, false)
>
> 没有DOM1, 因为DOM1标准制定的时候没有涉及DOM事件
>
> DOM3比DOM2添加了一些事件类型



### DOM事件捕获的具体流程

> 捕获流程:  window -> document -> html -> body- >...-> 目标元素
>
> 冒泡流程: 目标元素 -> ...-> body -> html - > document -> window



## DOM操作

### EventListener()方法

> - 添加事件监听器
>   - 语法 : `element.addEventListener( *event, function, useCapture* );`
>     - 参数1: 事件类型(不要使用"on"前缀, 例如"click", 而不是"onclick")
>     - 参数2: 时间出发调用函数
>     - 参数3: 是个布尔值(false: 冒泡, true:捕获[ 可选])
>   - eg: `document.getElementById("btn").addEventListener("click",myFunction);`
> - 移除事件监听器
>   - eg:`element*.removeEventLister("mousemove", myFunction);`

### 获取节点

> - document
>   - getElementById([元素ID])
>   - getElementByName([元素name属性])
>   - getElementsByTagName([元素标签])
> - 节点指针
>   - 父节点.firstChild : 获取元素的首个节点
>   - 父节点.lastChild : 获取元素最后一个节点
>   - 父节点.childNodes : 获取元素的==子节点列表==
>   - 兄弟节点.previousSibling : 获取已知节点的前一个节点
>   - 兄弟节点.nextSibling : 获取已知节点后一个节点
>   - 子节点.parentNode : 获取已知节点的父节点

### 节点操作

> - 创建节点 : 
>   - createElement
>     - document.createElement
>     - 创建元素节点
>   - createAttibute
>     - document.createAttibute
>     - 创建属性节点
>   - createTextNode
>     - document.createTextNode
>     - 创建文本节点
> - 插入节点
>   - appendChild
>     - appendChild(所添加的新节点)
>     - 向节点的子节点列表的末尾添加新的节点
>   - insertBefore
>     - insertBefore(所添加的新节点[已知子节点])
>     - 在已知节点前插入一个新的节点
> - 替换节点
>   - replaceChild
>     - replaceChild(要插入的新元素, 将被替换的老元素)
>     - 将某个子节点替换成另一个
> - 复制节点
>   - cloneNode
>     - 需要被赋值的节点.cloneNode(true/false)
>     - 创建指定节点的副本
>     - true : 复制当前节点以及所有的子节点
>     - false : 仅复制当前节点
> - 删除节点 : 
>   - removeChild(要删除的节点)
>   - 删除指定的节点
>     - 删除父节点的一个子节点 : document.getElementById("id1").parentNode.removeNode(document.getElementById("id2"))

### 属性操作

> - 获取属性: getAttribute([元素属性名])
>   - 元素节点.getAttribute(元素属性名)
>   - 获取元素节点指定的属性值
> - 设置属性: setAttribute([属性名], [属性值])
>   - 元素节点.setAttribute(属性名, 属性值)
>   - 创建或改变元素节点的属性
> - 删除属性: removeAttribute([属性名])
>   - 元素节点.removeAttribute(属性名)
>   - 删除元素中的指定属性

### 文本操作

> - insertData(offset, String)   :    从offset指定位置插入string
>
> - appedData(String)   :   将string插入到文本节点末尾处
>
> - deleteData(offset, count)  :    从offset起杀出count个字符
> - replaceData(off, count, string)    :    从off将count个字符用string替代
> - splitData(offset)   :   从offset起将文本节点分成两个节点
> - substring(offset, count)    :   返回有offset 起的count个节点





