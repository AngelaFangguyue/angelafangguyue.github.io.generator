---
title: "在vue中封装axios"
date: 2020-05-01T15:59:30+08:00
draft: false
categories:
  - vue axios
tags:
  - vue axios
featured_image:
---

axios 是 Vue 官方推荐的一个 HTTP 库，用 axios 官方简介来介绍它，就是：Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

虽然，axios 是个优秀的 HTTP 库，但是，直接在项目中使用并不是那么方便，所以，我们需要对其进行一定程度上的配置封装，减少重复代码，方便调用。

在 vue 项目中，和后台交互获取数据这块，我们通常使用的是 axios 库，它是基于 promise 的 http 库，可运行在浏览器端和 node.js 中。他有很多优秀的特性，例如拦截请求和响应、取消请求、转换 json、客户端防御 XSRF 等

其实，网上关于 axios 封装的代码不少，但是大部分都是在入口文件（main.js）中进行 axios 全局对象属性定义的形式进行配置，类似于如下代码：

```
axios.defaults.timeout = 10000
```

该方案有两个不足，首先，axios 封装代码耦合进入入口文件，不方便后期维护；其次，使用 axios 全局对象属性定义的方式进行配置，代码过于零散。

针对问题一，使用了 Vue 源码结构中的一大核心思想——将功能拆分为文件，方便后期的维护。单独创建一个 http.js 或者 http.ts 文件，在文件中引入 axios 并对其进行封装配置，最后将其导出并挂载到 Vue 的原型上即可。此时，每次修改 axios 配置，只需要修改对应的文件即可，不会影响到不相关的功能。

针对问题二，采用 axios 官方推荐的，通过配置项创建 axios 实例的方式进行配置封装。
代码如下：

```
// http.js
import axios from 'axios'
// 创建 axios 实例
const service = axios.create({
  // 配置项
})
```

接着就是在配置项中配置，根据环境设置 baseURL，统一设置请求头，设置跨域超时响应码的处理，请求响应处理，拦截器等。具体的封装代码如下：

```
// http.ts
import axios, { AxiosRequestConfig, AxiosResponse } from 'axios'

const showStatus = (status: number) => {
  let message = ''
  switch (status) {
    case 400:
      message = '请求错误(400)'
      break
    case 401:
      message = '未授权，请重新登录(401)'
      break
    case 403:
      message = '拒绝访问(403)'
      break
    case 404:
      message = '请求出错(404)'
      break
    case 408:
      message = '请求超时(408)'
      break
    case 500:
      message = '服务器错误(500)'
      break
    case 501:
      message = '服务未实现(501)'
      break
    case 502:
      message = '网络错误(502)'
      break
    case 503:
      message = '服务不可用(503)'
      break
    case 504:
      message = '网络超时(504)'
      break
    case 505:
      message = 'HTTP版本不受支持(505)'
      break
    default:
      message = `连接出错(${status})!`
  }
  return `${message}，请检查网络或联系管理员！`
}

const service = axios.create({
  // 联调
  baseURL: process.env.NODE_ENV === 'production' ? `/` : '/apis',
  headers: {
    get: {
      'Content-Type': 'application/x-www-form-urlencoded;charset=utf-8'
    },
    post: {
      'Content-Type': 'application/json;charset=utf-8'
    }
  },
  // 是否跨站点访问控制请求
  withCredentials: true,
  timeout: 30000,
  transformRequest: [(data) => {
    data = JSON.stringify(data)
    return data
  }],
  validateStatus () {
    // 使用async-await，处理reject情况较为繁琐，所以全部返回resolve，在业务代码中处理异常
    return true
  },
  transformResponse: [(data) => {
    if (typeof data === 'string' && data.startsWith('{')) {
      data = JSON.parse(data)
    }
    return data
  }]
})

// 请求拦截器
service.interceptors.request.use((config: AxiosRequestConfig) => {
    return config
}, (error) => {
    // 错误抛到业务代码
    error.data = {}
    error.data.msg = '服务器异常，请联系管理员！'
    return Promise.resolve(error)
})

// 响应拦截器
service.interceptors.response.use((response: AxiosResponse) => {
    const status = response.status
    let msg = ''
    if (status < 200 || status >= 300) {
        // 处理http错误，抛到业务代码
        msg = showStatus(status)
        if (typeof response.data === 'string') {
            response.data = {msg}
        } else {
            response.data.msg = msg
        }
    }
    return response
}, (error) => {
    // 错误抛到业务代码
    error.data = {}
    error.data.msg = '请求超时或服务器异常，请检查网络或联系管理员！'
    return Promise.resolve(error)
})

export default service
```

axios 在默认请求头为 application/json 时，会自动进行序列化处理，不需要开发者再去配置这里了。

[参考链接 1：](https://juejin.im/post/5da90c3e6fb9a04e031c0413)
[参考链接 2：](https://juejin.im/post/5b55c118f265da0f6f1aa354)
