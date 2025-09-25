---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662WQ56GX2%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T130058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDD3xO1IPFgQNirO6B4Kgjg44G6r3IQREebP8418xBA%2FAIgJlyF5qEslEWvb2owFFHN%2BRSfFu4XDrMjKInQ1I3vKBkq%2FwMIdRAAGgw2Mzc0MjMxODM4MDUiDMLq70AcOlHDRtlZcyrcA9fZVZIEnclmdRh56CppcQrb0Pv5Mao%2FVNlbLCdXSnndkHICbuUTDYx%2FRB9BB3NUNftRxw7juMJ763FIOFI4BV8WeYhmogBcDZOVgSturx6bObAywLd0i%2FDr0UA1rl%2FaAQaF%2F00EFm%2Be5pvyGsAIwZERBVgSsKgiInB2jk8CAny%2Bzhf9SMURC8uk0iXbzVB8NGEOn%2Bv8tzeXp1ZUbbdKLdZPiVBQwJopjFS2jMzWyxSIdK%2F%2Fb0YOqT8nJGx99PsSCqKFe%2FQhGy%2FSRqr4PrJ%2BwFiuUAzje6WD6WPrm7phF0fslynZOK6bmCYMZe4AZcUDxZFNtZrjOmpzjlCsHFbhzbgG1LicTWkLQLy70jH9FIyFf51pWfGXfsZ8G8SQ4p%2BOIMvakGxnvybT2hL6GFywsLU4r3VA0hr9ybvL3PeYypQ5yh7azDVQHKjQOEgWWqJJW%2BngsRM5RfpQ%2FaBBhUqOhRmMmrjGtqi1XFuJQWi1SV7qXuvoiGtkLGz7Z4%2Fqvs9%2ByUAjN1AGFDlHcOEhaeFdDHS9E523BzX%2FMGsUm66nGv3e%2B%2FJjxMKtsbSQUvbqC8Ez8bj%2Fy8qstBMgHD2kD61pQCLO%2BJNnJZS4IsbBbQwupayFblQpevnN1E1pmfx%2FMNHg1MYGOqUBmWaGEgbhbw2toxpVRH%2Fm3RPAp3gR3CEFjjWbOtVmzQsvmKXaCGg%2F8R2Cw0fixh6Go5fBGIWHkEENu44bwHh5SB07s4qp0dCyc5mvsMwhk6DC5e2eq5SbOivTk8zCyZpKiuXv7DoqN%2BJVXjvJxo7ayoOso%2BUCAavQt%2BfjBniTjWrS%2F2F05AapytOvfGd6%2BmlGLBOAsSWaAcxAje7WDCtmTeTTE762&X-Amz-Signature=fb1162dc52e27dc76e85ba962a1746b8e04b5cd393c35817aea80bd91a61c3fa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
