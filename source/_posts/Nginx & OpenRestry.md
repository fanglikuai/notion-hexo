---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SSNHKA56%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T130044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJHMEUCICsBng5aKj%2Besx1IigHMawp0qar2DI9wATdcOIfnephwAiEA7f24VMKiPEimYH0L9KRF3OapmSjugSF9YI%2FaDqtzbkoq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDLjBoXYoA66HJ%2Fi8CSrcA9Xw0fVwmDL2fKn1kXYHWlyE7JErWZBvozv7AlUV0C0mrhUywjWSQIQTjhTmXlRV0J%2FuxJPF21JD4ZgCb9I8x8OrWDLJTW2mLkUc%2BzXlHOD9Dl8u98FFS6wyRbe057SiGviDQ6mrwabk%2F2eunUEqF%2FsXS%2BolbLH2a4TGVWaIvaYMKaI8LBNuqtPKoPP50D%2F%2F0r5sPwPA%2BvtQx5DfWwQsM4vYjpPINvY0Jw5DQDB9ZUqiatSnyM7SbmPR3gTMjNsHAvco6q9a7oYXwaM2R0TX83Jl7byzRhdqa5K80Ruh2r5Gua6mCnKc7OhQ%2B6tBpqReclK3a2cjPAGD6YQmQUnsTgNcX7yP1Nkx9Z2sBVKa1FHwsR26aL7bq2m20v5RbU4dssI80jskvOijQJ%2FynLsJ2ZYB5w17Bq4%2BGkNyr6AEoBoC6%2B%2B4CyWPQJ4p%2BSXzjJThDroRCa3mBPVet3vUqIDKAsGVLcVG8OA2UlLCMLJjqoidExBKCDStFMTcsDd%2BfX92N6Ry8bGWK19h%2BOqDpRK35zVGRiXoY33weQr%2BbZF1fdJVbPuXgrAoP1EzXj%2BDsS7yrRnuVnS0iHS84%2FI7Xf0QpSTgfcPaxMbiCbotydQwGzRrsKgxAA8gKSVt8Z%2BeMIDbksgGOqUBUaitUYUhbtzOs%2Bdeh5QTa7yInMZLX8it3Mnj5zXRHYf%2BBPajGVe00YT02bPXPGFQ%2F%2B0TYu2QyFiNvmuLbtrGeqFWRfbINRgPmp3fUIHZVqZQIWfmM1E%2BMjEycTGrls7XC9ztkWCkCZ1VJmR6sHGbDByrrhRVw4rEVlu00noQDi5IAcnahMEfHmPk8nyJBeg%2B92DzTQyyUOpurWXgE%2FKP0rBSg524&X-Amz-Signature=192a4a2b5369fd947a19f0071fd0767e67377961d641afd1a21b973fb52da6a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 21:24:00'
index_img: /images/681caddd167c86081c93eb4da2dc581a.png
banner_img: /images/681caddd167c86081c93eb4da2dc581a.png
---

# 基本概念


**Nginx (engine x)** 是一款轻量级的 Web 服务器 、反向代理服务器及电子邮件（IMAP/POP3）代理服务器。


**反向代理与正向代理的区别：**


正向代理：在用户这一端，vpn


反向代理：在服务器端，nginx

> 拓展：
>
> 堡垒机：统一的运维入门，带权限认证
>
>

基本使用：


```bash
nginx -s stop
#快速关闭Nginx，可能不保存相关信息，并迅速终止web服务。nginx -s quit
#平稳关闭Nginx，保存相关信息，有安排的结束web服务。nginx -s reload
#因改变了Nginx相关配置，需要重新加载配置而重载。nginx -s reopen
#重新打开日志文件。nginx -c filename
#为 Nginx 指定一个配置文件，来代替缺省的。nginx -t
#不运行，而仅仅测试配置文件。nginx 将检查配置文件的语法的正确性，并尝试打开配置文件中所引用到的文件。nginx -v
#显示 nginx 的版本。nginx -V
#显示 nginx 的版本，编译器版本和配置参数。
```


# 实战


反向代理域名的tomcat


```plain text
upstream zp_server1{
  server 127.0.0.1:8080;
  # 写要代理的地方
}
server {
  listen       80;
  server_name  www.helloworld.com; #从哪里来的域名

  #charset koi8-r;

  #access_log  logs/host.access.log  main;

  location / {
    #  root   html;
    # index  index.html index.htm;
    proxy_pass http://zp_server1;
    #进行代理
  }
```


## 跨域问题

1. 在 Nginx 的`server` 或`location`块中添加以下头部：

```plain text
location / {
  add_header 'Access-Control-Allow-Origin' '*';
  add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, PUT, DELETE';
  add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization';
  add_header 'Access-Control-Expose-Headers' 'Content-Length,Content-Range';

  # 处理预检请求 OPTIONS
  if ($request_method = 'OPTIONS') {
    add_header 'Access-Control-Allow-Origin' '*';
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, PUT, DELETE';
    add_header 'Access-Control-Allow-Headers' 'DNT,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type,Range,Authorization';
    add_header 'Access-Control-Max-Age' 1728000;
    add_header 'Content-Type' 'text/plain; charset=utf-8';
    add_header 'Content-Length' 0;
    return 204;
  }

  # 其他请求正常处理
  ...
}
```

1. 指定的域名可以跨域
