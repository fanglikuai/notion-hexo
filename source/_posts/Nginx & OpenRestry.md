---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PASSHC2%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGaL12%2FQ0Eepd0nNK8FsxAiBJTRWPZ6JEfxHmRqP1aT6AiEA6XVUlbHym0vwXxDBXisSuJFYHnp2WS1SNopw1WNuywAq%2FwMIaxAAGgw2Mzc0MjMxODM4MDUiDKCsujg035IWROuSiSrcAwr12%2FaeVx%2FaR%2BEp1xuSdctW3vej7pwX72O3bXkaPqzhvHAXpnB3e05berOftSzqP9AiLbigB67x8DV%2BVg198XBtWIks%2BwkKhR2QpxRJuf0ZV7MawdP5XM9P9qdkHNOmEc26L%2FAtfYkQOW0zPFSwmMSRL57jGVDmLn1Im4wbSsP5dtH8eZCZqObhvLGE0u8uLIdWT%2F3e7suDjquT7GiWVGJctO1xWKy5UDBBLr%2F%2Fy6PmK7ufZsr31izJYkjtri2CkVw0BEoBn3fUBMw5y5lfI75pQ1T15NpkHrF0D7LMBJRSKeJm2EnyVFfrAwUshqRGSDeFgb%2B0mOsXmmf0pBxtbIc9EX0qr3PgdIMTaNkyNyxy6IccigNYaKCaHEMZnRv68tGmKs6LJsPujOipu5xKeVkeLL9sGdzI8uSYKz59Uq%2BH5uILkKcLJ6mcUT%2F8UTsdm2Cihb0r8a%2F%2BGLx6qtvLU%2FtSadXvKuV8ZC1BJKflFFKM1Wakvd2vFUWAsW0fFNzFCN4hy6b5qwgR7Gtf%2BiMTZ8%2FkaoZBlZIZn%2BrkZaOthnBkZvhnKTOj6qX43s%2FtfUL2XYuLEL1QIjpE1XItywqkIkMGzF6mmI9hsAhgqOv84n0KMa85dbs1zn%2F3HherMMTCpcgGOqUB8KtVEr9j%2BivoXzhlfDrPJumTbSRSxpMPtpQRrRfrszxDVGDucr0kDKPqY%2FLoLBTR49pKR7Ilm68MR5xQ%2BueJqk7czJQC4XahS8BfRn9i7DNcTN36eUaBBfJeCVZo810ay0JG4u%2Bg7pGwT21Uy35nomtqb4Mp8%2B8cfWWMXyfEAk%2FdLbsfSbAE%2BvvNCDCxQRk2UA99wfUnF%2BF8uTsbsf4m%2Fd6iTxzr&X-Amz-Signature=f7d7c5e8a4f2e2070bb16f3557d1c01a96f45debd79d78347ec0157e11619e51&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
