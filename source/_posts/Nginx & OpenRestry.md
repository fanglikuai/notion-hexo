---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46655M3RINL%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJHMEUCIQCxTL8TJxyXf6O9eZzdSiORMr7t97B0u6hm91z2LnQj6gIgJftBNQzLN%2BaKehcb3Y93dg7wzaKC4vSMJZ84l4KQqe4qiAQIrf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOLxfETd0g9i6hlf2SrcA2DLUx4f2%2FhyNhPUSibX3fJZAj%2FWiYcOEMCvckuRDidURLLyUutHGwgclSos7HlwrbxNNIAuJtDKpgutoYEWbBreVzt26ixcHDC88k%2Bmk5XK3iSy13%2Bi9yPQPjgmqt77C0tSBYOVi%2FO8AjJep9%2BsCQo%2FMqz47sWWRTh4HZsO6HpPrAFL2hSDZL9XLO4Q%2FufzxoDOdTOj%2Fo7Dhssz5LsX2lQaaUzEVakYCqj8WvCl0A8R5pz788simeVp6gpDAc7X4qriv%2FF8YvBXRaut1XVZwqkzYjlVN3Tc1QMuAPUTKA1atsMYBf1ZNcd%2BaxVRgJTs4iWhK58yT37X%2F5hayVyKTpRi7tt6mq%2BTxbtRC8wil4i6S%2B4lAkJD%2FFwvdgb60Wkk%2FbkF7jgIrfGb1Xzgv%2FC1PpwLAG2Kax0LZOIx2tLNuOiKsbYdqYVkKO2fSo3n5D7bmmQ7zYJd%2BXLggUYPhkmIpzC6kfbyQJy1OgDI9pGQ3ehkO8A63ewRMajbmZco2eWEr9PW3gh%2BpOso%2BFW6XQC5h%2FeT4YIgNSmxKklQsrxUojd%2FbPbrUHENFLECSZEHcZCYRrvKe50jBgRk6lvO6QhLiSH6z5gddbmvpMKLcd62WAl04cFqVndDwIZJM3BuMP6zyscGOqUB6tYgrZXfc0dNJrwMuGZHRodRU%2Ff6xIhgjP4YDTj0yCIWaAw8iSjj2E6UUfeL6ml6N6wAvfSA3LhdcARbehGmIchh7T2knwkHgHtfUANLzhE1eHR9gW0dOD50RsWAq1%2FfMwA16yggOzkRUM25vTJKPm0W7gntuQ4l4LN3H%2BGX5VfQwZ6xEKOLXM84HtlV4wjq0CbbleiEMkuSNqPOylyEYEmCibJ2&X-Amz-Signature=ff76555f2d9a0c10d355914bf2c0be9337b85dbe71466e22f641a293f28635a0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
