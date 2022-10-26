## Window对象 BOM

> 浏览器对象模型:BOM(Browser Object Model )

### Window尺寸&获取元素的相对位置

> Internet Explorer、Chrome、Firefox、Opera 以及 Safari：
>
> - window.innerHeight - 浏览器窗口的内部高度
> - window.innerWidth - 浏览器窗口的内部宽度
>
> Internet Explorer 8、7、6、5 ：
>
> - document.documentElement.clientHeight
> - document.documentElement.clientWidth
>
> 或者
>
> - document.body.clientHeight
> - document.body.clientWidth

| js代码                                              | 描述                                                         |
| --------------------------------------------------- | ------------------------------------------------------------ |
| element.clientWidth / clientHeight                  | 内高度 / 宽度: margin + content                              |
| element.offsetWidth / offsetHeight                  | 外高度 / 宽度:border + padding + content                     |
| element.clientTop / clientLeft                      | 上/左 边框                                                   |
| element.getBoundingClientRect()                     | x \ y:              元素的左上角和父元素左上角的距离 <br>width / height: 边框 + 内边距 + 内容框 <br/>top:                元素的上边界和父元素上边界的距离 <br/>left:                元素的左边界和父元素左边界的距离 <br/>right:              元素的右边界和父元素的左边界的距离 <br/>bottom:          元素的下边界和父元素上边界的距离 |
| element.offsetTop / element.offsetLeft              | 上边 / 左边 的偏移量                                         |
| document.documentElement.clientWidth / clientHeight | 可视区域的大小                                               |
| document.documentElement.scrollWidth / scrollHeight | 页面的实际大小                                               |
| window.screenX / screenY                            | 窗口左上角与屏幕左上角的距离                                 |
| window.screen.width / height                        | 屏幕可用宽高( 去除任务栏 )                                   |
| window.innerWidth / innerHeight                     | 窗口的内高度 / 宽度                                          |
| window.outerWidth / outerHeight                     | 窗口的外高度 / 宽度                                          |
|                                                     |                                                              |

### Window对象

#### 属性

> - 状态栏
>   - defaultStatus 改变浏览器状态栏的默认显示
>   - status          临时改变浏览器状态栏的显示
> - 窗口位置
>   - IE
>     - screenLeft  声明窗口的左上角的X坐标
>     - screenTop   声明窗口的左上角的Y坐标
>     - 声明当前文档向右滚动的像素数
>       - document.body.scrollLeft
>       - document.documentElement.scrollLeft
>     - 声明当前文档向下滚动过的像素数
>       - document.body.scrollTop
>       - document.documentElemnet.scrollTop
>   - !IE
>     - screeenX  声明窗口左上角的==X==坐标
>     - screenY     声明窗口左上角的==Y==坐标
>     - pageXOffset    声明当前文档向右滚动过的像素数
>     - pageYOffset     声明当前文档向下滚动过的像素数
>   - FF
>     - innerHeight    返回窗口的文档显示区的高度
>     - innerWidth      返回当前的文档显示区的宽度
>     - outerHeight     返回窗口的外部高度
>     - outerWidth       返回窗口的外部宽度
> - 其他属性

#### navigator(导航器对象)

>  - appCodeName   返回浏览器的代码名
>  - appName      返回浏览器的名称
>  - appVersion    返回浏览器的平台和版本信息
>  - cookieEnabled   返回指明浏览器中是否启动cookie的布尔值
>  - platform    返回浏览器的操作系统平台
>  - userAgent   返回客户机发送服务器的user-agent头部的值

#### screen(显示器对象)

>  - avaiHeight    返回显示器的可用高度
>  - avaiWidth     返回显示屏幕的可用宽度
>  - height      返回屏幕高度的像素数
>  - width       返回屏幕宽度的像素数
>  - colorDepth   返回屏幕颜色的位数

#### history(历史对象)

>- back()     返回前一个URL
>- forward()   返回下一个URL
>- go()     返回某个具体页面
>- 添加和修改历史记录中的条目
>
> - pushState()  添加
> - replaceState()  修改
> - popstate

