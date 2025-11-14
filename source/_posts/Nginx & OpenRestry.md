---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U6LN6RF2%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T190343Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC2rm70cHbEWSbRBqLubQufyqq6J70NdDtI2Y%2F%2BjWrooAIhAPoK5lffkC3WIe%2BDMtdiCGmLRK%2FrHDhKceIpNKMkCBpYKv8DCGwQABoMNjM3NDIzMTgzODA1IgyM6vcpvILk6z%2BpHRsq3AOxOYmDhN68F0AOcKcvNmcyD9pvxtmPRhnz9LRYWCIZnjEyqb69%2BDz5heogEJ%2BZkhb8c8ojBvpbrQKVNR3nLM%2BoaC8%2F1rzckIxj0Mo110CQcFaeYv8p%2FRjTGY5bp7urbPIWAyiMAarxjhBRlU02r3Ff6V5Sse4ulxDaAC54ifAbivsmlDG6ypYGV8%2BMNVXvrJpGXnnPSTLnGYKiqUTEycImb8zpQvlDW6bRplFHT7xeq3nYuoiqtNR6EOUxOG2fjHjDbQ6dDSJ2oCqT7fPzVR%2FCchBX1r5zDWualtDuD6xUaoclnq8Ny9o%2Fyv81WgyZeRMjwbPX8LGe1FfUZpXxalIkQ2pc5azK0JabhmRp6hTGLj%2Fvl97BZGuMq7%2F5cjxXEifj99Ol0LJEbWCdq71tygYU%2BaGJMRPS42MVooP1qHy%2B8FuyhYsGIURLmhDWS9f%2Bg568D1y7Ycmtoj6q8Xqiah05g%2BdotTX%2F6E3WOmz3IcN7XuTdv5Yrja8ubJOtk53uYwvA4Y65L3vS0FcHiNBVIKI9xyVQOyodlP4uAZ1hWCtvRoTsvEHt8v%2B5%2FdNETOROYYMnmzdOmP0%2B%2FFfvBTAxJkdmcKOrTWMoZD2VcMFN6rxV1CGN0m7xWgyRIiXBCjD68N3IBjqkAUrQ60ZUngWNNXnD%2BpD30zkA5RLhQIYKXwEyVrxSKubMOz7A0RqW%2BlMzOURRVzfbH18tl%2F9546Qy%2Btqf6rLQ0SLFQWxAMuPFcxf5SmDmDbf1D8gFXimHWrBtw4mkDYPi%2Fft6%2FphVIb6vJpvetlgl26yfjWN1ow1fU8Kcvd86T4g%2FPDjBy4CrDMM1PxU0k1vO5YFhcckvALzf8oUNp14BIaudxJJc&X-Amz-Signature=4e904f2fdd7ed1714f6163fe1c05e8042e10fe0ff4fdd6d1a71c6c2afa728b11&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
