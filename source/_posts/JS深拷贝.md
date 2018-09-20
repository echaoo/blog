---
title: JS深拷贝
date: 2018-03-10 15:30
tags: web前端, js
categories: notes
---
##### 深拷贝实现方式一： 递归

```js
var cloneObj = function (obj) {
    var str, newobj = obj.constructor === Array ? [] : {};
    if(typeof obj !== 'object'){
        return;
    } else if (window.JSON){
        str = JSON.stringify(obj), //系列化对象
        newobj = JSON.parse(str); //还原
    } else {
        for (var i in obj) {
            newobj[i] = typeof obj[i] === 'object' ?
            cloneObj(obj[i]) : obj[i];
        }
    }
    return newobj;
};
```

##### 方式二： JSON
```js
const test = {
    children: [
        {
            name: 'test1',
            age: 12
        }
    ],
    message: '1221'
}

const result = JSON.parse(JSON.stringify(test))

```