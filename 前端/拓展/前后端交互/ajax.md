# ajax

> - 全称`Asynchronous JavaScript and XML`
> - 主要实现`获取促进 -> 处理数据 ->展示数据`

```js
function ajaxRequest () {
  //实例化一个XMLHttpRequest对象
  var xhr = new XMLHttpRequest();
  //open()规定请求的类型、url、是否异步
  xhr.open("GET","https://www.w3cschool.cn/statics/demosource/ajax_info.txt", true);
  xhr.send();
  //send()发送请求，必须结合open一起使用
  xhr.onreadystatechange = function(){
    //监测服务器响应的状态
    if (xhr.readyState === 4 && xhr.status === 200) {
      //当 readyState 等于 4 且状态为 200 时，表示成功响应
      document.getElementById("view").innerHTML = xhr.responseText;
    }                
  }
}
```

## ajax和flash的优缺点

```
Ajax
  Ajax的优势：1.可搜索性 2.开放性 3.费用 4.易用性 5.易于开发。
  Ajax的劣势：
    1.它可能破坏浏览器的后退功能   
    2.使用动态页面更新使得用户难于将某个特定的状态保存到收藏夹中 ，不过这些都有相关方法解决。

Flash
  Flash的优势：1.多媒体处理 2.兼容性 3.矢量图形 4.客户端资源调度
  Flash的劣势：
   1.二进制格式 
   2.格式私有 
   3.flash 文件经常会很大，用户第一次使用的时候需要忍耐较长的等待时间  
   4.性能问题
```

## 优缺点

 优点：

 1. 页面无刷新更新，用户的体验非常好；
 2. 异步通信，响应更快
 3. 可以将一些服务器工作转移到客户端，利用客户端资源来处理，减轻服务器和带宽的压力，节约空间和带宽租用成本；
 4. 技术标准化，并被`浏览器广泛支持`，不需要下载插件或者小程序；
 5. Ajax 可使因特网应用程序更小、更快、更友好。

 缺点：

 1. Ajax 不支持浏览器 back 返回按钮；
 2. 有安全问题，Ajax 暴露了与服务器交互的细节；
 3. 对搜索引擎不友好；
 4. 破坏了程序的异常机制；
 5. 不容易调试。

## 同源策略

> **一个 URL 地址可以有以下几个组成部分：**`scheme`**: //**`host`**:**`post`**/**`path`**?**`query`**#**`fragment`
>
> - **scheme**：通信协议，一般为 http 、https；
> - **host**：域名；
> - **post**：端口号，此项为可选项，http 协议默认的端口号为 80，https 协议默认的端口号为 443；
> - **path**：路径，由 "/ "隔开的字符串；
> - **query**：查询参数，此项为可选项；
> - **fragment**：信息片段，用于指定网络资源中的某片断，此项为可选项；

### 同源

> - URL地址动的协议, 域名, 端口都相同, 不相同就会产生跨域
> - 只能和同源的后端接口进行数据交互

### 跨域处理

> - JSONP技术
> - 服务端代理
> - 通过修改window.name 实现跨域
> - 使用HTML5中新引进的window.postMessage 方法来跨域传送数据

## XMLHttpRequest

> - Ajax技术 核心就是 `XMLHttpRequest`类, 简称XHR, 允许脚本 `异步调用` HTTP API.
> - 浏览器在`XMLHttpRequest` 类上定义了HTTP API , 这个类每个实例都是独立 的请求/ 响应 对象, 可指定对象上和方法允许 指定细节 和提取响应数据

### 创建实例

```js
var xhr = new XMLHttpRequest();
// 兼容 IE7之前并不支持前者
var xhr = new ActiveXObject("Microsoft.XMLHTTP");
// 兼容性写法
var xhr = window.XMLHttpRequest ? new window.XMLHttpRequest() : new ActiveXObject("Microsoft.XMLHTTP");
```

## open()

