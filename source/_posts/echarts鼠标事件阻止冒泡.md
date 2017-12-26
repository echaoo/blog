---
title: echarts鼠标事件阻止冒泡
date: 2017-11-29 15:46
tags:
---
使用echarts的鼠标事件时，需要阻止鼠标冒泡事件。本来考虑自己捕捉事件，然后通过event.stopPropagation()来阻止冒泡事件，发现事件总是捕捉不成功，而且代码非常冗余。打印出来echarts鼠标事件的回调参数仔细一看，发现回调参数里有一个event属性，点进去一看，看到里面又包含了一个event属性，这个event就是我们需要的鼠标事件！
有了这个鼠标事件，再想阻止冒泡或者干什么的都不是问题了，你可以直接写：params.event.event.stopPropagation()

OK，问题解决
