---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDPTS52O%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJIMEYCIQCQH8UI%2BwiI%2FLrjk4SPxZH9CFoGhpiRMepE9hZHjzp6jgIhAJgQjxzrscF%2FH011Aw7imrElpZfAsh2gzbw4KLCtzwOcKogECPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwKOT9I9WSbTks%2F2CMq3AMZuAH0ypYQohYh1iLskM%2FfUdtxq2qurSa1gH22B3%2BvccuWGHdxMw6jqEBYlA5h2xAUZMSvN%2F91TTwGrpkJ2oeWRrGpTsBT2qJwHmvC5Tq7%2B2tPrOq8J6KNQ3sSQhqfcpv0ILTHu2GlEt0yII8yPx7nsfDM6XjJdfsvwOWg%2BQchcCwsTIOykx2XbJ1vlOfsBiEkivqR21ikn0GOOfKliKjcNgtEkCJn9AEBPCzfBAFDMkXN0uqiVfYUtduQVRKcE5JSItIIRMhAWk6V8r%2BjK9Pe%2B8HQaIrzCl0YRL1lHFG7gKb%2FF9%2BXvtkfM%2Bwz%2BmTqnbf%2F%2BoVshQvq6yTLqIJhSDLZLYvAPl4sDKGAcRX0jN2NXpDCZuPm7rQjGVxd%2F18mYYmwJBS13njqxR2jHaGkog8eXzzf%2FjSQ6qe9smq3SzSOlj3xNKxqF2s4LWGBaUXCYGKgbFyIFoMZI5mHDVQKufVj%2FFYfPJuzVRyVYYh%2BkJoqo44zd59T8UdSawAoGk8LuQXP%2BOKof66Z4Fl3JYvGBm7mT3qCZR0dPBQ4XyFZoG2Y0v79Wn7fubtO0BkQX1LiO5%2Bs%2BVOR6KBVXbg2bjDJhGbYGzPDBzTx9ahjHS%2BIDFGhkiWncw0GwPUjkWD26zCIvqXHBjqkAbtZW6oc3F08ArouNhrPda8w7um8kZ4hWWfnmpAl5MrPRuPzAi06zqi697k9zgZctIizWUtWekrWNaf3Tnyio3ZuPVibtS1RJmoJHaAAdmyKGbmKsRW8ATWfCWsnliJ4SEDhL%2BmLQpVZkQxPVw3kqlNZlUfJjzCIEiKl5zn%2FiZM693Xpxkj2CbR%2BAJ4yNidx1WP6h7sOt1e8zxk0sUDMjufkaMhC&X-Amz-Signature=324d6097a24b8522efe1f3c55e8191f3ee2e75977c6fb3f3600ed84910e05780&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