创建 XMLHttpRequest 对象之后，发起 HTTP 请求的`下一步`是调用 XMLHttpRequest 对象的`open`方法，指定 HTTP 请求的两个必需部分：**请求方法** 和 **URL**

**xhr.open(method, url, async)**

- **method：**
  - 第一个参数用于指定 HTTP 请求的方法，不区分大小写；
  - 该参数可取的值包括："GET"、"POST"、"HEAD"、"PUT"、"OPTIONS"、"DELETE"，其中，"GET" 和 "POST" 是得到广泛支持的请求方法；
- **url：**
  - 第二个参数用于指定 HTTP 请求的 URL 地址，可以是 **绝对URL** 或 **相对URL**；
  - 绝对URL：需要满足 "同源策略"（服务器明确允许跨域请求的情况除外）；
  - 相对URL：即相对于文档的 URL；
- **async：**
  - 第三个参数是可选的，可用`布尔值`指定脚本是否以异步的方式调用此次 Ajax 请求；
  - 该参数默认为 true，表示异步调用此次 Ajax 请求，不阻塞后续脚本的执行；

**注意**：

`open()`方法其实还可以有第四、第五个参数，分别是用于 HTTP 请求访问认证的`用户名和密码`，使用它们需要在服务器做相应的配置，较为少用。

## setRequestHeader

如果你的 HTTP 请求需要`设置请求头`，那么调用 open 方法之后的`下个步骤`就是设置它，使用的方法是：`setRequestHeader`

```js
// 在 open 方法之后设置请求头
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
```

**xhr.setRequestHeader(name, value)**

- **name**:请求头名称；
- **value**:请求头的值。

## send()

使用 XMLHttpRequest `发起 HTTP 请求`的`最后一步`是指定可选的请求主体、并向服务器发送它，使用的方法是：`send`

```js
var xhr = new XMLHttpRequest();
xhr.open("GET", "/statics/demosource/demo_get_json.php");

// 由于GET请求,没有请求主体，所以在调用 send 方法时可以传递 null或省略这个参数；
xhr.send(null);
var xhr = new XMLHttpRequest();
xhr.open("POST", "/statics/demosource/demo_post_json.php");

// 把 msg 作为请求主体发送
xhr.send(msg);
```

1. POST 请求通常都拥有请求主体，可在 send 方法中指定它；
2. POST 请求的请求主体，`应该匹配` `setRequestHeader`方法所指定的 "Content-Type" 头。

## 获取响应

> - 一个完整的 HTTP 响应由 `状态码、响应头和 响应主体` 组成，这三者都可以通过`XMLHttpRequest`对象提供的属性和方法获取。
>
> - 为了能够在 HTTP 响应准备就绪时得到通知，必须**监听**`XMLHttpRequest`对象上的`readystatechange`事件。但为了理解这个事件类型，需要先了解下`readyState`属性，因为该事件监听的是`readyState`属性值的改变。
> - XMLHttpRequest`对象上的`readyState`属性在 HTTP 请求过程中，会`从 0 变到 4

```html

<script>
  var oBtn = document.getElementById("btn");
  oBtn.onclick = function () {
  //兼容处理
  var xhr = window.XMLHttpRequest ? new window.XMLHttpRequest() : new ActiveXObject("Microsoft.XMLHTTP");
  alert(xhr.readyState);//0
  xhr.onreadystatechange = function () {
  alert(xhr.readyState);
  }
  xhr.open("GET", "/statics/demosource/demo_get_json.php");
  xhr.send();
  }
</script>

```

### readyState 属性

 `readyState`属性是一个整数，它的值代表了不同的 HTTP 请求状态。

- 0：初始值，表示请求未初始化，`open`方法尚未调用；
- 1：启动请求，open 方法已经调用，但尚未调用 send 方法；
- 2：请求发送，已经调用 send 方法，但尚未接收到响应；
- 3：接收响应，已经接受到`部分响应`数据，主要是响应头；
- 4：HTTP 响应完成，已经接收到全部响应数据，而且可以在客户端使用。

 每次`readyState`属性值的改变都会触发`readystatechange`事件，**但只有`readyState`属性值为 4 时才是我们所关心的状态**，因为只有这个状态才表示 HTTP 的响应准备就绪，可以真正意义上的结合服务器所响应的数据来实现我们的业务需求。

