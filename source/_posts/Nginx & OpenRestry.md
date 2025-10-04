---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q2CC2HJE%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T010040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAGCUk4mxHpp77kZBVoT0fgjwQdtRxffIz9fBhMKJQFeAiEAt4idbxng%2B1hl8yMUmvM64t8CZ2u%2FRWZENRouoyi2H6Uq%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDH%2FANPpwinA8N2XPPyrcA2%2F4H0Hz59XCSjh97atvXNFsHE0ndEaQIDHVK2jqlEV%2BDmE4uPxD1fhCM7Ong0s%2FRIXrEgDpXuvJMcaWJi7F5qa7u%2BMKARUZf%2BoV8VqUU2CG8SErOH0Kiqzdvc4csRdsU7l%2Fec06KfmRLK4kNcEBvDjW1hPb%2FSBboJdDYbFYft26iQvx7%2BFMc60scMKiS3mLDwS1hcDBodgR7MKwahiLclwO%2FYdZG5W8earj6luCBdWoh1%2BqBKepSzQcgDtlUysKR0%2Blz4Ir7MqdZtbLKOTFnUTWw2lg8KU9bsjewbLKk2AQnqMqjrQ1bstwO6ZBi9EoJNg7o%2BoFj7AKAp0C7BHyIJlqxB9ELM339dRIW8xinbCWYNIMur53rk9%2BMHttOjKwKSXWtp1Vf0PoJmIw%2F879p1ftjPRgiTA5uBNABG80SlV%2FPS2vaNO2Lg0UbQRJB%2BPwZh3r6EeIBt7uh5G2Wz4Ou%2FAJCL1GfrEzxPcxppfyIa02qvrbo8G3dj%2BKNonCRSI94khahbdk2QX3H%2FzzW0nINEMplERSTj38HPkKrlIkjWMEvXH4hDgkuH9tHJ4eYG9xoPONe0fp%2BZ81o%2FDLZQBz2blFGzs0ifHpGLEVm5I47EO0SFydWMN4GdWDkOBAMOHPgccGOqUBBn%2FU1jJgYkdcWuzQethfm4ZrfI6jGHki4jdPZ6nD74Jw%2FHnfzLYCb%2FsfmfvO6lFF1IMJzTOscKEWVxl09gwxMiO0lGs9IGlSWW3AYae%2FCOYQa01sskc7ePEuZFc2lulMSO5CW2Ah3mysTDPZf18yoocfSsM9Yd2sDTK3iDuLRSs4EZvRHRWjCyf08GrKlw9jIwSB%2BvrY4t0l5tEor6GgVaQ7KcOC&X-Amz-Signature=98020b7b8b4d2d4f1c17f4b4619d7ef82b4e1e6b19d48fc4c1fc2e12808e7827&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
