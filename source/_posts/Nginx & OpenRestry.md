---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665ISGLUJP%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T140113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCID3X7ZpbSwj%2BjGZw8l3U0iTKGr969CkSDHzM8DXxnH3KAiEA5LbQW0tD%2BCIK80m7ko%2F%2FMtrwHmPWox%2BGgtS74NQrCIYq%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDJJbu2xFSnnsIP9RDyrcAzrh4%2FynrQ%2B%2BciKneLH8nY0UGpO4zt2XgnsC2RodPDf6SHb%2BGjsffk5bXVQZoXsW85KjAMI4CD8ofM5YyvM6dfYvgX4obH1QdG2uJ0ElsXWQaZIGUH%2B%2BII34Nv8egQ%2FWHUKjP6kDBYOTwavTxKpEv7jWoviySIJDFQJjT2QoDmW3dun85pewlpF5Sd4iQtAQWaJQ4QeiDmDvshGFYDQspSyCDRnciwx2VYdGnLPC5AaQts6ZX2bmdKV6gmKPIKt7vI0%2B0BEnAAnFy6OJzGhpjvswMct7XFvmHkxYysbOeXhxzVHzRNJBQFGmztjsj6JK9%2FvfdfYynSIFix0WIa1U7zWrqNsO0mi8YlpQYjowfmEmLzFIAN%2FUpWA4YXeKcm3GzE7zfzS2DBt5W9uOuG0g5FxaDWffP4LgCJluQiWHp0HfmRfkwAo8WojB5r9ZrfDPvNeJ3gEZtpB%2FSQ1iDQDQE74BEjZe%2FJaZTIRkPxrTq25udejKZYsiAgv4dEkslZbEqO0Xdv3a1lNesyqTjZ8veKxanKQXqgqnAxvbvkp0xatMVdXX8ag3S%2FyYTVpWHiDfH1mvTkQZ0HIhYO8y5cg4hJ6%2F0ewwkqN%2FOava1GUXbhMCAqxtxoG6D7w0bWtYMMvD%2BcYGOqUBUbaGqVcms8uuIwBPRYU8ylvegGPy0%2BgwunQh3LAROWx0s63egx2Baxdci6ZiMaryfNVGVhIgpFj5Mfn0vt3wTjLqalbSs0AK9X4mVlgggGNCF7E2tYoN71c%2F1sfCKK8JwEIsHt8HYRiwDktpwZMNXxe5reNmRF0B45Ej9FQGfPhBiqcA5593E9h%2F9A1B1GCDaAlst5w3zEIR53GFoBEkDNoDwvug&X-Amz-Signature=3d4665018cb08d6a68a0686939dfbd3f0f2aa11edeb1d68b89cbb47428da0c12&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
