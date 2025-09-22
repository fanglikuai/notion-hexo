---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666BZAWLBY%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T090050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIF5x3kl9MR0FYFuB%2BcedfeS8ZrbZZng6TFh%2BDnE%2BXtjGAiA7AkPg1CCw4YxVNo1gYHuhPU%2BJDxFdUteIBuNBsX26Tir%2FAwgqEAAaDDYzNzQyMzE4MzgwNSIMmxx3cyrdlePGlOZJKtwDuHI5459Dd%2FAx9ReITF3mtzqMFPwpf%2Bj92wPeHgJbzPzeS4ncDdZQWUikra6ylaJuylXOkGQ4gSuhuHnZosOPJ90i2fzpBfnh5%2Bh3lAwc6uBqH0y0ENFmZLtps1VvHL4OCx1w0QkIsolIhlatbtErt2wi0WqXnQCMuLpgec1iIW5uUOHaghP0T2tzibArE6%2BYGyICyYeg3BtGf6qiSZicBVSsKcYJsrXU%2BgB7fyN5LAQdar5wmRmHb5saNtE%2FZqcJ7VHDGpS2mX4FYmM3%2F8o0w3Gr9RBQ6Q1ODwXukGPZDDFV89tA2M1ZP1gJ8bLFMa%2BxgVJzeLckg0A%2Fcz2FXHEm3DDPvRyEIUrC4D1ZsmPb7ZkOLxFVPhZ73xalYXbJPoeCeVZS8kSWXGUKe0Y4bEF8TU%2FJnJUdwPGw1ZQcFQWDZLRAnhb0AJx4EQwa6XR2AIqyd8oDhtlkQzMkTsiR%2B7R0NlTrBWK6dHysjZiAFI4JlzJ9h6BYTqi6xpGiZcjUyG80OQDfP9vS%2ByrtDIrnE3q0wCahylEwj8izavRhp%2FRVnsaFQJqR5xYhz3rHnOA7TNm%2BIwBfbbhuTtODEuUEeojIh1y6x3X9%2BYBabf0fK1csWBKK5PS4fy2S%2Bmbm8%2BYwqJ7ExgY6pgFDqYDdS28UHv%2BY9%2FShJWjKplHoSxvwAZT%2BfPUuwSRdPErPJPvWNg1rxCjcW5m2glf8carIun6kDGEaYraValpMBBfl3ddbPZMOeYNsDoSxZkq5UzgrQju6mNHVSBpv9umIfV7RR%2B48NS5UBlzd89al6YbQIscWFD5D0wWQnUBqiCuZZXBVKNiaoNVWkRUDM1AXNDU6iVdz8aEqojMLXx7ODZnryScr&X-Amz-Signature=4a1256ffbb07dbc1bb2792858cf79e598db780bf61e78220093d7c8f957c5259&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
