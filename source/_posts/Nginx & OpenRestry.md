---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QET55U76%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGUaCXVzLXdlc3QtMiJHMEUCIAP9YHmjPrM0RSYqtrQbvfEzUgbvHXiufTEi%2BK1rSNxLAiEArtBCrtRlWYWY%2FZf0x9%2F2QYaYYsiYlkdnm8gWBy9AowEq%2FwMILhAAGgw2Mzc0MjMxODM4MDUiDEPr6CEaZZm1pu0zdSrcA9x2IZRLEHjYRELp%2FyV6Ax0tJxFbISG5LgWABcNzj1zbd%2FaBJTmR1a64BeB5r1STCPavx%2FotYMKSGJMtl%2F1xEGc8RpBNa39xlHgDMIvjDL%2BHpuVsv5uSD2RW1IT1rsHJe0KqyjLyJMo2tyg0Ry6be4iC7QEa6iT9WLGByEgWkCyvzwQoQdpFH8hzIoTI8lGeqpPihsWaUcxqRvnc6EXGM9Aieidv2FXyGxWlBju4GYdrkOQWJt%2F9foFjA2XCiZ4i%2Fvy546C%2F%2FrDDJMDotXTaINT5KwHj9alFWk8cdfQtc9xLQomZoUFErPEs44pF%2FujskRFqNJG3lA0OTndi5fxkMEvbuNu2wkv7xbmzHkUNU%2FI95tJ7ZtWoSIIk9Ts2Yx%2B%2FRWxiJF%2BoouDH9h05ONWh2n6q1A1rVVOogBJqMmL9EwquK7OJTB61FvHmrH76TVfi3MUdllXU6Doe4Y5asMVGAHF47t%2FNR4hEJG0%2By%2FR1vJSASr926J4ICXmAsKoB97sNqudEtHgSmoe6Vmq3jCdiv7JWRS%2FBh7DDdmR3Xk%2F0wZUi6kk6eIkMy%2F1favH4w17rm2WvZpGXUb9Fl8Bt9oahsRbM%2FYYmqyWXpGkx6i7YiWMWdifC6vThNfcYX9%2BHMK2b0MgGOqUBm5meHvdXPEQFeHYUz8cIB8PGn7nD7%2F7AQuhmSl0uhCjs1Nu0WVQ2q1ghypjT3cqHqAZqLsX0aUlYrZPUt8nUuzHUOcAT2QmIgI3Q7%2BDdar1%2FLRjf61Ebwleh720uuGVf9pTJ1bwjy4fiRKkAfSPVJttruHOZ6HkZVfztQ6%2BR3WXW1ijOCPzhUniBgGuEP0qau7J5L7Go%2FFl0muXBOmYNOe3cmBd6&X-Amz-Signature=5add445ac2ea0edea5cfe42131ad34bd1f196ae4b0285a16dc9229769db4a11d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
