---
title: nginx部署静态服务器
date: 2018-05-21 19:58
tags: 服务端; nginx
categories: server
---

对于很多不熟悉服务器端的同学来说，部署一个静态服务器貌似是很高深的事情，其实很简单，这次简单记录一下：

#### 原料
CentOS7、nginx

##### 添加Nginx到YUM源

添加CentOS 7 Nginx yum资源库,打开终端,使用以下命令:
```bash
sudo rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```

##### 安装nginx
```bash
sudo yum install -y nginx
```

##### 启动nginx
```bash
nginx
```
到这如果顺利的话，访问你的ip应该能看到nginx的欢迎页面了

#### Nginx配置信息
网页存放默认目录
```bash
/usr/share/nginx/html
```
网站默认站点配置
```bash
/etc/nginx/conf.d/default.conf
```
自定义Nginx站点配置文件存放目录
```bash
/etc/nginx/conf.d/
```
Nginx全局配置
```bash
/etc/nginx/nginx.conf
```
重启nginx
```bash
nginx -s reload
```
注： 如果遇到nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)
的错误，可以查看一下端口占用情况,然后kill掉这个进程：
```bash
netstat -ntpl
kill 端口
```


以上
