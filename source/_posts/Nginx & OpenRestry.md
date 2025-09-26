---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466775QHLDE%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T190040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJIMEYCIQDJ9Eu4LcHqoTO%2FXmQmz0hL7f%2BxfhxvJ%2BInNAy8AAsrGAIhAK5BRP7zysAPiBRLQgUqqIcnQolQ6CWAo3UJIGBdiE%2BqKogECJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxEhubeo53Psev6rgMq3APmBh0qbnXON59gBF9Hy1IC%2BaI7jDbxXQmMiVlHzkH6Voj8XBEHvD%2FpZhEgEaL%2BubeCeDaGZfS9dp78X4erF34m0VNDVCC1zdvWaJugqpog8Caaxns0fAEiFJJBo%2FzVLcInndj2CmZsL0pgqzBsf6rkh%2F0J3PSid90SJtFYNqEIyAziSEtRTDdLuYBZrOBkNMNprI2rwAC4H0WA183wI5Ut4F9soVDxshUV9fXoXjtITpgFI3DfGRW6%2Bl39aKfFNoobd5%2BfpmZynG7i7ZqOPCFQGzmH4P%2BopliT1QkcnaQEejYosDpjEKWQeKlzvSdi047SnYnmV8vfej4AAI2yXd9N9i%2FqQCQoGQ8npUqZRzvnwfT5V3Dc7YQkAu7%2FbT4v%2Fvt6%2FwIZBLtpyIaYsPQks69xORILGEvvxwlocTxGQ52t19EXy9iTCXY1oD6mMPJHHYs1fkAqtz3zEQl%2FnicGw3dHhww0S3dp9VEdirteduGDXe%2FcgyqELzoF87FyQjMEI2LBPBzi8Ylum5wA5wRApSFdtwB75gBJ4VM9npuYugHy%2FZ4F%2BXWDftU2vjiKB20Al2NLsTLIX3K5Uz9XyGh0R6CtQ4V2ycC8cdyyjy1KDQUisIw1rnK6nXfKMfXqwzCuvNvGBjqkASc7V873zFwpLFDuhA1TMFnUPWB4YUzCTso1d9oeYMEZ0wfibax4yHf8qIpBDftJZCcHLtkrtfAmyq7LVaQN5ewubixDqsin4jVa2q0kVytImKIJrkiXiQhL9MC9rI%2F0KVmeEF5%2BwG1WoFKeC6oOIaEkusR3i%2BBWjPKi5S4HADCJRDNmxp4hzO7%2BslBmDBD7BKBAo5ef45Os3Fv9jHviIwz20d%2B4&X-Amz-Signature=9b4e889222f8e87ce7d2b854236801930d5578185005b0de34cecad534dff027&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
