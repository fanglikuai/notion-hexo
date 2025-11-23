---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QWY3V3DD%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T100050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJIMEYCIQCRFXds8GfZS9MmL9utClFxYWTDqJW1DoH4m0dnB3pc1AIhALVWLs5qmrfzVvGYkvXzyxgqDseXWf2raTl6kc76DZTIKv8DCDoQABoMNjM3NDIzMTgzODA1IgyNQBTg9lk9oFCmXboq3AMeAqqUlUVWbms29J3ww8OYvNsQoD%2BsDyfHNcoEY4%2BGmggGZbJWanqaBO10wmQcP5Xs0HNf4RGBlPORsmNjG4teBTwLgj6UX9uMi7sZG6WZE%2Fx7dPNN8C3QvpaIJaQeklURxtd8rC7ICitGX8O2fxzgfOFKN%2BomCLkqYU86hIwJ2OdjbkKd9FpjJnHJJ3XLJNXALTzzc%2B3O4cm2xbkZ%2BzfydPftkZTmNPpevHEA2hottQ6aRb7k4unr8YwHz2bfh%2BFfJRi3hHKApiz6%2BqG%2FvCJ3Wa%2BpJhXOJz4rlhY01e02OaUWn3Kc4B5OtNZKgm02doqc7qw4PasEJpWsfs7iWDdFtDzAROCEhyRSR0jI5BpP3hneVeMkEbE8mRG2f313Qk80SSFiehwLtmG1witgbfhvi5MTL42m%2Fko1%2FCR3zIuTClbaDMB6BuVfFsHS1jljvfTbzI5l2h%2BNtPF%2BC%2Fr53Cd3StP%2Bz2WN%2Fi2Dwc6KDQHETArnUqSgl7x5J56meGoc%2FnpZdmje5v1P98MYzn5Xl6LgfWDcIFwPWa3f%2BeBoYk82PAnPXXFovaSrQRtDI%2BUK7tPehAahSlUwB8t7oKcQPj5fXvYS7waoUOngA%2BhOuvXw080p8QqxtIugrIq96DDol4vJBjqkAWoDBB0pAN%2FdIRnCCfFJAVxoMKwwbj15plngBYhY2%2F1nwunxiCsAlq49PJH5JxihGabPhp73rBEG1bAy5qTl7P0Ut3GYXCyT4nJE1qVX3F3OA4NJpzJGtwAF6nGp1kzkVVAdJq3Yv1XDn62MholiPhV75UIrKVAt13wEpqkODtsBNRtUz0P%2FQEO7triECE9v1fI09NHnuS0cwYc8dmz7MuXrtTOy&X-Amz-Signature=aa49e3747dc5579a39d108687ed7b9d895f5b6188aa0877a35d83dbea1097401&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
