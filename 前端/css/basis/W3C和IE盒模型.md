# W3C盒模型和IE盒模型

> E的盒模型问题只会出现在IE5.5及其更早的版本中，因为在IE6及更新的版本在标准兼容模式下使用的是W3C的盒模型标准，但事实上不仅IE5.5，连IE6的使用率也很少了，这里对他们进行讨论只是为了更深刻的理解盒模型。

![img](W3C和IE盒模型.assets/20150629102231720)

## 例子

```html
<!-- 没有声明 -->
<html>
<head lang="ch">
    <meta charset="UTF-8">
    <title></title>
    <style>
        .box{
            float:left;
            width: 100px;
            height: 100px;
            background-color: #ff9000;
        }
        .container{
            width: 200px;
            padding: 10px;
            background-color: #f00;
            overflow: hidden;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="box"></div>
        <div class="box"></div>
    </div>
<script>
</script>
</body>
</html>
```

无论IE还是其他都会显示

![img](W3C和IE盒模型.assets/20150629102836021)

但是给container添加padding

```css
.container{
    width: 200px;
    background-color: #f00;
    overflow: hidden;
    padding: 10px;
}
```

则其在非IE下的样式为：



![W3C](W3C和IE盒模型.assets/20150629103146025)

但在IE下为：

![IE](W3C和IE盒模型.assets/20150629103238709)