### 发送请求规范

```html
<body>
    <button id="btn">点我观察 readyState 属性的改变</button>
    <div id="tip"></div>
    
    <script>
        var oBtn = document.getElementById("btn"),
            oTip = document.getElementById("tip");
        oBtn.onclick = function () {
            var xhr = window.XMLHttpRequest ? new window.XMLHttpRequest() : new ActiveXObject("Microsoft.XMLHTTP");
            xhr.onreadystatechange = function () {
                if (xhr.readyState === 4) {
                    oTip.innerText = "HTTP 响应完成";
                }
            };

            xhr.open("GET", "/statics/demosource/demo_get_json.php");
            xhr.send();
        }
    </script>
</body>
```

**注意**：

- `readyState`的属性值只代表此时的 HTTP 请求处于哪个阶段：是发送了请求还是未发送请求，是只接收到了响应头还是响应完成；
- "响应完成" 只代表 HTTP 请求结束，至于服务器的响应状态：是请求成功还是请求错误，又或者是服务器错误，需要通过 `HTTP 状态码`判断，它存储在`XMLhttpRequest`的`status`属性上；

### status属性

`status`属性会以数字的形式保存服务器响应的 HTTP 状态码，诸如使用最频繁的 "200" 表示请求成功，"404" 表示 URL 不能匹配服务器上的任何资源。

`HTTP 状态码`是用来表示网页服务器响应状态的 `3 位`数字代码，所有状态码的第一个数字代表了响应的五种状态之一：

- 1xx：临时响应
- 2xx：成功
- 3xx：重定向
- 4xx：请求错误
- 5xx：服务器错误

哪些 HTTP 状态码表示我们可以获取到 HTTP 响应数据呢？

**2开头的状态码** 与 **304**。2开头的状态码都表示请求成功，而 304 是对客户端可读取缓存的一种响应，同样能获取到 HTTP 的响应数据。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Tryrun 4</title>
    <style>
        #btn { margin-top: 7px; }
    </style>
</head>
<body>
    <div id="tip"></div>
    <button id="btn">点我发起 Ajax 请求</button>
    
    <script>
        var oBtn = document.getElementById("btn"),
            oTip = document.getElementById("tip");

        oBtn.onclick = function () {
            var xhr = window.XMLHttpRequest ? new window.XMLHttpRequest() : new ActiveXObject("Microsoft.XMLHTTP");

            xhr.onreadystatechange = function () {
                if (xhr.readyState === 4) return;
                if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
                    oTip.innerText = "HTTP 请求成功";
                }
            };

            xhr.open("GET", "/statics/demosource/demo_get_json.php");
            xhr.send();
        }
    </script>
</body>
</html>
```

### responseText 属性

`responseText`属性以字符串的形式存储了响应主体，即：服务器的响应数据。

无论返回的数据类型是什么，响应主体的内容都会保存在`responseText`属性中；

#### 响应html

```js
var xhr = new XMLHttpRequest();

xhr.onreadystatechange = function () {
    if (xhr.readyState !== 4) return;
    if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
        // 当响应成功，获取响应数据,将数据赋值给本地
        oView.innerHTML = xhr.responseText;
    }
};

xhr.open("GET", "/statics/demosource/demo_get.php");
xhr.send();
```

响应json

```js
var xhr = new XMLHttpRequest();

xhr.onreadystatechange = function () {
    if (xhr.readyState !== 4) return;
    if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
        // 使用JSON.parse把 响应数据转换为json数据
        var res = JSON.parse(xhr.responseText);
        // 将响应数据中的data属性赋值给oTime做内容
        oTime.innerText = res.data;
    }
};

