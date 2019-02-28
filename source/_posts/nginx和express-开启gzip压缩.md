---
title: nginx,express 开启gzip压缩
date: 2017-10-19 14:10:56
tags: 
- nginx 
- express
---
开启gzip压缩能大大较少文件请求大小，提升用户体验，那么服务器怎样开启gzip压缩呢？在这里记录一下自己的经验。

## nginx 开启gzip压缩
如果你的静态文件是用nginx管理的，只需要找到nginx.conf配置文件，修改和增加一部分字段：

（1）`vi nginx.conf`

（2）添加如下代码：

``` apacheconf
# 开启gzip
gzip on;
# 启用gzip压缩的最小文件，小于设置值的文件将不会压缩
gzip_min_length 1k;
# gzip 压缩级别，1-10，数字越大压缩的越好，也越占用CPU时间，后面会有详细说明
gzip_comp_level 2;
# 进行压缩的文件类型。javascript有多种形式。其中的值可以在 mime.types 文件中找到。
gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image/jpeg image/gif image/png;
# 是否在http header中添加Vary: Accept-Encoding，建议开启
gzip_vary on;
# 禁用IE 6 gzip
gzip_disable "MSIE [1-6]\.";
```
（3）刷新nginx: `nginx -s reload`

这时候就可以去访问静态资源，看看是开启gzip压缩了，如果开启了，可以在network里查看![Response Headers里是否有Contnt-Encoding:gzip](http://o71pfzm86.bkt.clouddn.com/WX20171019-140126@2x.png)

## express 静态资源托管开启gzip
我有时候会把静态资源放在express的web服务器，express也可以压缩文件，代码也很简单：

``` js
var compression = require('compression');
var express = require('express');
var app = express();
app.use(compression());
```

这样express上托管的静态资源也可以开启了gzip压缩，同时我发现，compression的压缩后的文件比nginx压缩后的文件更小一些