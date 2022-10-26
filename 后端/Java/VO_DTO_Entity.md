---
title: VO_DTO_Entity
date: 2021-01-20 09:33:03
tags:
- java
---

# VO_DTO_Entity

> 1、entity 里的每一个字段，与数据库相对应，
>
> 2、vo 里的每一个字段，是和你前台 html 页面相对应，
>
> 3、dto 这是用来转换从 entity 到 vo，或者从 vo 到 entity 的中间的东西 。



举个例子：

你的html页面上有三个字段，name，pass，age

你的数据库表里，有两个字段，name，pass ， 注意没有 age。

而你的 vo 里，就应该有下面三个成员变量 ，因为对应 html 页面上三个字段 。

```java
private string name；
private string pass; 
private string age;
```

这个时候，你的 entity 里，就应该有两个成员变量 ，因为对应数据库表中的 2 个字段 。

```java
private string name；
private string pass;
```

到了这里，好了，业务经理让你做这样一个业务“年龄大于 20 的才能存入数据库，这个时候，你就要用到 dto 了，

1）你要先从页面上拿到 vo，然后判断 vo 中的 age 是不是大于 20。

2）如果大于 20，就把 vo 中的 name 和 pass 拿出来，放到 dto 中。

3）然后在把 dto 中的 name 和 pass 原封不动的给 entity，然后根据 entity 的值，在传入数据库。

这就是他们三个的区别。

PS： dto 和 entity 里面的字段应该是一样的，dto 只是 entity 到 vo，或者 vo 到 entity 的中间过程，如果没有这个过程，你仍然可以做到增删改查，这是根据具体公司规范来的 。