---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RQX46V5D%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T150045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0K%2BLMbkLqnEdUUMIhHGtFfW4iRVnO07dYrFg7vWFGOwIhAM5bQ0iFmeJcLeHp%2FDbluR3IqazDc6qd6xx2z6okwwgHKogECKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyXPYwuhKmoeHTc0Ykq3AMGzo4KVUACrDyTQk9n5DmDEQqDlYAGiassKo6tmIPuiQsI7FCbgLeZePDlyaYz%2BllFnjFsxt3R4lGtPlGIT7LacUxoFCnuhEaipmRp8G%2BJbBoW4Fn0rbtrK0cDWT7zDAYqRWJrjWTfJJPe7UObmPe3UJpvG5dNlPQycA2%2BN1%2Flv%2F65DirxggJloOpCZTu3FV8wTKzFRXs%2FY2OvTfjDacxvdKkVTV5vTIRUBTjdqe2ISnuU7EyT1ixprHoSe6AO7JLQ4hLc3WOlYK7TrEaALeP9tdfYZAtBfV6ru%2B5KqQK4MhzG6Yd9COEJ0YHPoUui85tqiZvz9bfNBpaVFOPJRuBy6OpTrVsp2Gt%2FpayN%2BDiQK6sQx6z%2FmNWphVjUuVwUlQL3QBTTgp3UnVXTxAa8Q%2BJhD87x6hA1P3CX17xhxmtppJfooF3%2Ba1xj8ZjjtGeI38t3liVw0%2B%2FY2eFc1tGSA7XGqyhLufIawjYZqvdJvaN5a9e0zz8Mrin6HcLkvyZ9TsyGDtvTnRE2IWHTpGk3EgF74eStymxN6aDxdgvsthz3bIvozhu%2BFkLhtMRkPBT6wKQ8jLBVatBxfWajCGsf8pG0VwIXuqTiVVuYVLjJrGZ95V777hPzlm6xKwbw3jDi5LLIBjqkAb77p3%2BpqiQ6CLac7ZabAV5vVtplOUUw6XjOQmszdyqnkIHr5C3o8SbIea1EXtQK0M%2BjIH9NmWQxo4iJ3vMfNTmkUTgu24rsYgz5j37aSw575HFWQDXfoFhkP%2FL5maKXZvicEDCa1Kk9c2ZNUqB7Ka2tGVTW5xWHctN%2FsvBzTqrR7wEaDO6WHa2Z9O3Pgs3FrewHIHlGa67z4JwGDp2EJh8A1hBR&X-Amz-Signature=3274aa293a88b150c1a68f91ce3f70b18da9bd9688b954e8efb35add9eb8df4e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
