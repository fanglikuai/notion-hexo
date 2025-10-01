---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IX7LHLG%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T170048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDw5DVeWCs3s6MaqR%2FDa5meA0TOYv5o0uG5TH9dKF6EyAIgaC17yyrIzd9U2vM0iEQD3%2Bqpvdl0ojz5Oq2dfcUE0sQq%2FwMIGhAAGgw2Mzc0MjMxODM4MDUiDDUIjI0Glz0kEMMJ7ircA2FqjQ%2Fw0wCMjaRN9q2krv%2Bgi2XGerCYzt3Onh2jVyV3X298bOE4IgIyyvnd1b7cfdTVZ9SKonIxvgLIYGv9FV0cVeleHXKPpZHmDenkqaPLPfknSwTYAeeDBcY1lDKLgERImVV91cG70lGYKJdR%2FeSthRxLypdveUxGqTNsrEpJd%2F2pNCOOatPR%2BzcLtyPhzkqbq3pHMniygYBCutFe8%2BjplsNSXjLEr7uX1IC7qydb%2FU249Kr7lrXwdxXK4smN1pDyb%2B55jruMR%2FXcOMFEwHoCrQLcl6ic%2BlX606kJ6LqPKmpoxmrLV8Ll12sBtcI%2BdswIbaAL9v1d63vbgf1YaNmBT%2BB5JlJF7VWXT8C0qaFhYz2%2F25pg8ZCUVY4nVcILIxjiYSUai0%2FJrjFVQybHlDxVju8cSmk3sT0FFXm6W30tReOqCZxhfuw3AQ0vSDfaTbUrR2VrILOvIjmqwA4eOB%2Bz9qDFoiL2xSsXngPT5wg%2Fs0pn%2FAWv1T6DKswmcF6VR8zEkE9Tre%2Fm8mKYz14hw7CQ7CRNS3tFUam3zzpH%2BLQ1tOT4A12dvtIXtk%2BoWBSSNKz9abbc%2BS7NxJyR0Cy0FCNACkjMbx0VC7MaQBbVlm6O6toru3wPumI7C6RxMKiy9cYGOqUBG4MUqYUP2iwdsjnFD9MXZ8EiYfg5yTgOvPDYdMhrZoHGkL1HAi215TKmN%2BM6wUOkD6RytYrqGhwVQo1GjPXE91X6vwF3QmjyhULFajtxLfmG7ImhJL8dX4IpuPu4k7YZtGZ3GmPm8bszjZ1oxLgIm%2FpJednISElfcEFtug0p%2BGzr9Da%2BYGz6RLPUGH1IGF4OGmoAlJFjkQ%2FXtvsjQPKbIHM89TUa&X-Amz-Signature=ae34443bba78428eb260bf94698d435a3ba0c45e6cff833943c4901da0b89d40&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
