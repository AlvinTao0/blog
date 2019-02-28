---
title: http-proxy-middleware解决前端跨域问题
date: 2017-10-24 10:30:10
tags: 跨域 
---
极简的跨域解决方案，思路是：
1. 用express启动一个web服务器
2. 配置跨域选项

## 目录结构
![](http://o71pfzm86.bkt.clouddn.com/WX20171024-101807@2x.png)
- app.js: 本地web服务器
- src: 前端代码

#### app.js:
```js
const express = require('express');
const proxy = require('http-proxy-middleware'); //引入代理中间件
const app = express();
app.use(express.static('src'));

// Add middleware for http proxying
const apiProxy = proxy('/api', { target: 'http://goldtao.me:4001', changeOrigin: true }); //将服务器代理到http://goldtao.me:4001端口上[本地服务器为localhost:3000]
app.use('/api/*', apiProxy); //api子目录下的都是用代理

app.listen(3000, () => {
    console.log('Listening on: http://localhost:3000');
});
```
#### src/index.html
就是一个简单的页面，点击请求接口
```html
<!DOCTYPE html>
<html>
<head>
    <title>首页</title>
    <meta charset="utf-8">
    <script type="text/javascript" src="//cdn.bootcss.com/jquery/3.1.1/jquery.min.js"></script>
    <style type="text/css">
        .hello {
            color: #428bca;
        }
    </style>
</head>
<body>
    <h3>这是index页面</h3>
    <span class="hello">你可以点击这里</span>
    <script type="text/javascript">
        $(function() {
            $('.hello').on('click', function() {
                $.ajax({
                    type: 'get',
                    url: '/api/get',
                    success: function(data) {
                        alert(data);
                    },
                    error: function(data) {
                        console.log(data);
                    }

                })
            })
        })
    </script>
</body>
</html>
```

## 跨域效果
我在 http://goldtao.me:4001 已经写好了一个对应测试服务;

本地访问localhost:3000, 点击后，成功得到数据，弹出提示 - -
![](http://o71pfzm86.bkt.clouddn.com/WX20171024-102717@2x.png)
