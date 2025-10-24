---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X4OEJVWD%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T100046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDW3KgvFCQrzyM4LSplE9J00zkl6g4Z%2Fok1hzFPhSBG0wIhAMvklf7qGjYrnxxnERzBJC3C%2FHQBRP4oBe0YbUFaCZdSKv8DCFkQABoMNjM3NDIzMTgzODA1IgywFlEyVgHMNL7pDkcq3ANr%2FhE0FZyU%2BLU4xt2%2BrkEdo9QGY58HLkN5gTwVVCS2j3aZiUNabZiVe%2FRkrg7eNSusSQBl4wB2PVPnHmrUL%2F0nJIKuyT%2B3wwNjYi92DuAGYV3dIH%2Frt2clmcL6tXASkMOhpxiY%2BjYKtes%2FGoWFAN5B%2Bmhqpeq4iXJ3eBeuxGDF9gU5Y%2FODFhsuTz8OLjYzpCgCY%2FOjRB7ng9ZUPfY8IfA%2BsFIrswf7uHtswkCnngGqKGPpXGpB3QuT26YASo6VFSxzLHyQbAl9sVoGboyZQZvyJ6ZEV7TY4m0voWtbeF0lDPBUIULLt2gZxCAdyVHIGOUKBHqGnsCC6pcMI3o7WhrcktaCQ3PZusKuHd4ZuuldEn4s6ZdaNwq98KsKKMMSXbma4dq6y7JfIJol8SirYnZS%2BulLkbgxf3WC%2FIdAXtUXCB6jDn6yp7Kpd6rhyaDTu9KTBWpUyFjQZ6dsmmOpLiWVriOtc%2FbuMp7bujVW6TmyTge7YJS%2B4cV7esjjyKLVRYPI5s5xfVmrZNYQ3%2B1ia%2BHRXN%2BViPSnZd5fnSMD05vGjkVmaK3s8mE5GwXVlxDfNlumZ1Y2IQKs%2BQh4mS7t%2Fbu6uUxfme3X9rWsdtaiVL6Etm77liLQxbaPZawk3DCJ8OzHBjqkAQ9l3jHxMvbT4FHjK%2FrkNaz57Zec2NgyTL0AqhElW5Wy6Ex5JGWFDepuwgA%2BuquZ7ZMCVrTJY4FQLbKntxD4SJxaky5Q7IRG2tFNuLDxI4wDHC5OyPVJ%2Fo9GDvEmwHTgkWViISYekEOwGPFyEE1zTFiWIKsbaKbY8PGydB0aiRH2ep%2By6vDG86BhLH1PJh0ByAuXkWbiU6Ud7PNJEcLq7JvT97rI&X-Amz-Signature=9c0b1f979d514d4ac3c02e6ae65188ae8d367c9c0762bb4865c9b2304add916c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
