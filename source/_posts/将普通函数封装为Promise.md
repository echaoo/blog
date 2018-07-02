---
title: 将普通函数封装为Promise
date: 2018-04-26 17:32
tags:
---

##### 问题描述

简单描述一下遇到的问题：原方法是一个对象的方法，这个对象对于我来说是黑盒，接受一个回调方法。而我想要一个能够返回promise的方法，从而方便异步操作，因此我进行了封装，在这记录一下：

```js
const testEvent = {
  uploadImg: (callback) => {
    setTimeout(callback('uploadImg'), 100)
  }
}
```

封装方法： 

```js
const bridge = {
  call: (method, callback, ...arvg) => {
    return new Promise((resolve, reject) => {
      const realCallback = (...arvg) => {
        try {
          resolve(callback(...arvg))
        } catch (e) {
          reject(e)
        }
      }
      const callbackId = window.callBackArr.push(realCallback) - 1 // 项目中需要维护一个回调函数数组，类似于event loop, 如果不需要，直接将realCallback传入即可
      testEvent[method](...arvg, callbackId)
    })
  }
}
```

调用：

```js
test (image) {
     return bridge.call('uploadImg', (res) => {console.log(res)}, 'imageName')

```
以上