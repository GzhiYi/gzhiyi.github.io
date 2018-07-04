---
title: Webpack-Axios-demo
date: 2018-07-03 16:59:42
tags: 
---
### 前言
原打算只是简单的将axios封装好便于使用，没想到为了写个小demo却要顾及不少的东西。当我直接用ES6语法的时候，node就报错了，所以需要用Babel进行编译，那好，干脆用一下webpack顺便玩玩。

### 整体上
其实这个Demo主要还是封装axios的get和post，让调接口的时候不至于每调一个都写那么多代码，看起来优雅。  
三个文件不一定要同一级目录，不同级的话注意引用的对错。
<!--more-->
```javascript
// http.js 
import axios from 'axios'

let http = axios.create({
  baseURL: '//api.github.com' // baseUrl为接口地址，这里以github api为例
})

export const GET = (url, params, data) => { // 封装的GET对象包含了axios的get方法
  return http({
    method: 'get',
    url,
    params,
    data,
  }).then(res => res.data)
}

export const POST = (url, data) => {// 封装的POST对象包含了axios的get方法
  return http({
    method: 'post',
    headers: {
      'Content-Type': 'application/json;charset=UTF-8'
    },
    url,
    data
  }).then(res => res.data)
}

export default http

// api.js
import * as API from './http'
export default { // 每个接口都可以写在这，方便管理和调用
  getGitUserInfo: (params, name) => {
    return API.GET(`/users/${name}`, params)
  }
} 

// index.js 
import API from './api'
document.getElementById('github-name').onblur = (e) => {
  API.getGitUserInfo({}, e.target.value.trim()).then(res => { // 一个接口只需要通过封装的API进行调用即可
    document.getElementById('result').innerText = JSON.stringify(res, null, 4)
  }).catch(error => {
    document.getElementById('result').innerText = 'who?'
  })
}
```
### webpack配置
版本v4👆
```javascript
const path = require('path')
module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      { test: /\.js$/, exclude: /node_modules/, loader: "babel-loader" }
    ]
  }
}
```
当然还需要babel的配置
```javascript
{
  "presets": [
    "env"
  ]
}
```
相对粗糙，想说详细但是内容很枯燥，看文档的话会清晰很多。