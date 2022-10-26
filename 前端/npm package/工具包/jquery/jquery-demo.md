---
title: jquery-demo
date: 2020-09-13 10:56:43
tags:
	- jquery
	- demo
	- javascript
	- front-end
---

# jquery-demo

## 鼠标悬停时, 弹出下拉菜单[重要]

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style type="text/css">
        * {
            margin: 0;
            padding: 0;
        }

        ul {
            list-style: none;
        }

        .wrap {
            width: 330px;
            height: 30px;
            margin: 100px auto 0;
            padding-left: 10px;
            background-color: pink;
        }

        .wrap li {
            background-color: yellowgreen;
        }

        .wrap > ul > li {
            float: left;
            margin-right: 10px;
            position: relative;
        }

        .wrap a {
            display: block;
            height: 30px;
            width: 100px;
            text-decoration: none;
            color: #000;
            line-height: 30px;
            text-align: center;
        }

        .wrap li ul {
            position: absolute;
            top: 30px;
            display: none;
        }
    </style>
   

</head>
<body>
<div class="wrap">
    <ul>
        <li>
            <a href="javascript:void(0);">一级菜单1</a>
            <ul>
                <li><a href="javascript:void(0);">二级菜单1</a></li>
                <li><a href="javascript:void(0);">二级菜单2</a></li>
                <li><a href="javascript:void(0);">二级菜单3</a></li>
            </ul>
        </li>
        <li>
            <a href="javascript:void(0);">一级菜单1</a>
            <ul>
                <li><a href="javascript:void(0);">二级菜单1</a></li>
                <li><a href="javascript:void(0);">二级菜单2</a></li>
                <li><a href="javascript:void(0);">二级菜单3</a></li>
            </ul>
        </li>
        <li>
            <a href="javascript:void(0);">一级菜单1</a>
            <ul>
                <li><a href="javascript:void(0);">二级菜单1</a></li>
                <li><a href="javascript:void(0);">二级菜单2</a></li>
                <li><a href="javascript:void(0);">二级菜单3</a></li>
            </ul>
        </li>
    </ul>
</div>
<script src="https://cdn.bootcdn.net/ajax/libs/jquery/1.10.0/jquery.min.js"></script>
<script>
    //入口函数
    $(document).ready(function () {
        //需求：鼠标放入一级li中，让他里面的ul显示。移开隐藏。
        var jqli = $(".wrap>ul>li");

        //绑定事件
        jqli.mouseenter(function () {
            //这个位置用到了this.
            // console.log(this);  
            //打印结果是js中的dom对象。注意：jquery对象绑定的事件中，this指js中的dom对象。【重要】
            //让this中的ul显示出来。
          //原生 js 的做法是：this.children[1].style.display = "block";
            //把js的dom对象包装为jquery对象，然后用jquery方法操作
            $(this).children("ul").show();
        });

        //绑定事件：鼠标移开时，隐藏下拉菜单
        jqli.mouseleave(function () {
            $(this).children("ul").hide();
        });
    });
</script>
</body>
</html>
```

![img](http://img.smyhvae.com/20180205_1030.gif)

## 突出显示

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style type="text/css">
        * {
            margin: 0;
            padding: 0;
        }

        ul {
            list-style: none;
        }

        body {
            background: #000;
        }

        .wrap {
            margin: 100px auto 0;
            width: 630px;
            height: 394px;
            padding: 10px 0 0 10px;
            background: #000;
            overflow: hidden;
            border: 1px solid #fff;
        }

        .wrap li {
            float: left;
            margin: 0 10px 10px 0;

        }

        .wrap img {
            display: block;
            border: 0;
        }
    </style>
    <script src="jquery-1.11.1.js"></script>
    <script>
        jQuery(window).ready(function () {
            //需求：鼠标放入li中，其他的li半透明，当前盒子的opacity值为1
            $(".wrap").find("li").mouseenter(function () {
                //链式编程
                $(this).css("opacity", 1).siblings("li").css("opacity", 0.4);
            });

            //离开wrap的时候所有的li的全部opacity值为1；
            $(".wrap").mouseleave(function () {
                $(this).children().children("li").css("opacity", 1);
//                $(".wrap li").css("opacity",1);
            });
        });
    </script>
</head>
<body>
<div class="wrap">
    <ul>
        <li><a href="#"><img src="images/01.jpg" alt=""/></a></li>
        <li><a href="#"><img src="images/02.jpg" alt=""/></a></li>
        <li><a href="#"><img src="images/03.jpg" alt=""/></a></li>
        <li><a href="#"><img src="images/04.jpg" alt=""/></a></li>
        <li><a href="#"><img src="images/05.jpg" alt=""/></a></li>
        <li><a href="#"><img src="images/06.jpg" alt=""/></a></li>
    </ul>
</div>
</body>
</html>
```

