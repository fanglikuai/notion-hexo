---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z2Y24KWV%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T210037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDMaCXVzLXdlc3QtMiJIMEYCIQDp%2Bprxy212ttST0uSE3%2BxHKCP4BGVEd3NblYFCaT%2BNJwIhAMREuwygimf8%2B2hMldmqvQiXO3F8LGU3jZFfRsKcpbChKogECNv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzASGo9whaROlly9GIq3AOpu9ZFLs9TNJTHIbhRyRO%2FFY7kCCUZTpn5qqr8aDMSVLrLkEKcb8uprfz5ufefgy8K63ulqfXUI1YmuP1NPFqqKSQ10zEXN3mHfQTFuzwmKPE3L7IAsLMuc%2F1cAS30eFrGcHUnCfaPhE3ydmp0K0mpYBXFRtWxBbFJX7DMANWGoGO4Q1ABvMXU%2BJyb%2FiRSZKkZWGlJdehJS%2F5B0dDSQ1gdy8YvU52FxT5IvVfMYhIQyyIrsIzQUXuoGl3xygQukaxpypgTjnJomxg5UgYdbrBz453iaJXrfsUrJJonfe3t4jpjJj%2F%2FAqY5jiL35PwIOx3vSsphjSqn5K1iqRu0Ab4fho46fXhFXyamI%2BmHjHdfAHo6HyzbEd906Y%2BWv%2BQAC%2FRXobYA297JdTX%2Bxxy4zSH8KccEIiiA6F%2FBx%2BLRbO727tkHBVEFbYxPHqSh54NhOjAhY5yBBCJ6WYHZ%2B1V4wcQvpH%2BX%2BJ8XhQEvAKqJkc20GEhfRs%2FQYYTffyLzyWZRwOJK3ikg06zrT3pW%2B3Tf2ovtLrjA%2BTINvgnBIkvvqneSpFAmhN1dCUpB5Anqp83UsuLPOZQi2B0ZpS7835vPFZiDManWnxrvEX07tSEmZOeT7a3VgoKdRiq8GXf7UjDk1tTHBjqkAWxglhCfUgra36m4fPoVWTr0gB0V7pF8f%2FIALv2ZbgC0lXGez6lNcbrYsaAU5tUn2TiiKy4CAHteg7vZeEGNrd87ayAONb%2Bp6ko%2BNWzWHvF6CB1e%2FRp7Dg3AghReiAVfVOCJa5VqCoq8DswwwLAJSg%2B82N%2BUFu%2FQ8%2FxgfY%2FpXsa4nDJVvKWKICsqR4y4Q%2Fnv3tcUsHw3Jg2OZMEsHBwmAYVtFe3G&X-Amz-Signature=d741c3570ebbeaee5c402fe1b09b609c8b98026de3f403a1da4b3c95e938893b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
