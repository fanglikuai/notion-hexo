---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KSBXMOL%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJHMEUCIQDIYQXtSg%2Bi7I0%2FDdUjzZ5UsY%2BkX3%2Fdxnpv2HSoOP2sggIgM83LpJluZEnVVZO2a2uHVvrXtxF9hlxQgqVHH1%2FWeQwqiAQI1v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGOxqhFZLhmOUHmilircA4qbCLVx5Ne5RNDCijyKtI6ybM%2FcIwm0kWb874z7G8ByRdVA75R2lojcqQ5gB8fyp3%2B1jeaDh770em2YOHUExXkAG%2BcV8LMsDiWvh2qHjXqfGYhYeI1RScrOBGofdAn8TAy1OsmbJCC3P19Nj9PPPRd29c%2BLmWmKFDwb6HcQmgDFHjkr8jMwj74Vp9Mz8vtgDOTLd8q%2BFiCIJ892W4LP8j%2FVOzRB%2B%2FIHUJHjQow68fkUN9EX44zTiSYJgTas5i8IhXS%2F5pEy8%2BTXM4PpFgOFqBntg2JBYJL3z6LprmZS8WMMaMY2VhwmfNW6BMbsCLAp8Fh7VYSfzsKUVLdyXSporelGyC4Nc5kdKPd9SOCXDzAP0TlNjL8unmjNKAw8aIQHsZntFmlnUxNO37u5YHQGbLb1hqKWPeuUFU4MUsHahyXgq9WArQVIA%2Bp3Ffcdej%2BcCrAgZMtaxlZY8x0BAQIB1J1Sud3P5fA1uGfKPoM2%2FQZmKng59EaPhNC8lNkpcyUVwVHtP%2Bz%2BKJZkUtoJui4c%2FJyBST3iAAhHbE%2Fd7r0s4P5%2FmDGxImJfeVWyJiDlczS9gqYxF8lh23SLdUi4pX6uohbfz7ApbvZD768VX3i%2B3k6PNRHaFOPQzSKQvnL2MPaV9cgGOqUBsgCVYjKDH2i1FhiMmdKqQXyftKo%2BICQhcc3SffeXzdVAL5WOzusye8SPHEeO%2BUxV4xAByfiRmx%2Bbe8xFCAekakEdJS5jJeMJWRKXOlhjTfFXtUUm2yRDxNX3VrRYZAEyGDzE1ypYG1IzSPUK3MiSbAhvJ349Tabsl8OlVd1PDj0sosmy0dgyosquJHF3I2lSYTUKLpvEZWQaB%2FKgGY2qJxTBa%2BBR&X-Amz-Signature=103be551ada6e88e5adbf420903c2e77a3fe23378e8f6051e26973c2f4afba45&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
