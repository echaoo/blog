---
title: hexo Cannot read property 'offset' of null

date: 2017-12-26 18:38
tags:
---

写博客编译的时候一直报这个错误，查了一下竟然是时区错误，记录一下：
在_config.yml里，找到`#Site`里的timezone,将timezone更改为你所在的时区，我就找了一个亚洲的，看着接近中国的：Asia/Ho_Chi_Minh

时区对照表：https://en.wikipedia.org/wiki/List_of_tz_database_time_zones