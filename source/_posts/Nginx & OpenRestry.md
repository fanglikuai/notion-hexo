---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V6QPC5U2%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T020041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBgaCXVzLXdlc3QtMiJHMEUCIEMee20%2BkUOAjnF2ZX2wqstMQkFXx5B3f0INWu0NYZm7AiEAlNQdSAYQTtlA6W0AKZrSMjkeJypt2cZNlwHsxAIw7DIqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDh716Qm2NJOhrvXZSrcA9DpQ8vxuZliumuzzFDknKeq%2Bv2Ul%2BeWr0dcKyatLJIWxBBwoJdYGzCUIxrW1t9f7BpL6Fs55KgaBuGV%2BcDKUJPvpbjS9Jl4eSWgOvFg%2F8%2FwL5xW8KGYv%2FK5dAbTtUR0k2CEreVY%2B1J8liGhGG3lnPA3HhnGYd5XCApN9R3OT7AgSYUQD1HgEyq75%2BUcpPw7%2Bb%2BgRTriMXH5C8%2BZBk9p063PzH914l6qFpgghmpPlD07W6%2BIUL8rMpRNnJeyygpbOhjEkI%2BQxgSaJmaApXC0lrviglhetmXzx2npMJ1248MkNVAhtg%2FMM03ZHr5u4VKLR4FxvVNaXsLpFJMDxi7tWliM37G%2BxpNMfFVhCq9MF4%2BC21bKbinGBqc9tjhnwQv67RCisZ3qrSQNKKiB3bclHtvkEdZ39KR6SYDdUtcPTgbQtz1nDBx0m%2Fl3%2B0UkYriGNuD8X6pZsO1yqaOGs8MVpsztUxRZIHBOSrhOD075Vwo1W%2Bouy%2FL7HDa5ZT08N2NMRHQAlvLwD6erL217kgldbFapBWluUxcIeWD95DX8uC%2BT4a%2Bc%2BAMAOm%2F8ua7YLaM7jRondfoQD765zwYrWdDNAca7BuGNXZVlPJO7lTSOJhhQEqFZPZwmznt87Jx5MNmmv8gGOqUB9rTIWn404T1CiZLJzbDoGD2tUF0KZEDjO6hK%2BwRaf%2BeXDKxyLbVe2%2BmuDRXkZT%2B%2B4POUsT%2Fo9KnRJW0e2HGbeQbXI7xyiBH8VhfFWT0MC1spK37z4tYRxKPK3XWpvCkpGypIQGQVgHVSWN%2FaXlSW0E%2Bt60htcIs5pplTalZCFMwdrTrousFLBfQebYe9zJgNoZWZOxO8QE1p5jC57gUEi5FpDXUB&X-Amz-Signature=e0602eeceddd3ccc07e3fbd934d8dd7294e0a1d8f26d351082b3e5491169af19&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
