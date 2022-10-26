# html

***

## 基础标签

### a标签中href和tips

```html
<a href="http://www.one.com" target="_blank">

在HTML文档中插入ID:
<a id="tips">有用的提示部分</a>

在HTML文档中创建一个链接到"有用的提示部分(id="tips"）"：
<a href="#tips">访问有用的提示部分</a>

或者，从另一个页面创建一个链接到"有用的提示部分(id="tips"）"：
<a href="http://www.one.com/html/html-links.html#tips">访问有用的提示部分</a>
```

### table

#### 第一种

```html
<table border="1"> 
    <caption>标题</caption>
    <colgroup>
        <!--定义前两列样式-->
        <col span="2" style="background-color:red">
        <!--定义第三列的颜色-->
        <col style="background-color:yellow">
    </colgroup>
    <th>
        <td>表头</td> 
        <td>表头</td>
        <td>表头</td> 
    </th>
    <tr> 
        <td>row 1, cell 1</td> 
        <td>row 1, cell 2</td> 
        <td>row 1, cell 3</td> 
    </tr> 
    <tr> 
        <td>row 2, cell 1</td> 
        <td>row 2, cell 2</td> 
        <td>row 2, cell 3</td> 
    </tr> 
</table>
```

#### 第二种

```html
<table border="1">
    <!--表头(页眉)-->
    <thead>
        <tr>
          <th>Month</th>
          <th>Savings</th>
        </tr>
    </thead>
    <!--页脚-->
    <tfoot>
        <tr>
          <td>Sum</td>
          <td>$180</td>
        </tr>
    </tfoot>
    <!--主体-->
    <tbody>
        <tr>
          <td>January</td>
          <td>$100</td>
        </tr>
        <tr>
          <td>February</td>
          <td>$80</td>
        </tr>
    </tbody>
</table>
```

### 表单form

```html
<form action="demo_form.php" method="get">
<!--定义围绕表单的边框-->
<fieldset>

    <!--围绕边框的标题-->
    <legend>Personalia:</legend>
    
    First name: <input type="text" name="fname"><br>
    <label for="lastName">Last name:</label> <input type="text" name="lname"><br>
    
    <!--文本框-->
    <textarea name="textContext" rows="10" cols="30">
        我是一个文本框。
    </textarea>
    
    <!--选择列表-->
    <select>
      <optgroup label="Swedish Cars">
        <option value="volvo">Volvo</option>
        <option value="saab">Saab</option>
      </optgroup>
      <optgroup label="German Cars">
        <option value="mercedes">Mercedes</option>
        <option value="audi">Audi</option>
      </optgroup>
    </select>
    
    <!--定义选择列表-->
    <input list="browsers" name="browser">
    <datalist id="browsers">
      <option value="Internet Explorer">
      <option value="Firefox">
      <option value="Chrome">
      <option value="Opera">
      <option value="Safari">
    </datalist>
    <input type="submit" value="提交">
</fieldset>
</form>
```

### 框架

```html
<iframe src="//www.baidu.com">
  <p>您的浏览器不支持  iframe 标签。</p>
</iframe>
```

### 图像

#### img

```html
<!--图像-->
<img src="https://images.gitee.com/uploads/images/2020/0310/232238_8efc4b0b_6545143.jpeg" alt="Smiley face" width="42" height="42">
```

#### map(图像映射)

```html
<!--图像映射-->
<img src="planets.gif" width="145" height="126" alt="Planets" usemap="#planetmap">
<!--定义三片区域,点击这三个区域可以实现跳转-->
<map name="planetmap">
  <area shape="rect" coords="0,0,82,126" alt="Sun" href="sun.htm">
  <area shape="circle" coords="90,58,3" alt="Mercury" href="mercur.htm">
  <area shape="circle" coords="124,58,8" alt="Venus" href="venus.htm">
</map>
```

#### canvas(通过脚本绘制图像)

```html
<canvas id="myCanvas">你的浏览器不支持 HTML5 canvas 标签。</canvas>

<script>
var c=document.getElementById('myCanvas');
var ctx=c.getContext('2d');
ctx.fillStyle='#FF0000';
ctx.fillRect(0,0,80,100);
</script>
```

#### figure(通过标签与元素组合)

```html
<figure>
  <img src="img_pulpit.jpg" alt="The Pulpit Rock" width="304" height="228">
  <!--定义img的标题,这个是在下-->
  <figcaption>Fig1. - A view of the pulpit rock in Norway.</figcaption>
</figure>
```

### audio/video

#### audio

```html
<audio controls>
    <!--浏览器支持则任意一个-->
    <source src="horse.ogg" type="audio/ogg">
    <source src="horse.mp3" type="audio/mpeg">
  您的浏览器不支持 audio 元素。
</audio>
```

#### track(字幕)

```html
<video width="320" height="240" controls>
    <source src="forrest_gump.mp4" type="video/mp4">
    <source src="forrest_gump.ogg" type="video/ogg">
    <!--定义字幕-->
    <track src="subtitles_en.vtt" kind="subtitles" srclang="en"
    label="English">
    <track src="subtitles_no.vtt" kind="subtitles" srclang="no"
    label="Norwegian">
</video>

```

#### video

```html
<video width="320" height="240" controls>
    <source src="movie.mp4" type="video/mp4">
    <source src="movie.ogg" type="video/ogg">
    您的浏览器不支持 video 标签。
</video>
```

### 链接

```html
<a href="url">访问菜鸟教程!</a>

<head>
    <link rel="stylesheet" type="text/css" href="theme.css">
</head>

<nav>
    <a href="/html/">HTML</a> |
    <a href="/css/">CSS</a> |
    <a href="/js/">JavaScript</a> |
    <a href="/jquery/">jQuery</a>
</nav>
```

### 列表

#### 无序列表

```html
<ul>
    <li>Coffee</li>
    <li>Tea</li>
    <li>Milk</li>
</ul>
```

#### 有序列表

```html
<ol start="1">
  <li>Coffee</li>
  <li>Tea</li>
  <li>Milk</li>
</ol>
```

#### 定义列表

```html
<dl>
  <dt>Coffee</dt>
  <!--描述-->
  <dd>Black hot drink</dd>
  <dt>Milk</dt>
  <dd>White cold drink</dd>
</dl>
```

### 文档(article)

```html
<article>
    <header>
        文档头部
    </header>
    <section>
        文档的某个区域
    </section>
    <aside>
        所处内容之外的内容
    </aside>
    <footer>
        文档底部
    </footer>
</article>
```

### 元信息

- `<head>`:定义文档的信息
- `<meta>`:关于HTML文档的原信息

```html
<meta name="author" content="runoob">
<meta charset="UTF-8">
```

- `<base>`:订阅页面所有链接的默认地址或默认目标

```html
<head>
    <base href="url" target="_blank">
</head>
```

### 嵌入flash动画片

`<embed src="helloworld.swf">`

## base标签

```html
<head>
    <base href="http://example.com/here/is/my/deeply/nested/set/of/images/">
   <base target="_blank" />//还可以定义这个a标签的超链接方式
</head>

<img src="/here/is/my/deeply/nested/set/of/images/example-01.jpg" alt="">
<img src="/here/is/my/deeply/nested/set/of/images/example-01.jpg" alt="">
<img src="/here/is/my/deeply/nested/set/of/images/example-03.jpg" alt="">
//就可以改成下面
<img src="example-01.jpg" alt="">
<img src="example-01.jpg" alt="">
<img src="example-03.jpg" alt="">

```
