---
title: mvc&mvvm
date: 2020-09-16 22:40:04
tags:
	- mvc
	- mvvm
	- front-end
---

# mvc&mvvm

## MVC

> MVC的思想：一句话描述就是Controller负责将Model的数据用View显示出来，换句话说就是在Controller里面把Model的数据赋值给View，比如在controller中写document.getElementById("box").innerHTML = data[”title”]，只是还没有刻意建一个Model类出来而已。

### M

> Model模型

> - 是应用程序中用于处理应用程序数据逻辑的部分
> - 通常模型对象负责在数据库中存取数据
> - 数据结构模型

### V

> View: 视图

> - 是应用程序中处理数据显示的部分
> - 通常视图是一句模型数据创建的

### C

> Controller: 控制器

> - 是应用程序中处理用户交互的部分
> - 通常控制器负责视图读取数据, 控制用户输入, 并向模型发送数据

## MVVM

> MVVM：Model、View、ViewModel

> - 低耦合。视图（View）可以独立于 Model 变化和修改，一个 ViewModel 可以绑定到不同的"View"上，当 View 变化的时候 Model 可以不变，当 Model 变化的时候 View 也可以不变。
> - 可重用性。你可以把一些视图逻辑放在一个 ViewModel 里面，让很多 view 重用这段视图逻辑。
> - 独立开发。开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计，使用 Expression Blend 可以很容易设计界面并生成 xml 代码。
>
> - 可测试。界面元素本来是比较难于测试的，而现在测试可以针对 ViewModel 来写。

> **MVVM优点**
>
> MVVM模式和MVC模式一样，主要目的是分离视图（View）和模型（Model），有几大优点
>
> 1. **低耦合**。视图（View）可以独立于Model变化和修改，一个ViewModel可以绑定到不同的"View"上，当View变化的时候Model可以不变，当Model变化的时候View也可以不变。
>
> 2. **可重用性**。你可以把一些视图逻辑放在一个ViewModel里面，让很多view重用这段视图逻辑。
>
> 3. **独立开发**。开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计，使用Expression Blend可以很容易设计界面并生成xml代码。
>
> 4. **可测试**。界面素来是比较难于测试的，而现在测试可以针对ViewModel来写。
