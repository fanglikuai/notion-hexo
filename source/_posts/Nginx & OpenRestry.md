---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRROBSQ2%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T000040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDyUryK4gDc5ARNDj3udNHucW6%2F44JG0OXxCfmTQNTU1QIhALBRWfeGonFSHtNNBpz3HaJ%2FUzHFDD4jinNyso2s4o9AKv8DCH8QABoMNjM3NDIzMTgzODA1IgyTeaYkUmHWLSg96%2Fwq3ANQ%2FTRio5O%2BETj2lFAvce8oZz1nQ%2BMbiDcQJmbwcI2y0i30Wn7MKyNW9ZxVIS9hVb2Y46G%2B2AO3WOui3G41pZPrkEqIiLsmLFwligIdRb78cD%2BR96EXlFNJIUUA6ZBPORbX76uQVPw8VfAqoU0Lsvjf2mMKYJn10GstzMOInenrQ0EH3H4lpSdjR4r46uMWpP6XifbSIvT8E%2FCZ1aQm3KZrqcsqFgkUnT7jiTxZE0VNqCAEBKRqPJGXb7wnN852QvDdsy6u0zqrP1JXQsKJBA1y6h4Gg26cM92pVjtlAu9p43%2FVY4wGXm%2B45z0WAHqSivYz0luZk76XXY0GutzlhJB59Qv9Ez%2BRJMg4sVcJZlmGwpjqcqvq%2B8BMx2fB%2FOryA%2BiIxHQSAKspNpVr6MZDqUrw3iSEDQ84uM3DbIFOo4dF9IGi%2BeNXjIAY1bszO34Y8JmCHHzsv5p3zKmLWowo8GGdNGzKAUYcz7IsPdk3cOiDTCxo6aTAFFOOVvyeWWBlOem9qY0p%2FLi4D5dUIJX6pJZZXzv4cCPSmKWKY37zx7HsuFjvPArBXjEyYvC5VqGFP8K7z4zbwcA6YyvJm%2BVA4Xov11hv3Ksq68r56nkZmXZg0qF2Sh3sJ1ocjB7ejTDk56nIBjqkAbgSOHOCilpcpGNuL9spQ1j3ca1fHenboxcHm6y1wdZxukOFrI%2FW6fDHndFxe5Tm1UhZUKHY58vrp%2Fg7KVVe0yLbAQvXb9KIKU%2FiPc3OuncmZl6Eih%2FYqUh9NKpmhqQeEE9L24Rbv%2FrhAEslQQUBTnKPzXj2zCjUl28nFaGze81QbvODjAMDx0XzM4FOBdqNtLiO3r9VDG9M4e7TAayGxl0RJjeo&X-Amz-Signature=282c58f1d5eb1b52e6fe422327a17f5524cd0ecbb7e0e445942d21f542988aee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
