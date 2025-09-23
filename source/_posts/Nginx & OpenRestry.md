---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46636CYFHIR%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCQlzIONxeeL6udL%2FQ%2F4q2cFLp8FnW4oG1I7hpb7vtKpQIhAPyq5Z9bUkcPHW1yr5hCieMaAv%2BiTwTF%2BOJ1irvyGsRbKv8DCEcQABoMNjM3NDIzMTgzODA1IgwayVUzxmy0PURuM6Eq3APWW%2FPbKuodvAKelLzQYSWIGCl%2BxfLWVYMwjH9omJSUQ00461OPfbAvxWXZBJQ6Sz4buy%2BCdshMfQJAFpCKtyGWXtIESPr2AIyq7tE%2B2eCPFixt202ARvOgjTnWr44PHPxjkXjWHXepiHTmRe4TGOwo0McMM5YFNi4wUHUsme8%2ByYGqUvk01idPZcEQXZ4MXQy6stFruJayGnMu8zdrDps25COSU1zYBNEhYo0ONe6M0t2kKBTfR73FfZzYesg7ttbSg2iyoqCaPbL39Q%2Bc%2B3athURA2ndElZEEn9Qo5c%2BJWU3hIV8kP7yrbpqZ%2B0NnMlkrzjKEwdJ5atrwrl06e45oE1TIukOG3XNqbmo3biJMD8zwK4Ud49BlXa%2B%2Fh7d%2FZTGaFDwLH%2BKkmFGsWYSSCf2rzQOqcQ4YGJ9amhQoRM1jN1P4PSIfABDvsPCJNtTpVm%2BQRHf%2FC5BReHG1SPNDIVB5yWp0vJdxj5luri4AGBrZKxhSgSMFJ2KknDR1afrwJoE70r7daVI%2BEpFWi%2B71rcRXoSVnb5Ue4XcZCqAGe04WLowAf6vKDxqrSF0uNws49vBKhVjxIwNw3TFDwkcd8GY7dvf7Feo2F8nXAvTZEgf0DbKY5XVwVqYcZX6OAzDD1srGBjqkAf1B%2FiqQ2Ct3CrDkoksGUDy6t5ZbqIersuoQyqazaxSgpipEg8fqqIZbjOH34loGP6SMX8cEQm1NPERprJv2cOJQqQDbR2Kv3nRf8JjlkMQENhW3%2FHuKMBzqUnvJ6SeJV2aAKf5RViBdQxNw6B0Aijah2HAuZaa8cKKbjFTOQzUwTLtL97gdhCKKZZtQGeDXUpAd4uDnSkSL5igjNtqEzI8QjuD0&X-Amz-Signature=77c0f2497a40b431ff334f82aac960ecbaed19c819c0747d66f972009f610c93&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