xhr.open("GET", "/statics/demosource/demo_get_json.php");
xhr.send();
```

### 查询 HTTP 响应头的方法

在`XMLHttpRequest`对象上，可通过`getAllResponseHeaders`和`getResponseHeader`方法查询响应头信息。

#### getAllResponseHeaders

- `getAllResponseHeaders`方法无参数，用于一次性返回可查询的全部响应头信息

```js
var xhr = new XMLHttpRequest();

xhr.onreadystatechange = function () {
    if (xhr.readyState !== 4) return;
    if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
        // 获取所有可查询的响应头信息
        oView.innerText = xhr.getAllResponseHeaders();
    }
};

xhr.open("GET", "/statics/demosource/demo_get_json.php");
xhr.send();
```

#### getResponseHeader

- `getResponseHeader`方法用于查询`单一`响应头信息，需要传入一个指定 "头名称" 的字符串作为参数：`getResponseHeader(headerName)`

```js
var xhr = new XMLHttpRequest();

xhr.onreadystatechange = function () {
    if (xhr.readyState !== 4) return;
    if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
        // 查询 "Content-Type" 响应头信息
        alert( xhr.getResponseHeader("Content-Type") );
    }
};

xhr.open("GET", "/statics/demosource/demo_get_json.php");
xhr.send();
```

**注意**：

由于`XMLHttpRequest`会自动处理 cookie，将 cookie 从`getAllResponseHeaders`方法返回的响应头集合中过滤掉，并且如果给`getResponseHeader`方法传递 "Set-Cookie" 或 "Set-Cookie2"，则返回 null。

### 同步响应

```js
var xhr = new XMLHttpRequest();

// 指定 open 方法的第三个参数为 false
xhr.open("GET", "/statics/demosource/demo_get_json.php", false);

// send 方法的调用将阻塞后面代码的执行，直到此次 HTTP 请求完成
xhr.send();

// 不再需要监听 readystatechange 事件
if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
    oTime.innerText = JSON.parse(xhr.response).date;
} else {
    // 如果请求不成功，就报错
    throw new Error(xhr.status);
}
```

### abort 中止请求

若 HTTP 请求的时间超出预期，可以调用`XMLHttpRequest`对象上的`abort`方法来中止 HTTP 请求。

```js
var xhr = new XMLHttpRequest();
var timer = null;    // 用于存储定时器标识

xhr.onreadystatechange = function () {
    if (xhr.readyState !== 4) return;
    if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
        clearTimeout(timer);    // 未超时则取消定时器
    }
};

xhr.open("GET", "/statics/demosource/demo_get_json.php");
xhr.send();

// 2秒后中止此次 GET 请求
timer = setTimeout(function(){
    xhr.abort();
}, 2000)
```

## GET请求

GET 请求一般用于信息`获取`，它`没有请求主体`，而是使用 URL 传递参数（即：传递数据给后台）。

传递参数的方式：

1. 对所需发送的数据（具有名称和值）执行普通的 URL 编码，即：由一对对 "名称=值" 组成（称为：名/值对），每一对之间用 "&" 拼接，如 "name=value&name=value&...&name=value"；
2. 由于 名/值对 会附加在 URL 地址后面，因此在这串字符参数的最前面需要添加个 "?"，表示 URL 的 查询参数 开始。

```html

