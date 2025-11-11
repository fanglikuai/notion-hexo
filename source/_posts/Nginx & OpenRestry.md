---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VTZXPQNU%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFEaCXVzLXdlc3QtMiJGMEQCID6gV2aM1XhHhl6XSZWrFfRSUnDuYZAtUZmcz1zWqYaeAiA4eJrQ97YqXvAHvyOqjdO5%2F06osWhFIhAxEA%2FdgS00eyr%2FAwgaEAAaDDYzNzQyMzE4MzgwNSIMCbpcEP7xLjhpiuSSKtwDhwW%2FK13ZTqdHNG90ZgJDFEziboMPJnYxEZD0eZhGmSPTVdZWAfa2EkLe6%2BiebDTqSQgfKW7U7Yi%2FIXSolH0lDnPYgrh39swklok1L%2FUanaVbP7RluPFQ%2FX9Ywz53EHSYYQ4M9mfZxF8MMUkpUMR8n6xdAJI%2BuKRQr5lKQsHYv4HBNYlfSak0XjHoScIGlEo2KPUttsYE9MXy9fZ10CjVUTglTr7XwWbsaywx%2Fk4iuXsT%2BfOqZ3ZhJrrPXtAacNzQ4h5S1l1ck2ievrtzjZ9D7qRsG1Tg5n5Y4kjl26TDPuhtVuEaxxiTg1DvelkpBz9j3%2FocVOIxaDY%2Fln%2BAvIBbt3THY9bvcZTMq36wD06CTddkVCm%2BQn%2BY6XcUIX1u3e5EEo5N07IkCaggb%2Bl0ywB8%2F7%2Fb8%2B2HJm1MCohvTuIEjxG2ZxOQU%2Bqfl9HCVl6icPLpXh9scowKcwHECMOi8tkrgeM7ont2BNViHGQRmStTNUGlVbKZIZsdFRE8SD4bEmu0tUcNzufHIQOok0kqy0XZ10fspgq5Gx9S8uNBFnw91rkohWxve68jgFZePlYvQHwFTYefFmwuN0n9uLB1cU%2BcXPXBlf%2F6QIjxM9YQo2E%2FViYekCoSz92edkP4t20wv%2FvLyAY6pgEvFfA9EGTeozW%2FP%2F4Ojg17tIVq1XbkSvas2bhSCrG2jCwV%2FrNxxUS0d%2FXIgdWHzHe5VoMTMA8IYTdbEEMKKTq%2BU8Mi7wzc0cLVE05rLHjTzFmnYSHxEGKK7bE7OSlRctJLz4xo3DCCx%2BkeSItF8irGV30MaPIy4TXIY7ieFFagWYQfnfepEx%2F0q1JIuF1BEkV9S%2BW5QnGqJ8HPdr2IQeEdfHvSnSKw&X-Amz-Signature=007749a54020b6b869ffcb7027e27aec7d5711c5b30dd746cb75127ec0e0ad30&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
