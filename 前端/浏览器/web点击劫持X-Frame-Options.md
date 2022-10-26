# Web 点击劫持X-Frame-Options

### 原理解释

> 点击劫持，[clickjacking](https://baike.baidu.com/item/clickjacking)，也被称为UI-覆盖攻击。这个词首次出现在2008年，是由[互联网](https://baike.baidu.com/item/互联网)安全专家罗伯特·汉森和耶利米·格劳斯曼首创的。

它是通过覆盖不可见的框架误导受害者点击。

虽然受害者点击的是他所看到的网页，但其实他所点击的是被黑客精心构建的另一个置于原网页上面的透明页面。

这种攻击利用了HTML中`<iframe>`标签的透明属性。

就像一张图片上面铺了一层透明的纸一样，你看到的是黑客的页面，但是其实这个页面只是在底部，而你真正点击的是被黑客透明化的另一个网页。一个简单的点击劫持例子，就是当你点击了一个不明链接之后，自动关注了某一个人的博客或者订阅了视频。

假如我在优酷发布了很多视频，想让更多的人关注它，于是我们准备了一个页面：

```html
<!DOCTYPE html>
<html>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<head>
<title>点击劫持 POC</title>
<style>
iframe {
  width: 1440px;
  height: 900px;
  position: absolute;
  top: -0px;
  left: -0px;
  z-index: 2;
  -moz-opacity: 0;
  opacity: 0;
  filter: alpha(opacity=0);
}

button {
  position: absolute;
  top: 270px;
  left: 1150px;
  z-index: 1;
  width: 90px;
  height:40px;
}
</style>
</head>
<body>
  <button>美女图片</button>
  <img src="http://pic1.win4000.com/wallpaper/2018-03-19/5aaf2bf0122d2.jpg">
  <iframe src="http://i.youku.com/u/UMjA0NTg4Njcy" scrolling="no"></iframe>
</body>
</html>

```

当然真正的页面肯定会更精致一些，不会这么简陋。

然而这个页面只是表象而已，我们把iframe的透明度改成0.3以后再看看

![img](web点击劫持X-Frame-Options.assets/20180319164653726)

当你点击按钮以后，真正的点击的其实是隐藏的那个页面的订阅按钮，然后就会在你不知情的情况下订阅了

![img](web点击劫持X-Frame-Options.assets/20180319164951941)

当然点击劫持的危害可大可小，并不仅仅是关注订阅之类的东西，但是原理类似。

### 解决方案

X-Frame-Options HTTP 响应头是用来给浏览器指示允许一个页面可否在 <frame>, </iframe> 或者 <object> 中展现的标记。网站可以使用此功能，来确保自己网站的内容没有被嵌套到别人的网站中去，也从而避免了点击劫持 (clickjacking) 的攻击。

X-Frame-Options三个参数:

1、DENY

表示该页面不允许在frame中展示，即便是在相同域名的页面中嵌套也不允许。

2、SAMEORIGIN

表示该页面可以在相同域名页面的frame中展示。

3、ALLOW-FROM uri

表示该页面可以在指定来源的frame中展示。

换一句话说，如果设置为DENY，不光在别人的网站frame嵌入时会无法加载，在同域名页面中同样会无法加载。另一方面，如果设置为SAMEORIGIN，那么页面就可以在同域名页面的frame中嵌套。正常情况下我们通常使用SAMEORIGIN参数。

Apache配置

需要把下面这行添加到 'site' 的配置中

```bash
Header always append X-Frame-Options SAMEORIGIN
```

nginx配置

需要添加到 ‘http’, ‘server’ 或者 ‘location’ 的配置项中，个人来讲喜欢配置在‘server’ 中

正常情况下都是使用SAMEORIGIN参数，允许同域嵌套

```bash
add_header X-Frame-Options SAMEORIGIN;
```

允许单个域名iframe嵌套

```bash
add_header X-Frame-Options ALLOW-FROM http://whsir.com/; 
```

允许多个域名iframe嵌套，注意这里是用**逗号**分隔

```bash
add_header X-Frame-Options "ALLOW-FROM http://whsir.com/,https://cacti.org.cn/";
```

IIS配置

添加下面的配置到 ‘Web.config’文件中

```xml
<system.webServer>
  ...
  <httpProtocol>
    <customHeaders>
      <add name="X-Frame-Options" value="SAMEORIGIN" />
    </customHeaders>
  </httpProtocol>
  ...
</system.webServer>
```

HAProxy配置

添加下面这行到 ‘front-end, listen, or backend’配置中

```bash
rspadd X-Frame-Options:\ SAMEORIGIN
```

Tomcat配置

在 ‘conf/web.xml’填加以下配置

```xml
<filter>
        <filter-name>httpHeaderSecurity</filter-name>
        <filter-class>org.apache.catalina.filters.HttpHeaderSecurityFilter</filter-class>
        <init-param>
            <param-name>antiClickJackingOption</param-name>
            <param-value>SAMEORIGIN</param-value>
        </init-param>
        <async-supported>true</async-supported>
    </filter>
<filter-mapping>
        <filter-name>httpHeaderSecurity</filter-name>
        <url-pattern>/*</url-pattern>
    <dispatcher>REQUEST</dispatcher>
    <dispatcher>FORWARD</dispatcher>
</filter-mapping>
```

配置后如何确定X-Frame-Options是否已生效呢？我这里以Google浏览器为例，打开网站按F12键，选择Network，找到对应的Headers，如下图所示

![img](web点击劫持X-Frame-Options.assets/aHR0cHM6Ly9jZG4ud2hzaXIuY29tL3dwLWNvbnRlbnQvdXBsb2Fkcy8yMDE5LzAzL1gtRnJhbWUtT3B0aW9uc19IZWFkZXJzLnBuZw)