<body>
    <div id="form">
        <label for="country">国家：<input type="text" name="country" id="country"></label>
        <label for="city">城市：<input type="text" name="city" id="city"></label>
    </div>
    <hr>
    <div>你查询的国家是：<span id="ipt_country"></span></div>
    <div>你查询的城市是：<span id="ipt_city"></span></div>
    <br>
    <button type="button" id="search">查询</button>
    （查询成功后会把你输入的值显示在上方）

    <script>
        var oSearch = document.getElementById("search"),
            oIpt_country = document.getElementById("ipt_country"),
            oIpt_city = document.getElementById("ipt_city");

        var url = "/statics/demosource/demo_get_json.php";

        oSearch.onclick = function () {
            var country = document.getElementById("country").value,
                city = document.getElementById("city").value;

            var query = "country=" + country + "&city=" + city;

            var queryURL = url + "?" + query;

            // 发起 get 请求
            ajaxGet(queryURL);
        }

        function ajaxGet (url) {
            var xhr = window.XMLHttpRequest ? new window.XMLHttpRequest() : new ActiveXObject("Microsoft.XMLHTTP");
            xhr.onreadystatechange = function () {
                if (xhr.readyState === 4) {
                    if (xhr.status >= 200 && xhr.status < 300 || xhr.status === 304) {
                        var res = JSON.parse(xhr.responseText);
                        oIpt_country.innerText = res.params.country;
                        oIpt_city.innerText = res.params.city;
                    }
                }
            }
            
            xhr.open("GET", url);
            xhr.send();
        }
    </script>
</body>
```

### 缓存问题

对于 GET 请求，请求的结果会被浏览器缓存，特别是在 IE 浏览器下。这时，如果 GET 请求的 URL 不变，那么请求的结果就是浏览器的缓存（也就是上次 GET 请求的结果）。

**解决办法**

实时改变 GET 请求的 URL，只要 URL 不同，就不会取到浏览器的缓存结果。

在 URL 末尾`添加时间戳参数`。由于时间戳可以精确到毫秒，从而保证了每次发起 GET 请求的时间不同，达到实时改变请求 URL 的目的。

```js
var url = "/statics/demosource/demo_get_json.php";

// 在请求参数的最后附加时间戳参数 t
var query = "user=" + user + "&pwd=" + pwd + "&t=" + new Date().getTime();

