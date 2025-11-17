---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RDYADMJQ%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC4TJ7hoDzROwITxw68pUuaTv1lHuGVRXLE6Ga4uWZGTQIgPFjj6cT4mfb8VQkVaD9m6RIKGTki8dSkqr6XUde3IkcqiAQIoP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJLVpwKdf5VkFQj0XCrcA1BDFkpePLRD6y01Iwy8lhpIB9pqkHXWu5OqPcvXOLEGeH6cHqJVJAU0elv%2BXWEyrdIHjYa1x0ppodYEUrM6YKERUWFi7sGhHDestA36xGLAR6SMmXMqgiJmZBLUU71bjI6Y9MNVDUUwxvgmgqnLw907q4Fy9nn%2B0mciPQIIaljArEb00jznCNkewZZ14mWE5Q9ambjTgxGCrB3DDIVyyqCwCzPdhAPggklECW0Z1yyZ4ZFjBqLlRS4sS%2F6KClmFH2XwKQWYUZ0nSloau2JBQ30le2h7km0NMpejzH756V7T9jxcBMubvJIMyOTzwD3J%2Bjf%2BhgwU1ThEkZ5GChIQN8hW3HwDZg7Nee5qKMbs85KjiIRvVtRFnXyqWSpilclV6rxN0SpUQsiSeRL0tI6bxiG4Ky2Q65f69VkJJf1272dTGBwjKAF76g709GfpaRXUBk0qAFucJ0mk%2FGETQClVqtL9kofAtUOelUDWDz1fn39kiC7zZGxxCnKt7%2FyLn0%2BZAR1cvQre%2B1ENcbkJaYDnGUEf1G8%2BHU5qpj3OUCL%2FlLVYrpUrVIiLrmRubSUyesIYndGjfDadyHp3zn5VffG2AMy5qP4S59BKWILoiUOXUNQ9ofBsKL9bmAT6bOITMIiw6cgGOqUBuFoA7ypyfsQmzLRYfOkjy3Uy4qgw269iBZqWa3jBFio5aM87kysiH3AoXX7Bg3jzeAJo9LxtXRcPHhZFJLAPa0tHp31RCN4XPc5XQGxiwnSA2TIeLaZSBweYCdlz8ZcXJpVyw%2Bq3LVlVgc6urGMX4yvEJtRfJXuibV2T8R5x300tTtZIi%2BlC6ZdWIuyRqDzeMKTctLiPy2am3yG1VpQPJcUCE9zX&X-Amz-Signature=ea199782ca0b1e54987d0b43a4f768eab0ead30606fd3cb495a408a193c5434b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
