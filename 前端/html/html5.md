# HTML5

## 表单

### 输入类型

#### email

`E-mail: <input type="email" name="user_email" />`

> ==验证email域==的值

#### url

`Homepage: <input type="url" name="user_url" />`

> ==验证url域==的值

#### number

`Points: <input type="number" name="points" min="1" max="10" />`

> 限制接受数字的范围
>
> max:最大值; min:最小值; step:间隔; value:默认值;

#### range

`<input type="range" name="points" min="1" max="10" />`

> 包含一定范围数字值的输入域(类型为滑动条)

#### Date pickers(date, month, week, time, datetime, datetime-local )(日期选择器)

```html
Date: <input type="date" name="user_date" />
```

> - date - 选取日、月、年
> - month - 选取月、年
> - week - 选取周和年
> - time - 选取时间（小时和分钟）
> - datetime - 选取时间、日、月、年（UTC 时间）
> - datetime-local - 选取时间、日、月、年（本地时间）

#### search

> search 类型用于搜索域，比如站点搜索或 Google 搜索。
>
> search 域显示为常规的文本域。

#### color

> 选颜色

### 表单元素

#### datalist

> datalist 元素规定输入域的选项列表。
>
> 列表是通过 datalist 内的 option 元素创建的。
>
> 如需把 datalist 绑定到输入域，请用输入域的 list 属性引用 datalist 的 id
>
> option里面的value一定要设置属性

```html
Webpage: <input type="url" list="url_list" name="link" />
<datalist id="url_list">
  <option label="W3School" value="http://www.W3School.com.cn" />
  <option label="Google" value="http://www.google.com" />
  <option label="Microsoft" value="http://www.microsoft.com" />
</datalist>
```

#### keygen

> keygen 元素的作用是提供一种验证用户的可靠方法。
>
> keygen 元素是密钥对生成器（key-pair generator）。当提交表单时，会生成两个键，一个是私钥，一个公钥。
>
> 私钥（private key）存储于客户端，公钥（public key）则被发送到服务器。公钥可用于之后验证用户的客户端证书（client certificate）。
>
> 目前，==浏览器对此元素的糟糕的支持度不足以使其成为一种有用的安全标准==。

```html
<form action="demo_form.asp" method="get">
  Username: <input type="text" name="usr_name" />
  Encryption: <keygen name="security" />
  <input type="submit" />
</form>
```

#### output

> output 元素用于不同类型的输出，比如计算或脚本输出：

<!DOCTYPE HTML>
<html>
<head>
<script type="text/javascript">
function resCalc()
{
numA=document.getElementById("num_a").value;
numB=document.getElementById("num_b").value;
document.getElementById("result").value=Number(numA)+Number(numB);
}
</script>
</head>
<body>
<p>使用 output 元素的简易计算器：</p>
<form onsubmit="return false">
 <input id="num_a" /> +
 <input id="num_b" /> =
 <output id="result" onforminput="resCalc()"></output>
</form>
  </body>
</html>
### 表单属性

#### 新的 form 属性

##### autocomplete

- > 相当于有记忆功能
  >
  > 适用于:<input> 标签：text, search, url, telephone, email, password, datepickers, range 以及 color。

```html
<form action="/example/html5/demo_form.asp" method="get" autocomplete="on">
First name:<input type="text" name="fname" /><br />
Last name: <input type="text" name="lname" /><br />
E-mail: <input type="email" name="email" autocomplete="off" /><br />
<input type="submit" />
</form>

<p>请填写并提交此表单，然后重载页面，来查看自动完成功能是如何工作的。</p>
<p>请注意，表单的自动完成功能是打开的，而 e-mail 域是关闭的。</p>
```

##### novalidate

> novalidate 属性规定在提交表单时不应该验证 form 或 input 域。

#### 新的 input 属性

##### autofocus

> 页面加载时,自动获得焦点

```html
User name: <input type="text" name="user_name"  autofocus="autofocus" />
```

##### form

```html
<form action="demo_form.asp" method="get" id="user_form">
First name:<input type="text" name="fname" />
<input type="submit" />
</form>
//这个不在form里面,但是form属性指向的form的id,可以把它和form标签绑定
Last name: <input type="text" name="lname" form="user_form" />
```

