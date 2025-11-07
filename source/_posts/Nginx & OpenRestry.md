---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T6MGAQGQ%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T210046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGKtb5CB4DyJIyqmzYPeacpoSMqGfDEoGR7Y0VOGaColAiBClw9UnFf0Bum71LSKQLdO2xxjtO%2Fv5rA9%2B2wnfFykjyqIBAjG%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMpDAWbqap%2FbhO7yAJKtwDIha9AAul2Dt3OmO8BukA%2Fl0eFvPaX6wvpvAmdCQL1sj1xfRtoFxEEkZJ%2FLsi2e%2BqIZnmj2GoJKjamu79HgPH5l8l9dm08WgwGtBnJv6YIeM1EwXTLuC54sPzq0eDugXZYeUodRLXUqDe0DPzuGMV8I4Gz6WgrsfOpRRYdm1FQent%2BEH7mQOxW6GfJCl5%2FkY7rlvHXi8CgSXV68Z%2Bk5QUBxUCbBtM5qYX%2FhZ9m0JixhB9dYRN1VVO%2B0f7g%2BMzqT3LmtFRlaZt4pcl5j1hdtN9Y0%2BU1finOJyTHgCwroYC1rYwGcEHTmqhcJl1jSX4g4NqtdQYGGxsIuyBMO6TC163g2RqVAEwlcKq5E%2FFF4VAtJOtBdKv5P%2BP2tBqWJ%2Fdo3qaPuoq2srflG9vHVj76fCdRDGip04Wyv942X3xAsWwX1VUrRlyfpQHGjrjJ3rwnCNj%2Fn5HDTud4UHlBgvh2F8xoDA1ruDFUPT%2BIbpzcqX8EDPvxnwpdlP8U6qvaK4MVsmKMIqFnJbfUt4YvTv8zGfbVGC2%2Fdec9bViGvu4oYvZbqdMnic03Wp%2FEBgRiDhpDhiI9JqjfphomNVAJN3nsaVrIt9JBBTpknlwT24P49vhoWqRUHPoDmXQ0oxU14YwiLu5yAY6pgHUqcWp0SN6zvehRvRm1YgszeOYMLMxFkQSVLdnZwNyzDdssiv151xSRsqR9FQlejDGgI4Dj0yJ%2BsspkztCxPakBvkXMsj%2FfeR03M6RCgy0V63%2FON86twYFKPP80%2FsmMEgu4qVrRxrO%2FGCwnuV4n1bSPfVtwe95KYz3mUcgiyyznO9QnTvcPScEAwxq4hqvPWxwvAXULqDiy17R13wkDicK7GT4dUYp&X-Amz-Signature=8c64e3079710cf536a842acc263802abd6e41934e4668211b91300e519b02660&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
