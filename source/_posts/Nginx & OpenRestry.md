---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QNXAAFEP%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFNsrUSD8JpPY%2BClehlxQ6QD4i7KH3oA%2Bes%2F44ABwEyyAiA6SCSzr0FpbXpvFyOo%2BLY0MCp7viOaAoMyrqImBqb6lir%2FAwhLEAAaDDYzNzQyMzE4MzgwNSIMuwnqpzGAgq%2FOjTbzKtwDAmVghHAAD2Pf2Y6t2OIV2gdDO0jIqc5y6glgi0bwgY%2FY5SzSe3hGY7dGeCCltnfbLiDoVJLB%2F2WJ5mMyB4%2BBYNVVWesQTJ09sNNLr1kaxODW0j5GAJrzmljdHFqAl11eQ4CJlhmfCXKgLhKdaOurIAGm9spJgM7lV7RsKvgj4sQTCI1k6%2BWQeJwNx9e6Hmi5%2FY5nB8P6K1zUkKE%2BadncVEoMZg85zfBmTGXZhkorhlJN2Mj9UmjqBS2d6kZoC%2BpSfzxyDn8RgyIwhoTlDw8dyH54nF5hHKIMh0Zh5gABfZ99WxP3h2LwS8brQWkKUTwg%2Bc2YRQ0nVaN7T6B6d%2BZwEpEJIcQ2rTZepiYoyMy1bDqOzNDnYVq4i5955%2BWLk6tCBkJYhOQ9pStCBfxdtZ%2BHQ6r2lLrTnG%2BVlW0dmG%2BBybGtWYD4%2FkkFlDuYyTfz3jbjlAWHlU6C3DimMbBkYLkseSPI0Rt5v3p9etH4iVWPTRUxPWlsHgWBAIcTMhmLSBLsX9iEY7kJP5lzROXaleOQA9ZRyDfKeXY6lY37vtlyWy8%2Frnw%2FWB5Hyj%2BHthiSp%2Fi55Pufs4oSEF3X6UNXHAEKDoIl2OiqDiiMNn%2FiFmK0hoeOnbJbnu0sKuxf3tYw%2BpeAxwY6pgEDTMSHWpxLlT0UyCBmXUhoToFCyulMZ3rr3lH%2FOTw9pf8snoI6R092Pd%2BZHkapO2zHCsBI7HkcYAQPwlAgSx739EW%2FjT8bxQU3fEABLC%2BSSu%2BduaGR1TmveEcTBIwZcO%2BuiivzFhQsG%2F2oUpgbMQ%2B6YO4W2EV%2BF5Ugx0fuwsbK1g42nV1vHs1OMXkclwCfZzmJn8wkevJwIhjLFwyNgPA35ZA7iyxb&X-Amz-Signature=51064848556e546634ace32426b11c77db30e1366c4e5a2edc0b013bf3fe4662&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