```js
//history向后跳转
window.history.back()    =>   window.history.go(-1)
window.history.forward();  =>  window.history.go(1)
let numberOfEntries = window.history.length//获取堆栈中页面的数量
let stateObj = {
	foo: 'bar'
}
history.pushState( stateObj, "page 2", "bar.html");
history.replaceState(stateObj, "page 3", "bar2.html");

window.onpopstate = function(event) {
  console.log("location: " + document.location + ", state: " + JSON.stringify(event.state));
};
//绑定事件处理函数. 
history.pushState({page: 1}, "title 1", "?page=1");    //添加并激活一个历史记录条目 http://example.com/example.html?page=1,条目索引为1
history.pushState({page: 2}, "title 2", "?page=2");    //添加并激活一个历史记录条目 http://example.com/example.html?page=2,条目索引为2
history.replaceState({page: 3}, "title 3", "?page=3"); 	//修改当前激活的历史记录条目 http://ex..?page=2 变为 http://ex..?page=3,条目索引为3
history.back(); 																	// 弹出 "location: http://example.com/example.html?page=1, state: {"page":1}"
history.back(); 																	// 弹出 "location: http://example.com/example.html, state: null
history.go(2);  																	// 弹出 "location: http://example.com/example.html?page=3, state: {"page":3}
```



#### location(位置对象)

>  - 属性
>    - hash    设置或返回从#号开始的URL
>    - host     设置或返回主机名或当前URL的端口号
>    - hostname   设置或返回当前URL的主机名
>    - href     设置或返回完整的URL
>    - pathname    设置或返回当前URL的路径部分
>    - port    设置或返回当前URL的端口号
>    - protocol    设置或返回当前URL的端口号
>    - search    设置或返回?开始的URL
>  - 方法
>    - assign(URL)    加载新的文档
>    - reload()      重现加载当前页面
>    - replace(newURl)    用新的完蛋替换当前文档

#### document(文档对象)

##### 集合

> - anchors[]  锚点对象数组
> - images[]    图片对象数组
> - links[]        连接对象数组
> - forms[]      表单对象数组

##### 属性

> - cookie  设置或返回与当前文档有关所有cookie
> - domain   返回当前文档的域名
> - referrer   返回载入当前文档的文档的URL
> - title 返回当前文档的标题
> - URL 返回当前文档的URL

##### 方法

> - open()   打开一个新的文档, 并查处旧文档内容
> - close()   关闭文档输出流
> - write()    向当前文档追加写入文本
> - writeIn()   与wtite()相同,在<pre>中追加换行



#### 窗口控制

>- moveBy:
>- moveBy(水平位移量, 垂直位移量) 
>- 按照指定像素移动指定窗口
>- moveTo:
>- moveTo(x, y)
>- 将窗口移动到指定坐标(x, y)
>- resizeBy:
>- resizeBy(x, y)
>- 将当前窗口改变指定的大小(x, y)
>- 当x, y 的值大于0时为扩大
>- 当x, y 的值小于0时为缩小
>- resizeTo
>- resizeTo(x, y)
>- 将当前窗口改编成(x, y)大小, x, y分别为宽度和高度
>- scrollBy
>- scrollBy(x, y)
>- 将窗口中的内容给定的唯一量滚动
>- 参数为正数, 正向滚动, 反之, 反向
>- scrollTo
>- scrollTo(x, y)
>- 将窗口内容滚动到指定位置

#### 焦点控制

> - focus 得到焦点
> - blur    移出焦点

#### 打开关闭窗口

>- open
>- open("URL", "窗口名称", "窗口风格")
>- 打开一个新的窗口, 并在窗口中转载指定URL地址的网页
>- 窗口风格:
>  - height    数值    窗口高度   不能小于100
>  - width      数值    窗口宽度    不能小于100
>  - left    数值    窗口左坐标   不能为负值
>  - top    数值    窗口上坐标   不能为负值
>  - location     是否显示地址栏
>  - menubar     是否显示菜单栏
>  - resizable      是否可以改变窗口大小
>  - scrollbars   是否允许出现滚动栏
>  - status    是否显示状态栏
>  - toolbar   是否显示工具栏
>- close
>- close()
>- 自动关闭浏览器窗口

#### 定时器

>- setTimeout(将执行代码,  毫秒):定时器
>- 到了指定时间后执行代码
>- clearTimeout(定时器)
>- 取消由setTimeout设置的定时器
>- setInterval(重复执行的代码, 毫秒):时间间隔器
>- 指定周期重复执行功能代码
>- clearInterval(时间间隔器)
>- 取消由setInterval()设置的时间间隔期

#### 对话框

> - alert("提示字符串")
> - confirm("提示字符串")
>   - 确认=>返回true, 反之false
> - prompt("提示字符串", "缺省文本")
>   - 返回输入的字符串
>   - 点击取消   返回null

