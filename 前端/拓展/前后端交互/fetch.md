---
title: fetch
date: 2020-12-14 15:10:21
tags: 
- fetch
- js
- front-end
---

# fetch

> Fetch 的核心在于对HTTP接口的抽象

## fetch() 方法的使用

> `fetch(input ?: Requset | string, init ?: RequestInit ): Promise<Response>`

```js
fetch(url, option).then(function(response){
  // 处理 HTTP 响应
}, function(error){
 // 处理网络错误
})
```

## fetch() 参数

> `fetch(input ?: Requset | string, init ?: RequestInit ): Promise<Response>`
>
> - input 参数 : 字符串, | 包含获取资源的URL | 一个Request 对象
> - option : (可选) , 一个配置对象
>   - method: 请求方式 [GET, POST]
>   - headers: 请求的头信息, 包含与请求关联的Headers对象
>   - body : 请求的body信息,  [ 注意: GET 或 HEAD 方法的请求不得包含body信息 ]
>   - mode : 请求模式 , cors, no-cors 或 same-origin
>   - credentials: 请求 的credentidials, 如omit, same-origin 或 include [ 为了在当前域名内自动发送cookie, 必须提供这个选项 ]

## 常用Fetch请求

### html

```js
fetch('/index/fetchHtml')
 .then(res => {
 return res.text()
}).then( result => {
  document.body.innerHTML += result
}).catch( err => {
  // errs
})
```

### JSON

```js
fetch('/api/user/a')
 .then( res => return res.josn())
 .then( json => console.log(json))
 .catch( err => {
    // err
  })
```

### POST Form

```js
function postForm(){
  const form = document.querySelector('form')
  const name = encodeURL(document.getElementByName('name')[0].value)
  fetch(`/api/user/${name}`,{
    method: 'POST',
    body: new FormData(form)
 })
}
```

### POST JSON

```js
fetch('/api/user/a', {
  method: 'POST',
  header: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: 'a',
    age: 23,
  })
})
```

## fetch中止

> fetch自身没有提供中止请求的话方法, 但是大部分浏览器有实现AbortController, 可以通过AbortController中止fetch请求

```js
const controller = new AbortController();
const signal = controller.signal;
setTimeout(() => controller.abort(), 5000);


fetch('/api/user/CaiCai', {
  signal, // 在option中加入signal
  method: 'POST',
  // credentials:'include',
  headers: {
    'Content-Type': 'application/json'
  },
  body: JSON.stringify({
    name: 'CaiCai',
    age: '26',
  })
}).then((res) => {
  return res.json()
}).then((result) => {
  console.log(result)
}).catch((err) => {
  console.log(err)
})
```

## 兼容性

![Snipaste_2020-12-14_16-00-39](https://gitee.com/grh-gitee/picgo/raw/master/Snipaste_2020-12-14_16-00-39.png)

## 缺点

1. fetch不支持jsonp, 使用到该功能需要单独实现JSONP
2. fetch自身没有abort的方法, 需要AbortController去处理中止, AbortController兼容性也不是很好
3. fetch兼容性不是很好, 不支持的浏览器可以使用`fetch polyfill`
