---
title: iso H5页面滚动不流畅的问题
date: 2018-06-21
tags: 
---

最近做一个需要在移动端浏览的静态页面，发现在ios下滑动页面时，页面滚动很卡顿。下面记录一下解决方法：

- 查了一些资料，大家推荐在滚动容器内添加-webkit-overflow-scrolling: touch;但是我添加了以后效果并不明显

- 在添加了-webkit-overflow-scrolling: touch;的基础上，将容器设置固定高度，添加overflow：auto；滑动很明显流畅很多

示例如下：

```css
.content {
    height: 100vh;
    padding: 30px;
    overflow: auto;
    -webkit-overflow-scrolling: touch;
}
```

以上