![img](http://img.smyhvae.com/20180205_1118_2.gif)

## 淘宝精品服饰广告

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style type="text/css">
        * {
            margin: 0;
            padding: 0;
            font-size: 12px;
        }

        ul {
            list-style: none;
        }

        a {
            text-decoration: none;
        }

        .wrapper {
            width: 298px;
            height: 248px;
            margin: 100px auto 0;
            border: 1px solid pink;
            overflow: hidden;
        }

        #left, #center, #right {
            float: left;
        }

        #left li, #right li {
            background: url(images/lili.jpg) repeat-x;
        }

        #left li a, #right li a {
            display: block;
            width: 48px;
            height: 27px;
            border-bottom: 1px solid pink;
            line-height: 27px;
            text-align: center;
            color: black;
        }

        #left li a:hover, #right li a:hover {
            background-image: url(images/abg.gif);
        }

        #center {
            border-left: 1px solid pink;
            border-right: 1px solid pink;
        }
    </style>
    <script src="jquery-1.11.1.js"></script>
    <script>
        jQuery(function () {
            //需求：鼠标放入两侧的li中，让中间的ul中对应索引值的li显示出来，其他的隐藏。（右侧的li要+9）
            //左侧先绑。获取绑mouseenter
            $("#left li").mouseenter(function () {
                //显示对应索引值的中间的li
                //alert($(this).index());  获取索引值
                $("#center li").eq($(this).index()).show().siblings("li").hide();
            });

            //右侧
            $("#right li").mouseenter(function () {
                //显示对应索引值的中间的li
                //alert($(this).index());  获取索引值
                $("#center li:eq(" + ($(this).index() + 9) + ")").show().siblings("li").hide();
            });
        });
    </script>
</head>
<body>
<div class="wrapper">

    <ul id="left">
        <li><a href="#">女靴</a></li>
        <li><a href="#">雪地靴</a></li>
        <li><a href="#">冬裙</a></li>
        <li><a href="#">呢大衣</a></li>
        <li><a href="#">毛衣</a></li>
        <li><a href="#">棉服</a></li>
        <li><a href="#">女裤</a></li>
        <li><a href="#">羽绒服</a></li>
        <li><a href="#">牛仔裤</a></li>
    </ul>
    <ul id="center">
        <li><a href="#"><img src="images/女靴.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/雪地靴.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/冬裙.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/呢大衣.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/毛衣.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/棉服.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/女裤.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/羽绒服.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/牛仔裤.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/女包.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/男包.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/登山鞋.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/皮带.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/围巾.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/皮衣.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/男毛衣.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/男棉服.jpg" width="200" height="250"/></a></li>
        <li><a href="#"><img src="images/男靴.jpg" width="200" height="250"/></a></li>
    </ul>
    <ul id="right">
        <li><a href="#">女包</a></li>
        <li><a href="#">男包</a></li>
        <li><a href="#">登山鞋</a></li>
        <li><a href="#">皮带</a></li>
        <li><a href="#">围巾</a></li>
        <li><a href="#">皮衣</a></li>
        <li><a href="#">男毛衣</a></li>
        <li><a href="#">男棉服</a></li>
        <li><a href="#">男靴</a></li>
    </ul>
</div>
</body>
</html>
```

![img](http://img.smyhvae.com/20180205_1135.gif)



## 手风琴效果

```html
<!DOCTYPE html>
<html>
<head lang="en">
    <meta charset="UTF-8">
    <title></title>
    <style type="text/css">
        * {padding: 0;margin: 0;}
        ul { list-style-type: none;}

        .parentWrap {
            width: 200px;
            text-align:center;
        }

        .menuGroup {
            border:1px solid #999;
            background-color:#e0ecff;
        }

        .groupTitle {
            display:block;
            height:20px;
            line-height:20px;
            font-size: 16px;
            border-bottom:1px solid #ccc;
            cursor:pointer;
        }

        .menuGroup > div {
            height: 200px;
            background-color:#fff;
            display:none;
        }

    </style>
</head>
<body>
  <ul class="parentWrap">
      <li class="menuGroup">
          <span class="groupTitle">标题1</span>
          <div>我是弹出来的div1</div>
      </li>
      <li class="menuGroup">
          <span class="groupTitle">标题2</span>
          <div>我是弹出来的div2</div>
      </li>
      <li class="menuGroup">
          <span class="groupTitle">标题3</span>
          <div>我是弹出来的div3</div>
      </li>
      <li class="menuGroup">
          <span class="groupTitle">标题4</span>
          <div>我是弹出来的div4</div>
      </li>
  </ul>
  <script src="https://cdn.bootcdn.net/ajax/libs/jquery/1.10.0/jquery.js"></script>
    <script>
        $(function () {
            //需求：鼠标点击span，让他下面的div显示出来。让其他的div隐藏。
            $(".parentWrap span").click(function () {
              //$(this).next().show();
              //让其他的隐藏
              //点击的span的父亲li，的所有的兄弟元素li，的孩子元素div全部隐藏。
              //$(this).parent("li").siblings("li").children("div").hide();
              //连式编程
              $(this).next().show().parent("li").siblings("li").find("div").hide();
            });
        })
    </script>
</body>
</html>
```

![img](http://img.smyhvae.com/20180205_1120.gif)