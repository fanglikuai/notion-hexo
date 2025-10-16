---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SD2PXNBI%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIC%2BtVpCqAvIl6XSv5DfSFyxZCkapfXHfC20%2BTZu0HNlDAiEA8YPaLANk2YiUl27ciqGF%2FqO8zKq60wY%2FpTD4X6rbpOIqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDE8KPXfigN75kCT4YCrcA1HNiE4EzPOE8HRikVzU3YPpk3%2BPl7CQh1LgLLSHNJRPxaY47aNzgYEkUNqW1N9GN4n7mKHBri37bOlnBrToG3tf7CtkoKYWFp6z%2B5bO1sIkyRHWkzjEJ6WQG4tTgVAE%2Fwz0uHBaD2R%2Fh0Ht91PyLVVixNybgweHBjmuJNvDsdO7xgQZja9TPfvyZ5rvu4bb%2FE3F%2FJFFprzyVoxzG7Cb5Z0DXWwj5x4UgMwkAn1Ey2s%2FJVbTCjoY0Y2wWW0PT%2BsS9%2BfhemnYnbQ0AZZKjBZW%2B%2BYiFvSk7%2BSyFJRHAZvgNAKaDWi2nrxOxBhMJ%2Bx6D92cd51uCDTAaBSbxhJbbTqRGAaX6RyvBmuXg6Q6Po5wIknmdVfV1oQQTqOgQ94WP0xpADqqQIPc6L2xDTIQSb7es9LVNnO8pdQZ0XJyVtjHymye%2FvfmvM7Fv0pSVL14XugY6XLmpvirNKt%2BZkTJNrpkobe72bGmHhl%2BjwqB39m0fXO3jXXVIZDROnG%2FJ9tOwQxghraQY1oR0dc46%2F6gjgdtuv822F9o2CnAi%2BJHMc2Kdeo2GEePlK3hrP%2F4XYPgBFZQ%2Fnjn08bw9TnDOnhkb3pvTvl6hnVeBA8Z%2F6YnPN9gD3%2B3ysupCg8s7kDaiht6MMiKxMcGOqUBm6F5aLBV4LBFYJ%2FH1YAMC0ZwfDg%2BAwBOPYaxfwcKpMYEIxLUfCcZLVFqmTz5YNGWKHx6q4C%2FDJj20qTEBuRPZUl9s1KokCXiIPxmAv3rGDTtLD%2BxtocWPt5iEB%2FeVw6elhVkDbTRN04ZRZ02sICOntzgFCWJRwD9lmIYTbhY4PL8cRktqhAX%2BaiQZ0PxfvhjzIQkfs%2FSUl%2B0TTxl5500PWLKNjAO&X-Amz-Signature=d9719f3db31453f1d0f748f25ec628277e9990755edd584720f643c1f1207edb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