- 表单重写属性form overrides (formaction, formenctype, formmethod, formnovalidate, formtarget)

  - formaction - 重写表单的 action 属性
  - formenctype - 重写表单的 enctype 属性
  - formmethod - 重写表单的 method 属性
  - formnovalidate - 重写表单的 novalidate 属性
  - formtarget - 重写表单的 target 属性

  ```html
  <form action="/example/html5/demo_form.asp" method="get" id="user_form">
      E-mail: <input type="email" name="userid" /><br />
      <input type="submit" value="Submit" /><br />
      <input type="submit" formaction="/example/html5/demo_admin.asp" value="Submit as admin" /><br />
      <input type="submit" formnovalidate="true" value="Submit without validation" /><br />
  </form>
  ```

##### height 和 width

> height 和 width 属性只适用于 image 类型的 <input> 标签。

```html
<input type="image" src="img_submit.gif" width="99" height="99" />
```

##### list

> 通过list属性实现和datalist绑定

```html
<form action="/example/html5/demo_form.asp" method="get">
Webpage: <input type="url" list="url_list" name="link" />
<datalist id="url_list">
 <option label="W3School" value="http://www.w3school.com.cn" />
 <option label="Google" value="http://www.google.com" />
 <option label="Microsoft" value="http://www.microsoft.com" />
</datalist>
<input type="submit" />
</form>
```

##### multiple

> 可以选择多个文件

```html
 <input type="file" name="img" multiple="multiple" />
```

##### pattern (regexp)

> pattern 属性规定用于验证 input 域的模式（pattern）。

```html
 <input type="text" name="country_code"
pattern="[A-z]{3}" title="Three letter country code" />
```

##### placeholder

> 提示输入值

##### required

>required 属性规定必须在提交之前填写输入域（不能为空）。

```html
Name: <input type="text" name="usr_name" required="required" />
```

## 视频/DOM

### video支持的视频格式

> - Ogg = 带有 Theora 视频编码和 Vorbis 音频编码的 Ogg 文件
> - MPEG4 = 带有 H.264 视频编码和 AAC 音频编码的 MPEG 4 文件
> - WebM = 带有 VP8 视频编码和 Vorbis 音频编码的 WebM 文件

