---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WLEZLGXR%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIGQZ5ljXW%2Bdky3Ep%2FUbSuugj6i651NzmfyTAg%2FAHOP8DAiEAn1sIQZVYX%2FyWi9EOzr%2BV%2Far%2BS8E58wd0pNcYYR%2BLVFkq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDLwiF7nmbwvlYgb%2F6CrcA4QmNwhr94vesCvo8eT30Fao6rKxel8wtzngpaM%2FckGKDogdtC5HsMGxx8cUtuVMDp2tpLd0Ne%2B0wylF8UF3zbmM4ak2zl7gvOfD9pCM9djMKJxSYC%2FzqHtKGeOM5NhaszVSXYSt73Zo0UFftrLw2OFEjypTuX07GkkCW4tZyh424zJE1PNJoc5qWtl61nsYHmxzc2HghqeE%2FROGMfvIZ5exTqEggbV89AnpO09yjU9qz8rUq6wIUDXQkAIArEqQVYKr6Tksu2kaQS3%2BIS6VzqbdHfHLqTZwtrWFR0HH5OPp0TvH8xpzt9gVUlKbFFG4By%2FRWUmeqpvNagdw%2FXPAgP1eGvtzwwVtxcirVXAcILYHNwATs3xkhdNyzGPWq2CLCcB2lCRVxR%2FPLyQq53xLGZL%2F7CYzQASypecJD8v5R5XyfNBim6nNedAz3UTId%2BdXk7IIYbRDfkUHZyiUpm%2FtR%2B74iv4y2HGZvVSNgb9%2B32Y5jKz4DLeSeuH9E5j2ZvcFvgxz02a3I4Pmf5zdj6lm9O%2Fh9DFxHqQPSDrbhmynhUwvK8NFZYst%2B7Yb8ST5%2Bt1NLRHZjeQPxbi6%2FEANYYSPNOBwB1vJ8io4pKstyUOy6IpoxCKNeXuEirWZnHzPMOKXi8kGOqUBajhWuxZxWKkZWtclmIRE0Z%2FufYaJZMFjsYQFJA57HM%2F6X6%2B2oshTylucjvamjviXPIiFV6aDXlaH1q1GaWQn13%2FUNmoUrmZUp6lF8GtmALCH5FAsXIWuXDW%2F8QcNWQtPiU09Sze8QA%2FV7sSOQDiJw6QOc0kx9qwKG3ZqdF0MFqEeAyecr3WZdzETlVYqWNEp6nw80uHZCvYHkQk65gANwo7HS5N0&X-Amz-Signature=cb97a2ea1e761d57d24cb05aede1ecbf2d1d26214c6183fc910248e9c63b8ad5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
