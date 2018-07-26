---
title: express + mysql简单CRUD示例
date: 2018-01-02
tags: web前端
categories: notes
---

最近写一个博客的接口，就是简单的CRUD，选择了最熟悉的js，用了express框架，因为之前没有接触过，在网上查相关教程，很少有特别完整细致的示例，所以记录一下学习过程。

代码地址: https://github.com/echaoo/backend-example

#### 安装
假设你已经有了Node.js，创建目录，进入目录：
``` bash
 mkdir myapp
 cd myapp
```
使用 npm init 命令为应用程序创建 package.json 文件。根据你的需要接受或者修改缺省值
``` bash
 npm init
```
安装express。如果暂时安装express可以省略--save选项
``` bash
 npm install express --save
```
本项目没有使用express的生成器，只是简单的示例。
完成上述安装后，准备工作就完成啦。

#### 相关代码
具体代码我就不粘贴了，在上述的代码仓库里可以查看详情。在这里我主要想介绍一下大致思路：

1. 首先我们需要启动一个服务，官网的hello world就可以。我们希望可以接受并处理请求，就需要添加一个叫body-parser的中间件
```javascript
var express = require('express');
var bodyParser = require('body-parser');

app.use(bodyParser.json()); // for parsing application/json
app.use(bodyParser.urlencoded({ extended: true })); // for parsing application/x-www-form-urlencoded
```
2. 其次我们需要考虑数据库的问题。
- 首先连接数据库，具体可以参考文档：https://www.npmjs.com/package/mysql。我的示例中连接数据库代码是单独在dbConfig.js文件中的

- 创建一个存放sql语句的文件commentsSqlMapping.js，创建一个操作数据库的文件comments.js，在comments.js文件中封装每个操作数据库的方法，例如：
```javascript
 add (req, res, next) {
        const param = req.body || req.params;
        const connection = mysql.createConnection(config.mysql);
        connection.connect();
        let query = connection.query($sql.insert, [param.title, param.content, param.time], function (err, rows, fields) {
            if (err) {
                res.json({
                    code: '1',
                    msg: '操作失败'
                });
            } else {
                res.json({
                    code: 200,
                    msg: "增加成功"
                });
            }
        });
        connection.end();
    }
```
- 上述是一个完整的连接数据库返回信息的方法，因此我们只需要在接口中调用此方法就可以了:
```javascript
app.post('/api/add', function (req, res, next) {
    comments.add(req, res, next);
});
```

完整代码参见github

以上