var queryURL = url + "?" + query;
// ajax_get为自己封装的请求对象，不是固定用法
ajax_get(queryURL);
```

### 封装 GET 异步请求函数

步骤：

- 实例化一个`XMLHttpRequest`对象，如果你正在使用 IE7 以下版本的浏览器，应该对它做兼容处理；
- 对`data`数据执行普通的 URL 编码，也可以使用预置代码中提供的`urlencodeData`工具函数完成这步操作；
- 调用`open()`方法，指定请求方式、请求地址、是否异步，注意请求地址需要是`url`与请求参数拼接的结果；
- 调用`send()`方法；
- 给`XMLHttpRequest`实例添加`readystatechange`事件处理程序：
  - 最好在`open()`方法调用之前实现`readystatechange`事件，比较严谨；
  - 在事件处理程序中判断 HTTP 请求状态，只有请求状态为 "完成" 时，才能保证响应内容完整；
- 根据 HTTP 状态码（即`status`属性值），执行对应的回调函数：
- HTTP 状态码在 200 与 300 之间（不包括300）、或为 304 都表示成功：使用`JSON.parse()`方法将`responseText`属性值解析为 JavaScript 对象，并作为`success`函数的实参传出；

- HTTP 状态码为其它值则表示请求失败：调用`error`函数，并将失败的 HTTP 状态码作为实参传出；

- 在运行结果区封装的 `ajaxPost`,能否正常响应，常见的错误有：
  - 成功请求的测试出现 403：GET 请求的 URL 地址不正确，可以检查一下是否在`url`与 请求参数之间拼接了 "?"。

```html

    <script>
        function ajaxGet (url, data, success, error) {
            // 在下方开始你的代码
            var xhr = window.XMLHttpRequest ? new     window.XMLHttpRequest():new ActiveXObject('Microsoft.XMLHTTP');
            data = urlencodeData (data)
            console.log(data);
            xhr.onreadystatechange = function(){
                if(xhr.readyState !== 4) return;
                if(xhr.status >= 200 && xhr.status < 300 || xhr.status === 304){
                    console.log(xhr.status);
                    var res = JSON.parse(xhr.responseText)
                        success(res);
                }else{
                    var res = JSON.parse(xhr.responseText)
                    console.log(res);
                    error(res);
                }
               
                    
            };
            url = url + '?' + data;
            xhr.open('GET',url,true);
            xhr.send(null);
            }
         
    </script>
    
    
    <!-- 工具函数 -->
    <script>
        // 用于对 JavaScript 对象执行普通的 URL 编码
        // 编码后的格式为："名称=值&...&名称=值"
        function urlencodeData (data) {
            if (!data) return;
            var pairs = [];
            for (var name in data) {
                if (!data.hasOwnProperty(name)) continue;
                if (typeof data[name] === "function") continue;
                var value = (data[name] === null || data[name] === undefined) ? "" : data[name].toString();
                pairs.push(encodeURIComponent(name) + "=" + encodeURIComponent(value));
            }
            return pairs.join("&");
        }
    </script>
    
    <!-- 以下均是测试代码 -->
    <!-- 测试代码的 css 部分 -->
    <style>
        #request { text-align: center; }
        .request-result { padding: 12px; border: 1px solid #e8e8e8;    border-radius: 2px; box-shadow: 0 1px 3px 1px #d9dfe9; }
        .request-btn { margin-top: 12px; padding: 7px; color: #fff;    border-radius: 7px; transition: all .2s; cursor: pointer; }
        .request-success { background-color: #1890ff; }
        .request-success:hover { background-color: #40a9ff; }
        .request-error { background-color: #d9363e; }
        .request-error:hover { background-color: #ff7875; }
    </style>
</head>
<body>
    <!-- 测试代码的 html 部分 -->
    <div id="request">
        <div class="request-result">
            <div class="res-tip">测试下你封装的异步 GET 请求能不能正常响</div>
            <div class="res-param"></div>
        </div>
        <div class="request-btn request-success">成功请求的测试</div>
        <div class="request-btn request-error">失败请求的测试</div>
    </div>
    
    <!-- 测试代码的 js 部分 -->
    <script>
        var oDivs = document.getElementsByTagName("div");
        var oResult_tip = oDivs[2],
            oResult_param = oDivs[3],
            oSuccess = oDivs[4],
            oError = oDivs[5];
            
        var url = "/statics/demosource/demo_get_json.php",
            badUrl = "/statics/demosource/404.txt";
        var data = {
                aa:null,
                from: "南昌",
                to: "厦门",
                time: "今天"
            };
        var success = function (res) {
            oResult_tip.innerText = "请求成功";
            oResult_param.innerHTML = "<div><span>起点：</span><span>" + res.params.from + "</span></div><div><span>终点：</span><span>" + res.params.to + "</span></div><div><span>时间：</span><span>" + res.params.time + "</span></div>";
        };
        var error = function (res) {
            oResult_tip.innerText = "请求失败：" + res;
            oResult_param.innerHTML = "";
        };
        
        oSuccess.onclick = function () {
            ajaxGet(url, data, success, error);
        };
        
        oError.onclick = function () {
            ajaxGet(badUrl, data, success, error);
        }
    </script>
</body>
```

## POST请求

POST 请求一般用于修改服务器上的资源，它需要发送一个请求主体，客户端传递给服务器的数据就包含在这个请求主体中。

`"Content-Type"`请求头用于设置请求主体的编码格式。

### 表单编码的 POST 请求

POST 请求使用 **表单编码** 的方式来发送数据的关键步骤：

1. 对所需发送的数据（具有名称和值）执行普通的 URL 编码，即：像 GET 请求那样拼接为 名/值 对的形式；
2. 将`"Content-Type"`请求头的值设置为`"application/x-www-form-urlencoded"`。

```js
// 获取用户输入的表单数据
var country = document.getElementById("country").value,
    city = document.getElementById("city").value;

// 将数据拼接为 名/值对 的形式
var query = "country=" + country + "&city=" + city;

var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function () {
    // ... ... 省略事件处理程序
}

// 指定 POST 请求
xhr.open("POST", "/statics/demosource/demo_post_json.php");

// 设置请求主体的编码方法
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");

// 发送请求主体（数据）
xhr.send(query);
复制代码
```

### JSON 编码的 POST 请求

JSON 是一种轻量级的前后端`数据交换格式`，直接使用`JSON.stringify`原生 API 即可实现 JSON 编码，比表单编码的方式更加快捷。

POST 请求使用 **JSON编码** 的方式来发送数据的关键步骤：

1. `"Content-Type"`请求头的值需要为`"application/json"`；
2. 对请求主体进行序列化，在 JavaScript 中可使用`JSON.stringify`完成这步操作。

```js
// 获取用户输入的表单数据
var country = document.getElementById("country").value,
    city = document.getElementById("city").value;

// 将数据转换为 JavaScript 对象
var data = {
    country : country,
    city : city
}

var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function () {
    // ... ... 省略事件处理程序
}

// 指定 POST 请求
xhr.open("PO ST", "/statics/demosource/demo_json_data.php");

// 设置请求主体的编码方法
xhr.setRequestHeader("Content-Type", "application/json");

// 编码请求主体并发送
xhr.send(JSON.stringify(data));
复制代码
```

## 两种方式的比较

**GET 请求：**

1. 一般用于信息`获取`：通过发送一个请求来取得服务器上的资源；
2. 数据包含在 URL 地址中；
3. 数据量受 URL 的长度限制；
4. 不安全：浏览器的 URL `可见`到，明文传输；
5. GET 请求`会被缓存`；
6. GET 没有请求主体，请求速度相对较快。

**POST 请求：**

1. 一般用于`修改`服务器上的资源：向指定资源提交数据，后端处理请求后往往会导致服务器 建立新的资源 或 修改已有资源；
2. 数据包含在请求主体中；
3. 没有数据量限制，可在服务器的配置里进行限制；
4. 只能是比 GET `安全`，实际上也是不安全的：可通过开发者工具或者抓包看到，明文传输；
5. POST 请求`不会缓存`；
6. POST 相对稳定、可靠：可发送包含未知字符的内容。

**容易产生的误区**：HTTP 协议里并没有限制 GET 和 POST 的长度，GET 的最大长度限制是因为浏览器和 Web 服务器对 URL 的长度限制，不同的浏览器和 Web 服务器限制的最大长度不一样，它们所限制的是整个 URL 的长度，而不仅仅是查询参数的数据长度。

## Ajax扩展

### jQuery 中的 Ajax

jQuery 是一个 JavaScript 工具库，它封装了 JavaScript 常用的功能代码，包括我们刚刚学完的 Ajax。

**jQuery 中，Ajax 常见的请求方式有以下几种：**

- `$.ajax(url, options)`
- `$.get(url, data, callback, dataType)`
- `$.post(url, data, callback, dataType)`
- `$.getJSON(url, data, callback)`
- `$.getScript(url, callback)`
- jQuery元素`.load(url, data, callback)`

```js
// 使用jQuery发起ajax请求
$.ajax("/statics/demosource/demo_get_json.php", {
    //请求类型
    type: "GET",
    //要发送的数据
    data: {
        country: country,
        city: city
    },
    //数据格式
    dataType: "json",
    //请求成功后执行
    success: function (res) {    // res为响应成功返回的数据
        oIpt_country.innerText = res.params.country;
        oIpt_city.innerText = res.params.city;
    },
    //请求失败后执行
    error: function (res) {    // 这里的res为响应失败返回的数据
        alert("请求失败：" + res.status);
    }
});
```

### Ajax 的替代品：fetch

Fetch API 是随 ES6 发展而出现的一个 JavaScript 原生接口，与 Ajax 一样允许开发者异步发起 HTTP 请求，但却以更加简单明了的调用方式、基于 Promise 的数据处理方式被称作是 Ajax 的替代品。

```js
fetch("/statics/demosource/demo_json_data.php", {
    method: "POST",
    header: new Headers({"Content-Type" : "application/json"}),
    body: JSON.stringify(data)
})
.then(function (res) {
    return res.ok ? res.json() : Promise.reject(res);
})
.then(function (data) {
    oIpt_country.innerText = data.country;
    oIpt_city.innerText = data.city;
})
.catch(function (res) {
    alert("请求失败：" + res.status);
})
```

## JSON

JSON = JavaScript Object Notation，意思是：JavaScript 对象表示法，是一种轻量级的`数据交换格式`。

### 语法规则

JSON 的语法可以表示以下三种类型的值：

- **简单值**：使用与 JavaScript 相同的语法，可以在 JSON 中表示`number`、`string`、`boolean` 与 `null`，但 JSON 不支持 JavaScript 中的特殊值`undefined`；
- **对象**：对象作为一种复杂数据类型， 表示的是一组无序的键值对，而每个键值对中的值可以是简单值，也可以是复杂数据类型的值；
- **数组**：数组也是一种复杂数据类型，表示一组有序的值的列表，数组的值也可以是任意类型 —— 简单值、对象 或 数组。

### 简单值

```js
//JSON 表示数值 7
7
//JSON 表示字符串
"JSON is a format for data exchange"
```

**注意**：

JSON字符串 与 JavaScript字符串 的最大区别在于，JSON 字符串必须使用 **双引号**，单引号会导致语法错误。

### 对象

JSON 中的对象与 JavaScript 对象字面量稍微有一些不同。

JavaScript 的对象字面量：

```js
{
    name : "Alan",
    age : 21
}
```

json表示

```js
{
    "name" : "Alax",
    "age" : 21
}
```

JSON 对象的键（属性名） **必须** 加 **双引号**。

JSON 对象的属性值可以是简单值，也可以是复杂类型值

```js
{
    "name" : "Alan",
    "age" : 21,
    "child" : {
        "name" : "Tim",
        "age" : 7    
    }
}
```

### 数组

JSON 数组采用的就是 JavaScript 中的数组字面量形式。

JavaScript 中的数组字面量：

```js
[21, "Alan", false]
```

json表示

```js
[21, "Alan", false]
```

### **总结**

1. 对于 JSON 与 JavaScript 的关系，你现在可以这么理解：JSON 是 JavaScript 对象的字符串表示法，它使用纯文本格式来表示一个 JavaScript 对象的信息，本质上是一个字符串；
2. 通常，我们会将对象和数组作为 JSON 数据结构的最外层形式，利用它们能够创造出各种各样的数据结构。当然，这不是强制规定的。

## JavaScript 内置的 JSON 对象

ECMAScript 5 定义了一个原生的 JSON 对象，可把 JavaScript 对象序列化为 JSON 字符串，或把 JSON 字符串解析为原生的 JavaScript 值。

**JSON 对象的方法：**

1. `JSON.stringify()`：用于序列化 JavaScript 对象，将其转换为 JSON 字符串；
2. `JSON.parse()`：用于解析 JSON 字符串，将其转换为 JavaScript 值。

**提示**：除了以上两个方法，JSON 对象本身并没有其它作用，也不能被做为构造函数使用。

### stringify 方法

`JSON.stringify()`方法用于将一个 JavaScript 值 / 对象 转换为 JSON 字符串。

```js
var obj = {
    name: "Alan",
    age: 21,
    child: {
        name: "Tim",
        age: 7
    }
};

// 序列化 obj 对象，转换为 JSON 格式的字符串
var json = JSON.stringify(obj)
```

### parse 方法

`JSON.parse()`方法用于将 JSON 数据解析为原生的 JavaScript 值。

```js
var json = '{"name":"Alan","age":21,"child":{"name":"Tim","age":7}}';

// json 数据本质上是字符串，无法直接访问某一属性
console.log(json.name);    // undefined

// 解析 json，转换为原生的 JavaScript 对象
var obj = JSON.parse(json);
console.log(obj.name);    // 此时可以使用 JavaScript 方法访问某一属性
```
