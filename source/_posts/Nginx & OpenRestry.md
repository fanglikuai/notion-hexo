---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46675XC55U6%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T030114Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIAYppzUCUPlwa9idFGdoi9oUUryUmh0bC8SMA801oOVEAiBtsg1dhbiyXBXL%2BExIRR5cvSqtZAYpQGy1jgLk5lYWGyqIBAjL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjtWg%2Ba4oFBSXky6iKtwDIF%2F4aB3S8rzHiSNFaYmE86ruB4YfzerYaRGR1Bl6AM%2BsYPfgR4iBQphljWMxiA0HJm5a%2FxAZRUUjr4MqPuFdqWor1giR2QMwbUudKvKea4QoIIDR9q2QbWBucxexm9yUJ8E%2F%2B2EIrOYgwqqj44hyphHAPCILAH6GcaQgk3D2xX1QuRg305kD6PwVFVkGkr82G6uaDG%2FJ%2BUyX0L8YxdV05eXb8Vkcy%2BX0bMSJQoRqL1qvr13YR40BhEK1Omwa%2Fj1rH0dolmnu%2F3XcibMQxsvj5twmoj8iOVqmXxxGcHmnpYwho9ppU2OXrZ52Cs5W3h4FhfpyDGBMCPMYOwonJxernVj8NTK8FaYYot9VLN8V8d6nK5g23aHDci2omxhC1PlfQ7X8aUZn%2BFFYqsBLtbYz6C0VcEqvJFO18qX9IvquPboP7IsDpeduTEoh12p1CIxShDOCbO7HhW5iUsUXvZTWs3I7WvAbAhscJO6DOZTbiO%2BaXYhdnyFCx2GoKUnt4vLQxfzuS9xwMFaeE1RP935g0MYH9mG%2FPCKwVeEuGjKpy9zAQM2umuRJy2KKHONjhGcfxNyWDkwVbY3Q7W2CjCckphK4wXn7sggUAGacVeOgeTg9E3%2F6oKUIt0rigLUwhqicxwY6pgGCrXh0T1qgXOsCqxirQ8Bq8AjB9CGylormeiprZ%2B5CeiDVfKq7%2Bo393GXow5V7jM5dXkXMWSBb7PoHbwq5uj8aFz9CSZucJT3DvvU3JiKbVw%2FoJ34sCUbffwmIcgZqDUKWTU3cC5I2whoLZ6Ti0meMXFfuYjLeOYUrc7tDT7xvKtTDjgIiuajG5kwKaNah7Yr3U%2Bmoyv1C5azIhmUmR2%2Bso2p3i1%2BQ&X-Amz-Signature=5a0403b87bad8b3656a83f877531c80fbd44afe6965c79187807d01fcd18b999&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
