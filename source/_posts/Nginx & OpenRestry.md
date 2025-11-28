---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XKOPYYHO%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T060053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAdJ5TktI3dBxXdWNeqwb6lKAXD3laSskYAf4ZyzGwzcAiB1Hxoh7b1dCGro3zduyl%2FXY4fqt%2BABW5dhWqk3xtH2YCqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxaRsKPOgNvJkIpnuKtwDYnH%2FvA10YvIKrOsFpsH8P94Ev%2FuoLGURGwiOKF6%2FU7fqt31%2BnGiFdSGEviiUnpnzbTtbIa%2FYb6XXSxVbzowgtqrDDnyrFdVgd2zInz%2BcvOuzGvBnvbl3NgBzvI15cx5scpWIQb4NCv8Yyk39wBxkYFT8TrWrz0sQ%2FrW9p02PXwEfqLmIAo2jRtRnAzM%2BM0HlL%2FaSO34yEVWoG1JwwytKeT5YFD%2FCc%2FdsUOoK4SALCqPd0heNfBqvoGTKlN3ab05Xg2ahuVnTMLI2N0GSAzwbMsYhnJx9i1kPjTM3DlA%2FSHkEdXs83NqpDe%2F1UYa5KOEdVv3gMWHR2bCzPtWK1XcXCV0h%2BgSuZiM3vncSVvSR3OHjU3NhxvYUeuiNC%2Fqsif9zcS4aZlHj9uU5myVj9GOAvvKOTmur%2BOSIc7P%2BxNU%2Fo9Z0ioeQRR5vocs%2FefPZWOJ2wozv6go%2FCsMkMcaLkLp1Z4dU8FYWExZgjBONwVKpob56hP4ExZhfrO4YzXpifxUBmy8LFtvgHHSaOAKBO5F6GHIDLxaucglwpbSnZoLNCSiDUcQ4LowAZzZviOGZbTMXL5gZyjQlrb6R7s4hqX1%2FZ0%2BVf59hJ09qmKRuBUjGUnhqGFrBnv%2BMeT5YmGIwi7ukyQY6pgH6yBZvdR59ugreacnIIuFUaK%2BCIsiK%2BtV6HLB1jfxiYJ%2BJ80xNaWt7NOd4CV%2F6%2BGlCNikl6JGg%2BDhKV6Kp%2F8G%2FfS9gDGY6ZjWBlKsetSAQHutGW77XLlSUjHA5RlQ7u0kZO30xWpAMc0uEr7ODTSI0%2BddbnXSQbiqvMwatWKvmLQU5YkHKpxyM1JmfkrfX1NvABZfl9rovQ7zG6Oq%2Bnq3sr28eUrQf&X-Amz-Signature=bd8c749c19108299fb91e71d8f134050c237252c5d5097344ccd4c1d0e00836c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
