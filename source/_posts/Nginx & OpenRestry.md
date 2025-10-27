---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTZI2U5Z%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T160045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDQc3dXl1qo%2Byij7hFBcJ7JrK54vENQeknUBD8AtZqPxgIhAKnFcGDon43SSum3rdhIIzVjq4I5O%2BlybestnPqfuHQFKogECKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igysn0Kq6HFMZe1eWM8q3AMcyOZoG8m%2BpEsYCIIHsRZmEVTrpze6oFeDluIJ%2B2msJ02V5CfwEpqJ2xa9gQKc1AZn%2BIpG8Kmu%2FQWsuVd0gQKloxESyCeqQ171CljmpPH5wtjUcrCoHkUim%2FiVaH2PHZjWFrkFxhXrCCu0z6pyyY6kMSS5EKT2flZYUPW3zx2z7mfYSwePRR1PcfC8rHmyOezRoXkQ91vqvfS2%2FLWfuJTrs%2FITWmumC6eIAmioVlZCkFshlpr7eHvXCmKXBDs%2FjtHy59BXUpkwR5Vy7ExZu5gikhwNINn241T6KZfrzDh%2F7AhzCdWOsfW9tTzmSs%2FcbrIF7qIoY3WyJIyEeIYrqaeE%2B6ZXyi9tASNHi50RX7FOCstWaqhTAGS%2FUjS%2FN74v7xK%2Bbndn4gcxzAtnnXoVF%2BJUDsZWUTW8QfrtCJAzKlrthfkeXGc9O3k5BLC%2B0oK4WZ8sn4stlTlsr29wS0xD%2BW9xhBf%2Blts9QRlED6wZatDrTFwKqVfJ23YGUGsez46hkld4kyYB0EuQE3bQOIOBzMDp9MVS91%2FMXyhSYhUUFoOJLUflTw4gaGeYaa01ldf0sgizm1BSUuhDTTv7eK3xC9dz5xR9ru2i6vL6L6DA4QgVoOoPD8vfHwifNIvzIzC2ov7HBjqkAe7H2g51VpZEXEK9kU3u1ldlCkuEt9RSVCAxV%2Bh%2BYSCJMYGKE8pTadB6aiznor4i3ABubMdhY2%2BLs2iD7r%2BfjbB7GuNquiDUdT9x4jsXiYv27IZtQwpoMyrka2D7Rv2XO7TPePId0lT8WUL9n0CcwW3gqAFRSojTiyQnMyUBEeSL5uwK0A3%2FftOOQ2KHo530Xyz0SbbaI5G5Jx%2F30rRUuYIkUR4c&X-Amz-Signature=69cf90ada0d4a01d73da57956f169ee90076704144b1df39c19c4df6035a0c3b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
