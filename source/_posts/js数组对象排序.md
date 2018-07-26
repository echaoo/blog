---
title: js数组对象排序
date: 2017-07-22 19:58
tags: web前端
categories: notes
---

最近用到排序功能，简单记录一下：

js中用方法sort()排序，默认按ASCII字符顺序排序，在这里我记录三种常见的用法：

#### 1、字符串排序
这是最简单的情况，示例如下：
```javascript
let a = ['zhangsan', 'lisi', 'wangwu']
a.sort()
```
结果为：['lisi', 'wangwu', 'zhangsan']

#### 2、数字排序

如果一个数组如下：
```javascript
let arr = [2, 4, 23, 5, 7]
arr.sort()
```
你可以试一下，这样排序得不到想要的效果，原因是，sort方法在排序时会调用toString方法，得到字符串后，按照字符串的ASCII字符顺序进行排序。好在sort方法接收一个参数，这个参数是一个用来比较的方法，叫比较函数。因此，数字的比较可以这样写：
```javascript
let arr = [2, 4, 23, 5, 7]
let compare = function(x, y) {
  return x - y
}
arr.sort(compare)
```
简单的来说，当(x - y)返回负数则交换x和y的顺序，返回0和正数顺序不变

#### 3、数组对象排序
有时候我们需要按照对象中的某一个属性的值来排序，可以这样写：
```javascript
let arr = [{name: 'zhangsan', age: 12}, {name: 'lisi', age: 10}]
let compare = function(prop) {
  return function(obj1, obj2) {
    return obj1[prop] - obj2[prop]
  }
  arr.sort(compare("age"))
}
```
这样就能很好的得到我们想要的效果。以上