| 属性                                                         | 值       | 描述                                                         |
| :----------------------------------------------------------- | :------- | :----------------------------------------------------------- |
| [autoplay](https://www.w3school.com.cn/tags/att_video_autoplay.asp) | autoplay | 如果出现该属性，则视频在就绪后马上播放。                     |
| [controls](https://www.w3school.com.cn/tags/att_video_controls.asp) | controls | 如果出现该属性，则向用户显示控件，比如播放按钮。             |
| [height](https://www.w3school.com.cn/tags/att_video_height.asp) | *pixels* | 设置视频播放器的高度。                                       |
| [loop](https://www.w3school.com.cn/tags/att_video_loop.asp)  | loop     | 如果出现该属性，则当媒介文件完成播放后再次开始播放。         |
| [preload](https://www.w3school.com.cn/tags/att_video_preload.asp) | preload  | 如果出现该属性，则视频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。 |
| [src](https://www.w3school.com.cn/tags/att_video_src.asp)    | *url*    | 要播放的视频的 URL。                                         |
| [width](https://www.w3school.com.cn/tags/att_video_width.asp) | *pixels* | 设置视频播放器的宽度。                                       |

```html
<video width="320" height="240" controls="controls">
  <source src="movie.ogg" type="video/ogg">
  <source src="movie.mp4" type="video/mp4">
 Your browser does not support the video tag.
</video>
```

### Video + DOM

```html
<div style="text-align:center;">
  <button onclick="playPause()">播放/暂停</button> 
  <button onclick="makeBig()">大</button>
  <button onclick="makeNormal()">中</button>
  <button onclick="makeSmall()">小</button>
  <br /> 
  <video id="video1" width="420" style="margin-top:15px;">
    <source src="/example/html5/mov_bbb.mp4" type="video/mp4" />
    <source src="/example/html5/mov_bbb.ogg" type="video/ogg" />
    Your browser does not support HTML5 video.
  </video>
</div> 

<script type="text/javascript">
var myVideo=document.getElementById("video1");

function playPause(){ 
  if (myVideo.paused) 
    myVideo.play(); 
  else 
    myVideo.pause(); 
} 

function makeBig(){ 
 myVideo.width=560; 
} 

function makeSmall(){ 
 myVideo.width=320; 
} 

function makeNormal(){ 
 myVideo.width=420; 
} 
</script> 

```

#### HTML5 <video> - 方法、属性以及事件

下面列出了大多数浏览器支持的视频方法、属性和事件：

| 方法        | 属性        | 事件           |
| :---------- | :---------- | :------------- |
| play()      | currentSrc  | play           |
| pause()     | currentTime | pause          |
| load()      | videoWidth  | progress       |
| canPlayType | videoHeight | error          |
|             | duration    | timeupdate     |
|             | ended       | ended          |
|             | error       | abort          |
|             | paused      | empty          |
|             | muted       | emptied        |
|             | seeking     | waiting        |
|             | volume      | loadedmetadata |
|             | height      |                |
|             | width       |                |

**注释：**在所有属性中，只有 videoWidth 和 videoHeight 属性是立即可用的。在视频的元数据已加载后，其他属性才可用。

## 音频

```html
<audio controls="controls">
  <source src="song.ogg" type="audio/ogg">
  <source src="song.mp3" type="audio/mpeg">
Your browser does not support the audio tag.
</audio>
```

### <audio> 标签的属性

| 属性                                                         | 值       | 描述                                                         |
| :----------------------------------------------------------- | :------- | :----------------------------------------------------------- |
| [autoplay](https://www.w3school.com.cn/tags/att_audio_autoplay.asp) | autoplay | 如果出现该属性，则音频在就绪后马上播放。                     |
| [controls](https://www.w3school.com.cn/tags/att_audio_controls.asp) | controls | 如果出现该属性，则向用户显示控件，比如播放按钮。             |
| [loop](https://www.w3school.com.cn/tags/att_audio_loop.asp)  | loop     | 如果出现该属性，则每当音频结束时重新开始播放。               |
| [preload](https://www.w3school.com.cn/tags/att_audio_preload.asp) | preload  | 如果出现该属性，则音频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。 |
| [src](https://www.w3school.com.cn/tags/att_audio_src.asp)    | *url*    | 要播放的音频的 URL。                                         |

## 拖放

> 抓取对象然后拖到另一个位置

```html
<!DOCTYPE HTML>
<html>
  <head>
    <style type="text/css">
    #div1 {
      width:198px;  
      height:66px;
      padding:10px;
      border:1px solid #aaaaaa;
    }
    </style>
    <script type="text/javascript">
    function allowDrop(ev){
      //消除默认动作
      ev.preventDefault();
    }

    function drag(ev){
      //设置拖放元素的数据类型和值
      //类型为"Text"
      //元素的id("drag1")
      ev.dataTransfer.setData("Text",ev.target.id);
    }

    function drop(ev){
       ev.preventDefault();
       var data=ev.dataTransfer.getData("Text");         ev.target.appendChild(
         document.getElementById(data));
     }
  </script>
  </head>
<body>

<div 
     id="div1"    
     ondrop="drop(event)"<!--放置被拖数据时，会发生 drop 事件-->
     ondragover="allowDrop(event)"<!--规定拖放到的数据-->
     ></div>
<img 
     id="drag1" 
     src="img_logo.gif" 
     draggable="true" <!--是元素可以拖动-->
     ondragstart="drag(event)" 
     width="336" height="69"/>
</body>
</html>
```

## 画布

### 创建Canvas

```html
<canvas id="myCanvas" width="200" height="100">
 Your browser does not support the canvas elemen 
</canvas>
```

### 通过javaScript绘制

#### 绘制矩形

![](https://images.gitee.com/uploads/images/2020/0521/153506_a30b3390_6545143.png)

```html
<script type="text/javascript">
  var c=document.getElementById("myCanvas");
  var cxt=c.getContext("2d");
  cxt.fillStyle="#FF0000";//设置画笔的颜色
  cxt.fillRect(0,0,150,75);//绘制
</script>
```

#### 绘制线条

![](https://images.gitee.com/uploads/images/2020/0521/153518_417dc519_6545143.png)

```html
<script type="text/javascript">
  var c=document.getElementById("myCanvas");
  var cxt=c.getContext("2d");
  cxt.moveTo(10,10);
  cxt.lineTo(150,50);
  cxt.lineTo(10,50);
  cxt.stroke();
</script>
```

#### 圆形

![](https://images.gitee.com/uploads/images/2020/0521/153530_441f2afe_6545143.png)

```html
<script type="text/javascript">
  var c=document.getElementById("myCanvas");
  var cxt=c.getContext("2d");
  cxt.fillStyle="#FF0000";
  cxt.beginPath();
  cxt.arc(70,18,15,0,Math.PI*2,true);
  cxt.closePath();
  cxt.fill();
</script>
```

#### 渐变

![](https://images.gitee.com/uploads/images/2020/0521/153702_738cd56b_6545143.png)

```html
<script type="text/javascript">
  var c=document.getElementById("myCanvas");
  var cxt=c.getContext("2d");
  var grd=cxt.createLinearGradient(0,0,175,50);
  grd.addColorStop(0,"#FF0000");
  grd.addColorStop(1,"#00FF00");
  cxt.fillStyle=grd;
  cxt.fillRect(0,0,175,50);
</script>
```

#### 图像

![](https://images.gitee.com/uploads/images/2020/0521/153826_63883e4b_6545143.png)

## SVG

#### 简介

> - SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
> - SVG 用于定义用于网络的基于矢量的图形
> - SVG 使用 XML 格式定义图形
> - SVG 图像在放大或改变尺寸的情况下其图形质量不会有损失
> - SVG 是万维网联盟的标准

#### 优点

> 与其他图像格式相比（比如 JPEG 和 GIF），使用 SVG 的优势在于：
>
> - SVG 图像可通过文本编辑器来创建和修改
> - SVG 图像可被搜索、索引、脚本化或压缩
> - SVG 是可伸缩的
> - SVG 图像可在任何的分辨率下被高质量地打印
> - SVG 可在图像质量不下降的情况下被放大

```html
<svg xmlns="http://www.w3.org/2000/svg" version="1.1" height="190">
  <polygon points="100,10 40,180 190,60 10,60 160,180"
  style="fill:lime;stroke:purple;stroke-width:5;fill-rule:evenodd;" />
</svg>
```

![](https://images.gitee.com/uploads/images/2020/0521/160322_cf31cb9f_6545143.png)

## 画布vsSVG

#### SVG

> SVG 是一种使用 XML 描述 2D 图形的语言。
>
> SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。
>
> 在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。

> - 不依赖分辨率
> - 支持事件处理器
> - 最适合带有大型渲染区域的应用程序（比如谷歌地图）
> - 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快）
> - 不适合游戏应用

#### Canvas

> Canvas 通过 JavaScript 来绘制 2D 图形。
>
> Canvas 是逐像素进行渲染的。
>
> 在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。

> - 依赖分辨率
> - 不支持事件处理器
> - 弱的文本渲染能力
> - 能够以 .png 或 .jpg 格式保存结果图像
> - 最适合图像密集型的游戏，其中的许多对象会被频繁重绘

## Web存储

> 客户端存储数据的新方法：
>
> - localStorage - 没有时间限制的数据存储
> - sessionStorage - 针对一个 session 的数据存储

#### localStorage

> localStorage 方法存储的数据==没有时间限制==。第二天、第二周或下一年之后，数据依然可用。

```html
<script type="text/javascript">
  localStorage.lastname="Smith";
  document.write(localStorage.lastname);
</script>
```

#### sessionStorage

> sessionStorage 方法针对一个 session 进行数据存储。当用户==关闭浏览器窗口后，数据会被删除==。

```html
<script type="text/javascript">
  sessionStorage.lastname="Smith";
  document.write(sessionStorage.lastname);
</script>
```

## 应用缓存(Application Cache)

> - 离线浏览 - 用户可在应用离线时使用它们
> - 速度 - 已缓存资源加载得更快
> - 减少服务器负载 - 浏览器将只从服务器下载更新过或更改过的资源

```html
<!DOCTYPE HTML>
<html manifest="demo.appcache">

<body>
The content of the document......
</body>

</html>
```

> manifest 文件需要配置*正确的 MIME-type*，即 "text/cache-manifest"。必须在 web 服务器上进行配置

#### Manifest文件

> manifest 文件是简单的文本文件，它告知浏览器被缓存的内容（以及不缓存的内容）
>
> manifest 文件可分为三个部分：
>
> - *CACHE MANIFEST* - 在此标题下列出的文件将在首次下载后进行缓存
> - *NETWORK* - 在此标题下列出的文件需要与服务器的连接，且不会被缓存
> - *FALLBACK* - 在此标题下列出的文件规定当页面无法访问时的回退页面（比如 404 页面）

##### CACHE MANIFEST(必需)

> 文件会被缓存

```txt
CACHE MANIFEST
/theme.css
/logo.gif
/main.js
```

##### *NETWORK*

> 文件永远不会被缓存

```txt
NETWORK:
login.asp

或
NETWORK:
*
#代表所有资源文件
```

##### *FALLBACK*

> 规定如果无法建立因特网连接，则用 "404.html" 替代 /html5/ 目录中的所有文件：

```txt
FALLBACK:
/html5/ /404.html
```

##### 更新缓存

一旦应用被缓存，它就会保持缓存直到发生下列情况：

- 用户清空浏览器缓存
- manifest 文件被修改（参阅下面的提示）
- 由程序来更新应用缓存

##### Manifest 文件

```txt
CACHE MANIFEST
# 2012-02-21 v1.0.0
/theme.css
/logo.gif
/main.js

NETWORK:
login.asp

FALLBACK:
/html5/ /404.html
```

> **重要的提示：**以 "#" 开头的是注释行，但也可满足其他用途。应用的缓存会在其 manifest 文件更改时被更新。如果您编辑了一幅图片，或者修改了一个 JavaScript 函数，这些改变都不会被重新缓存。==更新注释行中的日期和版本号是一种使浏览器重新缓存文件的办法==。

##### 关于应用程序缓存的注释

请留心缓存的内容。

一旦文件被缓存，则浏览器会继续展示已缓存的版本，即使您修改了服务器上的文件。为了确保浏览器更新缓存，您需要更新 manifest 文件。

**注释：**浏览器对缓存数据的容量限制可能不太一样（某些浏览器设置的限制是每个站点 5MB）。

## Web Workers

#### 简介

>当在 HTML 页面中执行脚本时，页面的状态是不可响应的，直到脚本已完成。

> web worker 是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。您可以继续做任何愿意做的事情：点击、选取内容等等，而此时 web worker 在后台运行。

demo_worker.js

```js
var i=0;
function timedCount(){
  i=i+1;
  postMessage(i);
  setTimeout("timedCount()",500);
}

timedCount();
```

```html
<!DOCTYPE html>
<html>
<body>
<p>Count numbers: <output id="result"></output></p>
<button onclick="startWorker()">Start Worker</button>
<button onclick="stopWorker()">Stop Worker</button>
<br /><br />

<script>
var w;
function startWorker(){
  <!-- 检测是否支持Worker-->
if(typeof(Worker)!=="undefined"){
  if(typeof(w)=="undefined"){
    w=new Worker("demo_workers.js");
    }
  w.onmessage = function (event) {
  document.getElementById("result").innerHTML=event.data;
  };
}
else{
document.getElementById("result").innerHTML="Sorry, your browser
 does not support Web Workers...";
}
}

function stopWorker(){
w.terminate();
}
</script>
</body>
</html>
```

## 服务器发送事件(**server-sent event**)

> Server-Sent 事件指的是网页自动获取来自服务器的更新

## 检测 Server-Sent 事件支持

```js
if(typeof(EventSource)!=="undefined"){
  // Yes! Server-sent events support!
  // Some code.....
  }else{
  // Sorry! No server-sent events support..
  }
```

### 接收Server-Sent事件通知

```js
var source=new EventSource("demo_sse.php");
source.onmessage=function(event){
  document.getElementById("result").innerHTML+=event.data + "<br />";
  };
```

> - 创建一个新的 EventSource 对象，然后规定发送更新的页面的 URL（本例中是 "demo_sse.php"）
> - 每接收到一次更新，就会发生 onmessage 事件
> - 当 onmessage 事件发生时，把已接收的数据推入 id 为 "result" 的元素中

### EventSource 对象

在上面的例子中，我们使用 onmessage 事件来获取消息。不过还可以使用其他事件：

| 事件      | 描述                     |
| :-------- | :----------------------- |
| onopen    | 当通往服务器的连接被打开 |
| onmessage | 当接收到消息             |
| onerror   | 当错误发生               |
