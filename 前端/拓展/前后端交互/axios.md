# axios

> 相关: `axios-jsonp`, `axios-retry`
> [axios中文文档|axios中文网 | axios (axios-js.com)](http://www.axios-js.com/zh-cn/docs/)

## 配置header

axios 使用 post 发送数据时，默认是直接把 json 放到请求体中提交到后端的。也就是说，我们的 Content-Type 变成了 application/json;charset=utf-8 ,这是axios默认的请求头content-type类型。但是实际我们后端要求的 'Content-Type': 'application/x-www-form-urlencoded' 为多见，这就与我们不符合。所以很多同学会在这里犯错误，导致请求数据获取不到。明明自己的请求地址和参数都对了却得不到数据。

我们现在来说说post请求常见的数据格式（content-type）

1. Content-Type: application/json ： 请求体中的数据会以json字符串的形式发送到后端
2. Content-Type: application/x-www-form-urlencoded：请求体中的数据会以普通表单形式（键值对）发送到后端
3. Content-Type: multipart/form-data： 它会将请求体的数据处理为一条消息，以标签为单元，用分隔符分开。既可以上传键值对，也可以上传文件。

### application/x-www-form-urlencoded配置

#### a.【用 URLSearchParams 传递参数】代码简单，省事。 需要注意的是： URLSearchParams 不支持所有的浏览器，但是总体的支持情况还是 OK 的

```js
let param = new URLSearchParams()
param.append('username', 'admin')
param.append('pwd', 'admin')
axios({
    method: 'post',
    url: '/api/lockServer/search',
    data: param
})
```

#### b. 配置axios请求头中的content-type为指定类型

`axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded';` 或者

```js
{ headers:{'Content-Type':'application/x-www-form-urlencoded'} }
```

将参数转换为query参数, 利用qs，引入 qs ，这个库是 axios 里面包含的，不需要再下载了。

```js
import Qs from 'qs'
let data = {
    "username": "cc",
    "psd": "123456"
}

axios({
    headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
    },
    method: 'post',
    url: '/api/lockServer/search',
    data: Qs.stringify(data)
})
```

## **Content-Type: multipart/form-data**

```js
 let params = new FormData()
 params.append('file', this.file)
 params.append('id', localStorage.getItem('userID'))
 params.append('userName', this.name)
 params.append('sex', this.sex)
 params.append('mobile', this.phone)
 params.append('email', this.email)
 params.append('qq', this.qq)
 params.append('weChat', this.WeChat)

axios.post(URL, params, {headers: {'Content-Type': 'multipart/form-data'}}).then(res => {
  if (res.data.code === 0) {
  this.$router.go(-1)
  }
  }).catch(error => {
  alert('更新用户数据失败' + error)
})
```

## **Content-Type: application/json**

> axios默认的请求数据类型，我们只需将参数序列化json字符串进行传递即可，无需多余的配置。

## axios-retry

> 让axios支持重试

```ts
import Axios, { AxiosRequestConfig } from 'axios'
import axiosRetry from 'axios-retry'

const client = Axios.create({
  // 你的配置
})

// 安装 retry 插件
// 当请求失败后，自动重新请求，只有3次失败后才真正失败
axiosRetry(client, { retries: 3 })

export async function request(url: string, config?: AxiosRequestConfig) {
  const response = await client.request({ url, ...config })
  const result = response.data
  // 你的业务判断逻辑
  return result
}

// 只有3次失败后才真正失败
const data = request('http://example.com/test')

```

## axios-jsonp

> 支持jsonp的功能

```ts
import Axios, { AxiosRequestConfig } from 'axios'
import jsonpAdapter from 'axios-jsonp'

const client = Axios.create({
  // 你的配置
})

export async function request(url: string, config?: AxiosRequestConfig) {
  const response = await client.request({ url, ...config })
  const result = response.data
  // 你的业务判断逻辑
  return result
}

export function jsonp(url: string, config?: AxiosRequestConfig) {
  return request(url, { ...config, adapter: jsonpAdapter })
}

// 你现在可以发送 jsonp 的请求了
const data = jsonp('http://example.com/test-jsonp')

```
