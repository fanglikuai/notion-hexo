---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DZ3CB3S%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAEaCXVzLXdlc3QtMiJIMEYCIQCWspmqhWkhP0%2Be0DAHatLD7D4tFebOC4o%2FL9vlZA3JyQIhALYJCY8a%2Bx1Vh1Ff2blB12q379hQzgyFr8XJqcQZLrviKogECLr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyvZzlPdE8q4VDyOcoq3ANpXmL0yicAU42d0T6SZ7FLclvfEpADU3sXZ5%2FrIZ%2BA3k6%2FpMDS9fC6VhyxqLGnEjBXK4VOsHtI2ayfIS4mwahLRWhRWVhUdRq%2FShT1N3wpJotDBfmT%2FhGMbhkHMtVqs1YpM5QzLBvWzA7xdpsSetVqXnCPQyT4CgANtYxxXXfEBFYl5exzxIX6bgKN0p%2B4FYKZKde7noa8BAirxQ2%2Fj3JCyIxkVmIr73Rq5248S7Ub28cC%2FWvXxwuj3iy3A%2B8%2FksquhlHHdb5x5jZHQfEEYm6LgUlwaxSXmwmQoX7Z89s7CNrrhEosw3IZwqPSHc1PrugHzJVUd2F11psDGhiXB8Ek6RaBIpa5TUzkqSE4wFYHUwrd0uqk2vHC0t%2BSMROyS5hWtmqRonsAELDnuKrZI%2FlG2ys9nM2ea1smIR8UYAzWsZZng1tRLYSAsWS470GycgxcUFNuDiWjgSzGn%2BOGKgK%2BQTAtfqAN7J2xaVuw5%2Bvrft8Ofo%2Fm1XkoWZwZ3xLADppPXIzrenv3RFvw9Dz0B903USD43gY3bDCLEMXDgePsaOuvNGUxMFJHxLI9v%2FbvVOS%2BXyblh4%2BMocWoQnc8Xs4hEL9KElsIAraggKx8nzhCjaadcGFlmvSE%2FQXnuzCGhYLIBjqkAdMsc%2FwVZcMKMOneLVSG4aCGfa6zHbwnRuC2c3xb1WilLa49bkLKoZAzNP%2FXsw8yV3C662aRSy%2FcQfsZXvBlJ4OPeX9eTQcPIOjfkerM9Gi24agL35Awjm3IvfqAbFlkTfuvnMEhBUBGmQlsPDER3rtcqTMgS60usfkkWPSW%2FLSxd2651CBjhhhBxmQ4beibmlfEYZU1KVQDlK1TAIRrpB9tBvlt&X-Amz-Signature=fd9af9cf3bacb5e86bc1a35219a65f795c0023a009d3167296ceeba4e6593b12&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
