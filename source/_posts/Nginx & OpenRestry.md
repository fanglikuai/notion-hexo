---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665BXKYUKB%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGkeuUjD5MgFCn19HLS7830QBIB%2BQAkZd9qS5kjGvOCjAiB5tvsfjKRyRhlJpi6p9Vp4Fxj0W4mEJ9IFIjJFE6OCByr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMBJ9PsYOoTFr0umcbKtwD1z3IuyVy%2F9PiPIy4%2BgJ%2FK1%2B%2B%2BtFeKeBslmsQ54%2FMRvgrtvx1YNnONycehAu0JhlyJSbIh6V17jWBveSJYTZt1nfoWEXl%2F3DGpFrAhas2tNL7IHqHNQfOOufkdxb0rGqMa2Pl7koJdXii8RTqvY%2BwsGqVV4zbDSpYFokTYva74qNuLRZSoYZxYcvEzSDQAl1zIndnSCZ%2Fe1gYrb6MEEeVFKovjraMrsGoI932GYBINp7ePBcEvEsGlkMOG%2BGmqidJkOzui6AqA5GwCuYEdA0gv%2FXQjwwGEv3rSj1DaU2TiaBbkjUJtpdnOYwm6vQ0F0q6zZIa%2FOFETquBFXR9uqRhQeJoMeVSplSUKc7%2FAgmW84pBlh0H9BOeX1yP3%2BxKNOF28XbdR2Y4D2h%2FAksC9v9CXzCAm6OnV0ibGP4xMlKmbjmZ%2BvA3egixcO4x1ZtEblG2jdadtI2cxdY3yuWv9wQLx%2BeCue673Z9SUncYk8nkeaaN%2BHoAtEXNN93PTOCiUlOBpm%2B8nWJmvdXS006hL%2BBBbIlkPpSuYUj1d4FUZFE1TRxOk4WSytEU4zelPZYMFoRIdylXjxgl%2FLVM3CZjDTZUhIgvcOjfFaZNwvaQn3SOBvx%2FdrsM%2FzGZCF8QOo8whPKhyAY6pgHkMM6CI5nhrlPpFynDa5w%2FNG2qHElWf5DtUfgmlwZCI9K%2BgLAXZRndGYcNOYNlCS7Nv9xE2PtIrmhPSUPI8NAegddvSPhZ27msEJB5SoDnxW5LQCUC8uYr%2Fg8%2BT79q%2B59OckmC%2Fqr3Wivhe6QLhjvdOcj12qVyHqmgyJSXTzIdDbjP4ic%2BU0fndLuGZq1DCMH9KLgfgxSjKq0cQh3AYMXblIPJQhNt&X-Amz-Signature=fbc0b820e93fb5e54dd4b7d708f864e1427e14af1d56aefdd9baa49bbc61e1a2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
