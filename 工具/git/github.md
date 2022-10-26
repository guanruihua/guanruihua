---
title: github
date: 2020-11-20 20:11:36
tags:
- github
- git
---

# github

## 速度太慢

## github图片加载问题

> 在原本的``url`地址上加`cnpmjs.org`
>
> 例如:`github.com/...`=>`github.com.cnpmjs.org/...`

添加到hosts文件( `C:\Windows\System32\drivers\etc` ) 中

> - 但是还是会有丢包的情况出现
> - 若文件无法修改记得把文件只读的选项去掉

```tiki wiki
# GitHub Start 

192.30.253.112    Build software better, together 
192.30.253.119    gist.github.com
151.101.184.133    assets-cdn.github.com
151.101.184.133    raw.githubusercontent.com
151.101.184.133    gist.githubusercontent.com
151.101.184.133    cloud.githubusercontent.com
151.101.184.133    camo.githubusercontent.com
151.101.184.133    avatars0.githubusercontent.com
151.101.184.133    avatars1.githubusercontent.com
151.101.184.133    avatars2.githubusercontent.com
151.101.184.133    avatars3.githubusercontent.com
151.101.184.133    avatars4.githubusercontent.com
151.101.184.133    avatars5.githubusercontent.com
151.101.184.133    avatars6.githubusercontent.com
151.101.184.133    avatars7.githubusercontent.com
151.101.184.133    avatars8.githubusercontent.com

 # GitHub End
```

然后还是发现有的图片无法显示就可以通过以下来解决

> 1. 在无法查看图片的页面按下``F12`
> 2. 找到该图片的链接 例如:`https://avatars2.githubusercontent.com/u/15832957?s=60&v=4`
> 3. 打开<https://www.ipaddress.com/>
> 4. 输入`avatars2.githubusercontent.com`进去就可以看到域名的信息和IP地址
> 5. 然后添加进``hosts文件``去

```git
ip地址    avatars2.githubusercontent.com 的格式
```

```git
echo "# File" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:guanruihua/File.git
git push -u origin main



git remote add origin git@github.com:guanruihua/File.git
git branch -M main
git push -u origin main
```

## 无法访问github

> 绕过DNS, 直接使用本地DNS记录进行直接跳转

## 1.通过命令提示符ping到GitHub的IP

打开命令符：win+R，输入cmd，回车即可调出命令符

ping：在命令行中输入ping [http://github.com](https://link.zhihu.com/?target=http%3A//github.com)，记录红框中的IP

![img](https://pic3.zhimg.com/80/v2-305df8d0e362b9b04904f0817917f94e_1440w.jpg)

## 2.DNS查询

在浏览器中打开DNS查询网站：[http://tool.chinaz.com/dns?type=1&host=github.com&ip=](https://link.zhihu.com/?target=http%3A//tool.chinaz.com/dns%3Ftype%3D1%26host%3Dgithub.com%26ip%3D)

选一个TTL值小的IP记录下来

![img](https://pic3.zhimg.com/80/v2-3878daee945c457c5a0ce75d8d279f3a_1440w.jpg)

## 3.修改HOST

通常在修改系统host的时候，为编辑器授权管理员权限是一件比较麻烦的事情，这里给大家介绍一种非常简单的方式。详情见文章：[用创建桌面快捷方式的方法快速修改HOST](https://link.zhihu.com/?target=https%3A//pzyer.com/tips/%E7%94%A8%E5%88%9B%E5%BB%BA%E6%A1%8C%E9%9D%A2%E5%BF%AB%E6%8D%B7%E6%96%B9%E5%BC%8F%E7%9A%84%E6%96%B9%E6%B3%95%E5%BF%AB%E9%80%9F%E4%BF%AE%E6%94%B9HOST.html)

操作完成后，这里我们将前面查到的两个IP都写进HOST记录

![img](https://pic1.zhimg.com/80/v2-bb2b0fd6de49540d951e917c3d6e24a0_720w.jpg)

## 4.刷新本地DNS缓存

打开命令提示符，输入：ipconfig /flushdns

返回显示：
Windows IP 配置
已成功刷新 DNS 解析缓存。

## 5.成功优化

这个时候我们再次访问就会发现加载速度快多了，或者之前不能访问，现在能访问了
