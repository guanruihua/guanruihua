---
title: jquery
date: 
tags:
	- javascript
	- front-end
	- jquery
---

# jQuery

> - 官网：http://jquery.com/
> - 官网API文档：http://api.jquery.com/
> - 中文汉化API文档：http://www.css88.com/jqapi-1.9/
> - [ jQuery 3.3.1) (html.cn)](https://www.html.cn/jqapi-1.9/category/utilities/)

在用 js 写代码时，会遇到一些问题：

- window.onload 事件有事件覆盖的问题，因此只能写一个事件。
- 代码容错性差。
- 浏览器兼容性问题。
- 书写很繁琐，代码量多。
- 代码很乱，各个页面到处都是。
- 动画效果很难实现。

![img](jquery.assets/20180204_1710.png)

```js
style.
	div{
    display: none;
  }

html
	button | 显示五个div盒子和设置内容
	- for(var i=0; i< 5; i++)
		div


<script src="https://cdn.bootcdn.net/ajax/libs/jquery/1.10.0/jquery.min.js"></script>
<script>
  $(document).ready(function() {
  var btn = $('button')
  var div = $('div')
  btn.click(function() {
    div.show(1000);
    div.html('tomorrow!');
    div.show(3000).html(1111);
  })
})
</script>
```

## jquery两大特点

1. **链式编程**: 比如`.show()`和`.html()`可以连写成`.show().html()`

   > 链式编程原理: return this

2. **隐式迭代**: 在方法的内部会匹配到所有元素进行循环遍历, 执行响应的方法; 而不用我们在进行循环, 简化操作

   > 多元素的值, 大部分情况会返回第一个元素的值



## jquery版本

> jquery-1.11.1.js: 未压缩版
>
> jquery-1.11.1.min.js:压缩版( 项目上线时候使用)

## jquery入口函数

| 写法                                     | 描述                                                   |
| ---------------------------------------- | ------------------------------------------------------ |
| `$(document).ready(function () { ... })` | 文档加载完毕，**图片不加载**的时候，就可以执行         |
| `$(function (){ ... })`                  | 文档加载完成后, **图片不加载**的时候就执行             |
| `$(window).ready(function(){ ... })`     | 文档加载完毕, **图片也加载完毕**的时候, 在执行这个函数 |

**jQuery入口函数与js入口函数的区别**：

区别一：书写个数不同：

- Js 的入口函数只能出现一次，出现多次会存在事件覆盖的问题。
- jQuery 的入口函数，可以出现任意多次，并不存在事件覆盖问题。

区别二：执行时机不同：

- Js的入口函数是在**所有的文件资源加载**完成后，才执行。这些**文件资源**包括：页面文档、外部的js文件、外部的css文件、图片等。
- jQuery的入口函数，是在文档加载完成后，就执行。文档加载完成指的是：DOM树加载完成后，就可以操作DOM了，不用等到所有的**外部资源**都加载完成。

文档加载的顺序：从上往下，边解析边执行。

## jquery基础

> $===jQuery
>
> jQuery对象转换为DOM对象: `jqbox = $(mybox)`
>
> ```js
> $(); // 调用上面我们自定义的函数$
> $(document）.ready(function(){}); // 调用入口函数
> $(function(){}); // 调用入口函数
> $(“#btnShow”) // 获取id属性为btnShow的元素
> $(“div”) // 获取所有的div标签元素
> ```
>
> 

## 选择器

### jquery基本选择器

![img](jquery.assets/20180204_2125.png)

### 层级选择器

![img](jquery.assets/20180204_2138.png)

### 基本过滤选择器

![img](http://img.smyhvae.com/20180204_2150.png)

> `:not(selector) `  : 除了selector都要选择$(":not(selector)")
>
> `:header `: 选择标题元素(就是h标签)$(":header")



### 属性选择器

![img](jquery.assets/20180204_2155.png)

## 过滤器

### 子元素过滤器

```js
:first-child
:last-child
:nth-child():选择第n个子元素
:nth-last-child():选择倒数第n个子元素
:only-child选择唯一的子元素
```

### 内容过滤器

```js
:contains(str):选择内容含有str的元素
:empty:选择元素内容为空的
:parent:选择拥有子节点的
:has():选择包含指定选择器
```

### 可见过滤器

> :hidden  	不可见元素
>
> :visible	可见元素

### 筛选选择器

![img](jquery.assets/20180204_2200.png)

## 事件

### **常用事件**

```js
$(selector).action(function(){.......});
```

### 常见文档/窗口事件

```js
ready()就绪

load()加载

unload()卸载(跳到其他页面)
```

### 键盘事件

```js
keydown()按下

keypress()按下并快速释放

keyup()释放
```

### 鼠标事件

```js
click()

dbclick()

hover()

mousedown()

mouseenter()鼠标刚进入被选中元素

mouseleave()鼠标刚离开被选中元素

mousemove()鼠标移动事件

mouseout()离开(离开子元素也会触发)

mouseover()穿过

mouseup()鼠标按键被释放事件

toggle()一次绑定多个函数,指定元素被点击依次执行其中一个函数

$(selector).toggle(function(){},function(){},function(){}....);
```

### **JQuery事件绑定 解除**

#### 事件绑定

> bind()可以给指定元素绑定一个或多个事件

```js
//绑定一个事件
$(selector).bind(event(事件名称),[data,] function(){})  
//可以绑定多个事件 
$(selector).bind(event1(事件名称) event2(事件名称) event3(事件名称),[data,] function(){})  
//可以绑定多个事件和方法
$(selector).bind({
    event1(事件名称),[data,] function(){},    	
    event2(事件名称),[data,] function(){},    
    event3(事件名称),[data,] function(){}}
  )
```

delegate():给指定元素的**子元素**绑定一个或多个事件

```js
$(selector).delegate(childSelector, event, [data,] function)
```

==on():给指定元素或子元素绑定一个或多个事件(包含前两个的用法)==

```js
$(selector).on(event,[childSelector,] [data,] function)
```

#### 事件解除

```js
//unbind()

$(selector).unbind([event] [,function]) 

//undelegate

$(selector).undelegate([childSelector] [,event] [,function]) 

//off()

$(selector).off(event, [childSelector,] [data,] function)
```

### **jQuery特效**

#### **隐藏和显示**

```js
hide()

show()

toggle()用来切换hide和show
```

#### **淡入和淡出**

> 【duration可以选择slow，fast】

```js
fadeIn():淡入

fadeOut():淡出直到隐藏

fadeToggle():切换淡入和淡出

fadeTo():用于将元素变成指定通明度

$(selector).fadeTo(duration(持续时间), opacity(通明度) [, callback])
```

#### **滑动**

```js
slideDown():向下滑动

slideUp():向上滑动

slideToggle():切换向上向下滑动
```

#### **动画**

> animate({params} [,duration], [,callback])
>
> > params是形成动画的css属性(可以设置多个)
> >
> > duration:表示规定的效果时长
> >
> > callback:动画结束后执行的函数

#### **方法链接**

```js
$(selector).action1(),action2().action3()...;
```

#### **停止动画**

```js
$(selector).stop([stopAll] [, gotoEnd]);//这两个都是布尔值
```

## **jQuery HTML DOM**

### jQuery获取和设置

#### 获取和设置常用的方法

```js
text():文本内容

html():包括html标签本身

val():表单字段的值

attr():指定元素的属性值

css():指定元素的css值
```

#### jQuery添加

```js
append():在指定元素内部的结尾插入内容

prepend():在只当以元素的内部的开头插入内容

after():在指定元素之后添加内容

before():在指定元素之前添加内容
```

#### 删除remove();

```js
$("p").remove(".style01");//删除class="style01"所有的p标签
```

#### 清空empty();

#### 清空removeAttr();

> 删除元素的指定属性

#### 类属性

```js
addClass()//添加class
removeClass()
toggleClass()
```

#### 尺寸

```js
width:
height
innerWidth包括内边距
innerHeight
outerWidth包括内边距和边框
outerHeight
```

### **jQuery遍历**

**后代遍历**

```js
$(selector).children([selector2] [".style1"])
$(selector).find(selector2)//查找后代全部元素
$(selector).filter(selector2)//过滤器,只会选择符合selector2的元素
```

**同胞遍历(具有相同的父元素)**

```js
siblings:所有兄弟节点
next:指定的下一个
nextAll:指定元素后全部
nextUntil(selector,selector):指定元素后的指定范围
prev:指定的前一个
prevAll:指定前全部
prevUntil(selector,selector):指定元素前的指定范围
```

**祖先遍历(父,祖父,曾祖父等)**

```js
parent():指定元素的直接父元素
parents():指定元素的全部祖先元素
parentsUntil():指定元素向上指定范围的所有祖先元素
```

## **AJAX技术**

### **load()方法**

> $(selector).load(URL [, data] [, callback])
>
> - URL规定获取数据的URL地址,可以是文本,XML,JSON
>
> - data:用于规定与请求一起发送给服务器的字符串(该字符串以键/值对合集)
>
> - callback:load()完成后执行的函数

```js
$("#demo").load("test.txt")

$("#demo").load("test.txt div.style01")

$(selector).load(URL [, data] ,function(response, status,xhr){    
  //回调函数代码    
  //response:调用成功时的结果内容    
  //status:调用状态,eg:success 和 error    
  //xhr:表示XMLHttpRequest对象 
})
```

### **get()方法**

> $.get(URL  [, data] [, success] [, dataType]);
>
> ​	dataType规定从服务器端获取的数据类型,eg:XML,JSON,HTML等

### **post()方法**

> $.post(URL [, data] [, success] [, dataType]); 

### **ajax()方法**

```js
$.ajax({    
  url: "demo.php",    
  method:"POST",    
  data:{username:"admin", password:"123"},    
  dataType: "json",    
  success:function(){alert("请求成功");},    
  error: function(){alert("请求失败");} 
});
```

