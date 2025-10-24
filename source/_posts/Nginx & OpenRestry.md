---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466STX5JA5P%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T020054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDQC3tuZ0%2FuO5DnkD%2FSV3LoLJX8oGtmnlJf74ot0ROM9gIhAJuwcXfaNQYMVVV6jPOFjBzJqtldYDiNmjoB7z9ms%2BJ4Kv8DCFIQABoMNjM3NDIzMTgzODA1Igxdww8WbheFBhBLZC0q3APwMDKd2vRWxmvlYcMBXemgJRUwctPcTrFl%2BgnxrhTRD2QAhOsqU9XzutIgJkI71tsbDKlEZ%2BjiQV2uwdigXGRZ54fUPJjURHK50ylMCIjc9lLkl7Oh7W4WqevHHJMDEiw0Itu4oZmclfcQOWRmsqIdvGeuJlRRseVAWe30WervZgMILQkimZ0GnJ1FwgLCz44jAPREfjc8QqYfH1L4r9eBquQ8VYubo6TGd7mKx5QEnPN0rbx%2BHLSN%2Bq88dHQ6paRXPqYOHhXWQodyL5FO22c3FyZG6b48qhLgd0OMw0Rzu89P4WiWshVi5GYFSae1683LZgtVGz3JgVCzh5FnlszVw%2B7GnPts8Q2c50U4%2Bf%2FMHHJj19XX9q9ffbkeU0z89MEAYpCMmk0NG391bhJCk9YG42LyRJyGHfJB7Ed4vaLnVhlQc%2BoaSWbjje9Rqcrc7zPwnfnnhbKKkNX99ej1I45LdeWRDyqKVfZAx3NGSWW6W4gy%2BcjjcBCjE7R%2BO0pC5qpo0YVdRsd3uWXXBRum%2FrEBm6%2BKo2%2F5H68RYAg5thbDTurS7n2A3Zv0y0db4w2ahnYBesTBdU0yUs1p4ZYX4baiCTNEK54%2BVSz%2BXB47jl%2F6z0cd6jRlgqzP9ThsZzDhqevHBjqkAcUXNZg3mD5lgasy94xeFbIMP8P22OLHw2A4oM1dpdiITbJdILMZWg30uuQr57eDTg3rjM3wAednv0xvJodwaLiNuPsfYuGwiRJ5WhS9kg1kwUENKm4kg4OqzOwSUyEAAOmAQS%2B3mSe%2FqOOCkbBclSLYhcTI5d9fLW%2Bp9pXkJ6XmwR7fveX%2B2WqLOqm11hWqrVETYc%2B29nWIY%2F%2FsTNirIU%2BXdBuC&X-Amz-Signature=9912cee654b00148cab2dea2f78e4fee047f953fdb51c6700460a11d907cb6a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
