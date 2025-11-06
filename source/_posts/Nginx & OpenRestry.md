---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZFZJJXD2%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDFWXza5ndkioXJ4DG7l2v5XSB4Tr%2FAzQDPWzkZPgi1cQIhANnEPKCANEv0UtJ6fbM4t4StOtUrZkM1wlKfD99BWy1DKogECKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx5igE7wqLrr8I4JMUq3AOFGRpXua%2Bn%2FqzwSLjmBujwAEix%2FWC3MAeBhUNOHw5n1riiVzqGlhxoRNIn7nqfCaNDkylzlz9KC8h7CRFk%2F3%2Fuqzh7yM9A%2FQ1%2BbjYrftIoduYeV%2FYsWQJZW5e09Id%2BEMN6Nf0%2F7gieICKEdFWOYJkzry0oXkYRgzVgDmhkEEDuNNcFDAeAYZ0OT1wcTb%2BK%2FQeOiHp3vMVs9KfWVqXCdaD2iwfHfcosMMhSmwxJ%2F2aag5Z1isVTxxNI2StlEN8HxUzAMbtrFNsLNb5N8CXKadyI%2Fjb%2BinsEyZJkItuB0C6uSekyzI56C3KrZuK0eHGnNWiDsVWBE%2BVj%2FmWVU6%2BR96IX%2Fk8pIuEA7QtG8hFHAr1pdUJhWgiASTHJ19vcHT3LQtjFtQLy3n40n2zVn6DPyhUOCAfd4W6wq67ty%2Fr7aF9mxD8WzY8Xmd4vUAmYnJG6%2FyHP8PvlMh0F4fQnKXjwPf6A%2BN7FO%2BIfZW8iHHP1V5sFe2cOv8EzeCSqatTLTqK0oFbf9pED%2Fi64L6SV4KeD4qZL3wdqNmgF3a6%2B4mbSxrgDnhkvYgfMEQwX5PWYMrzJZjJ%2FdNfURRNGNOlVrsXiHlMqAbtjTkOIm1YQmF5sEJhdx26d4u8OPgturlUgADCTwrHIBjqkAe0M8BKxJElJ%2BFkhT8DNcQPip1KlDLSy41ru%2Fnc1yb2fOZEZKtKtOYxOZRq9zA8OEBhBqpO1yWqVh79SeAwBwzTFAfIjxpLbsVALiQ1%2F4881g3gzzrQK5zjykQt7XCKUxSrrbdMkhF5h0%2FztROA5UeUmw%2B9JlX6nwX80DiGyMuWrWW8xDJxt9Dl%2B%2Bn0Dc3K4idK2Q27XqFKN5dSGcA8Xo6N%2BMMej&X-Amz-Signature=3a473cebf680b46866e59dfc13be12bac441a579c0ecf573ff6698f83a973144&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
