---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLVVAZ7P%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFcaCXVzLXdlc3QtMiJHMEUCIHONxqwDGG3t39g7rUUdilER1F3QnKB8x%2FC0dzhJW0OgAiEAsc88BM0Iqa9H0uvWmGihcu0bxCq5f72V7hKegPD%2BreMq%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDPJDxbQkdqMasvSUjCrcAxsKyynOp2oGGMtigU78bd5hbXrZzE%2FK8MsOuFTH3Q1077ZM3emzi7pjAv5QgQS5mdVGfkQzciX3c2tARI4P4aTYUlzyRt%2FTbN9KTzGES0BuHLWFFi7v1fkz2sV%2BsZ8MDvdErFHH3i4YCDs3TlNK%2FIkT0eM%2Fw2fdeJISkyR0N4EzN%2FlYKbd%2BxfIgJ2xJZyZPERwrLJssN%2BtfLEwu%2BqJgkjVcnJ8VGA%2Bq1C1vqiIZNNiS3PP8Glhl7ZhtENWSUj5Dd3WkX5UOyy1S2JAr8dRJSAVEPRmQ6fOUi2cIzuPpqUUpb0XAQzYIK%2FT%2Fu0%2FVRXWNS%2BnTVo9xS1UBr8JLkFdoO0KmPRVq35y6f%2FwwL4mcDjnhAbXX7tYy%2FMAe7qTw6lR%2F8eIiPfrz46iPql8DeV73zEGVkD%2FaqcbiYDMGqmuyqej%2FAe9Sv5Xe0Z3AdT558733rQrOUN3quk%2Ffi5B6WctC8NYGLdGACwv0H49doeVrtW02160JeHds4gMQVqR5jiVaNVx%2BZH8c1syslJQwTr%2Fe4Qduj0n5g1MtwbYr7FDE8HO5Pe5OxZGFB6CVt19Bt5aB7MGeIjny0CeNwywKcQEPjAfSGA0FqdXyow34XAGLVKMZ%2BPo%2BkwJkfO33WN9UMLONzcgGOqUB6GPyPT0CZkHr2sFeYpJQPApdY0IszymMeB2g8%2BPYxUK%2F0glBO2njmm6bMdZhpbppJ4boMoTEa3m6Jm0fO%2B0yBRFT%2F%2FCxx5TU2qnCHOqkJlYCg8Zeg%2BovCHUrRJIG%2Bus2KUsG26hOovFplnLHErw36FQD%2BgqaSfnEeDG8w8iYAQTJNVGEzMF1zWVW8KUOwMEycq0yG2pWtTCvo5e5v87riAfxeGIr&X-Amz-Signature=296ea4d3a2023e98236b467783faa1fae034e4f56dc659e8df988f3506a617c0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
