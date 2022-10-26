# html meta属性（set-cookie和cache)

- HTTP-EQUIV类似于HTTP的头部协议，它回应给浏览器一些有用的信息，以帮助正确和精确地显示网页内容。常用的HTTP-EQUIV类型有：

## Content-Type和Content-Language （显示字符集的设定）

- 设定页面使用的字符集，用以说明主页制作所使用的文字已经语言，浏览器会根据此来调用相应的字符集显示page内容。

用法：

```html
<Meta http-equiv="Content-Type" Content="text/html; Charset=gb2312">
<Meta http-equiv="Content-Language" Content="zh-CN">
```

- 注意：该META标签定义了HTML页面所使用的字符集为GB2132，就是国标汉字码。如果将其中的“charset=GB2312”替换成“BIG5”，则该页面所用的字符集就是繁体中文Big5码。当你浏览一些国外的站点时，IE浏览器会提示你要正确显示该页面需要下载xx语支持。这个功能就是通过读取HTML页面META标签的Content-Type属性而得知需要使用哪种字符集显示该页面的。如果系统里没有装相应的字符集，则IE就提示下载。其他的语言也对应不同的charset，比如日文的字符集是“iso-2022-jp ”，韩文的是“ks_c_5601”。

- `Content-Type`的`Content`还可以是：`text/xml`等文档类型；

Charset选项：ISO-8859-1（英文）、BIG5、UTF-8、SHIFT-Jis、Euc、Koi8-2、us-ascii， x-mac-roman， iso-8859-2， x-mac-ce， iso-2022-jp， x-sjis， x-euc-jp，euc-kr， iso-2022-kr， gb2312， gb_2312-80， x-euc-tw， x-cns11643-1，x-cns11643-2等字符集；Content-Language的Content还可以是：EN、FR等语言代码。

## Refresh （刷新)

- 让网页多长时间（秒）刷新自己，或在多长时间后让网页自动链接到其它网页。

用法：

```html
<Meta http-equiv="Refresh" Content="30">
<Meta http-equiv="Refresh" Content="5; Url=http://www.xia8.net">
```

- 注意：其中的5是指停留5秒钟后自动刷新到URL网址。

## Expires （期限)

- 指定网页在缓存中的过期时间，一旦网页过期，必须到服务器上重新调阅。

用法：

```html
<Meta http-equiv="Expires" Content="0">
<Meta http-equiv="Expires" Content="Wed, 26 Feb 1997 08:21:57 GMT">
```

注意：必须使用GMT的时间格式，或直接设为0（数字表示多少时间后过期）。

## Pragma （cach模式）

说明：禁止浏览器从本地机的缓存中调阅页面内容。

用法：

注意：网页不保存在缓存中，每次访问都刷新页面。这样设定，访问者将无法脱机浏览。

## Set-Cookie （cookie设定）

- 浏览器访问某个页面时会将它存在缓存中，下次再次访问时就可从缓存中读取，以提高速度。
- 当你希望访问者每次都刷新你广告的图标，或每次都刷新你的计数器，就要禁用缓存了。
- 通常HTML文件没有必要禁用缓存，对于ASP等页面，就可以使用禁用缓存，因为每次看到的页面都是在服务器动态生成的，缓存就失去意义。如果网页过期，那么存盘的cookie将被删除。

用法：

```html
<Meta http-equiv="Set-Cookie" Content="cookievalue=xxx; expires=Wednesday,21-Oct-98 16:14:21 GMT; path=/">
```

注意：必须使用GMT的时间格式。

## Window-target （显示窗口的设定）

- 强制页面在当前窗口以独立页面显示。

用法：

```html
<Meta http-equiv="Widow-target" Content="_top">
```

注意：这个属性是用来防止别人在框架里调用你的页面。Content选项：_blank、_top、_self、_parent.

## Pics-label （网页RSAC等级评定）

> 在IE的Internet选项中有一项内容设置，可以防止浏览一些受限制的网站，而网站的限制级别就是通过该参数来设置的。

用法：

```html
<META http-equiv="Pics-label" Contect="(PICS－1.1'http://www.rsac.org/ratingsv01.html'I gen comment 'RSACi North America Sever' by 'inet@microsoft.com'for 'http://www.microsoft.com' on '1997.06.30T14:21－0500' r(n0 s0 v0 l0))">
```

注意：不要将级别设置的太高。RSAC的评估系统提供了一种用来评价Web站点内容的标准。用户可以设置Microsoft Internet Explorer（IE3.0以上）来排除包含有色情和暴力内容的站点。上面这个例子中的HTML取自Microsoft的主页。代码中的（n 0 s 0 v 0 l 0）表示该站点不包含不健康内容。级别的评定是由RSAC，即美国娱乐委员会的评级机构评定的，如果你想进一步了解RSAC评估系统的等级内容，或者你需要评价自己的网站，可以访问RSAC的站点：<http://www.rsac.org/>.

## Page-Enter、Page-Exit （进入与退出）

- 这个是页面被载入和调出时的一些特效。

用法：

```html
<Meta http-equiv="Page-Enter" Content="blendTrans(Duration=0.5)">
<Meta http-equiv="Page-Exit" Content="blendTrans(Duration=0.5)">
```
