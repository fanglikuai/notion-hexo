---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666Y663HCM%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T030113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJGMEQCIHP6m6y0zITWUGNm8qiYMse7Wo9sHALNVCaggT%2BJNibzAiBGYdQkT2BITs8iy3%2Boadxu3hsVmelruq8k81bGKDjXFyqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMCRGAHLnk8dYppnI1KtwDgnKXURAONeVdrvE7Sw7hS0wK3FSCms5s39megDizjW0n3dZ329aFVNeHmmKSAgMr0kp2blIxJeWRVKc%2Bln2MfaHTF7oxal2wbf2b8gZE%2FUw6Tt%2FZcxL4pSG2WR2md8L6e%2BS7cK9C33kMNTXa5tm%2B6xXft%2Bxl%2BLzxjGFuj9z%2FIedi4lBPgfq2iGH2LHn4%2Bl3PSCX8Fd2pC8dE%2FS5kCK0uJiaEXq6aR5zePESFnYixHvB2eA0Z56awQwQrI67Ap1NPraDJA9HmkJFi9U7S5i9Ec7TTjJ54kZjzLI4Vrky%2FGGM0uwyHg7I%2B6eVFsGWIH3rYMWczpMAm0gryFeFySOMzvXjh%2Frdaf3EkLDxY9wCfeQW4DcT8cio0dYEW2JZQ3ebNWTLqukhj9KU9u7bz6RBNC4Y6t7p0LWEICRwTQSjbCHowygq%2FVSU2UVs0XTJ2x5XSdjWsSAq3JqI6ARVGQQOJAlVO7YG5Ln8FLUaTVd3XHtGz4WEH4sEKrLajtOOYpiQW7iXL9xx7uyezqfnRmLH2JinYyHisT%2BRxNRVZ3co%2FFiNOSR4K7lZxsxJQESG4BevvQkAUfb%2Bj55Lz9uPiQPQ8bly8EnOUT60%2Blz3VEI%2FkAOpUa9adnkSPPqveDuAw74%2BXxwY6pgF%2B49GBjJhI6n%2B39h63XH11e9UBlMXoPBpHiYDL7faoU0CwaogV4%2FfdnNGhDViu%2FsXAucn1fenYpn5opTgpaHP1QlsJAurYXXKsfvlm%2FFYY5fsVOcDt7SBfMOXqXpd4XMyZ79r8ssfqbx5ErGlLC3BL8t26Lki%2FeoSk3fM1cAptXF5kmf3Pfg04QgSvs5RuarO1wLWUNFOMlygN4VC2rwNdVQlhopzo&X-Amz-Signature=05d383902f16b4147d2789005c6a29fafcba1c5d2c08f10644b051bf1dddd34d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
