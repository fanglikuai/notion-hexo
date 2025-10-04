---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664R3OLGC5%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDunFpt9FNrovL3hhHCV7yzCs3oxDa6RJ7FEqOdlo6b5AiA%2BuPMPiZfkrbySIoNWUW8ySMfMFY%2BCKQ9tFNi0Eu90Wir%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMaxakw%2Bk9M2KtYPsgKtwDDRjOqqFJU3zOJIO7f17F2Kl4Q6DfOByCsikl4dQQp9L5Xb%2B0zmjOaOD7co0X2k0wNMRx6EbIYFI1Pct%2BZ5F8tFqbVCHrIf0UBEb%2BNyVCOoWgakrOujGkJJSsGlYsuV%2BYfKb3RAiJ8Z%2BwjpeKrihyCeQKEBRlP9j2%2BkrUNdxT1yKr2w3oVXLekOVx74gpc4UvgTbWjn5xomWGLjTUtT%2F9VgXB30IJWgzc3rce1aTpkoxbE0dPsMNDda0GE4joAF3kF7CLAXL1%2FxD4JWBnsd%2B8Ij0n2quMT7DwVGoObiqkU9DfZ46%2BWYhpakrf7HWsRpSw8sWsS0uEVihck%2BYU3JTVY1QGjNmxBPgCFNJVoq1d2DsmnmS4XprURuTxr2eOHgK2fVyQHCJHyzDPRwmDpL34LMcZ6QiHMY4QImOwuR%2B3v5X2UeEmMd8ucToxKP42cvK9fCHb4SjuffqYXjW0YNcZLjOUGG%2BHW4PLNvU5KmU12xl8oLJZoU8za2Ocj9feJb%2BDYRvczKAn1SyVHZ48fjfVfOPXz15BUjKzRO5o32Ikjudd6agXS2l5oXmbnrZmF%2BWiOrhruxPqjY%2BhdG6gMxK6KSHp%2B2grmh5uxaZG%2BC%2BQhLqD2aPDh2Qbky4ZrpkwteGDxwY6pgGGj63paDhFQkm5sD9ej01gTvA4JQzDqhUoq6m64RU9vgb8tMfjS%2F%2BF53ke69yOt%2FetlgOawQOXe6jEyPWlg266KtpmhT%2FIdF7YE16X7kYcOww0trP9GDd8nupuFNwxBczKQtQVJ4D2oW0MsBHwasdsn0ucTSSnOnY5HpdW%2FJGOrs%2BnGWzaUL9Fg4iOXQZsRZh1AzLNkv6GYJ6AIDWJyTqbe%2F6uMU9M&X-Amz-Signature=ba33564a3e968c55675d80dea10fed7e7a52bac185584a07ef85bce975e28b99&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
