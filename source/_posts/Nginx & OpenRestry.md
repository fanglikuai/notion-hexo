---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBQTHJHK%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T020051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJGMEQCIGv%2BiNdE66S5YIF4IKzI4NgKZe1yHOonfcDi6vSawSemAiAqQvontBG6b%2BKpxeGMlBNrucMyW4TEW26OkJenJ5S3rir%2FAwhCEAAaDDYzNzQyMzE4MzgwNSIM%2BmToYV05dHcu8Y0BKtwD8wD%2FDZtUuQpD6mPtJ3Zj9Z3Nyyl7D2OW1fjYI0bNavuUWah5Y9gtqwDpiw41ION%2F9SVLFQhm0%2BEFqdTyk5dfQK6s7Zu6svNRGPbWGA%2F3S8Gj3Y7LghII6SFuKZfyHPBN3ZGNwGLm%2Fc7tzZ9OMQOsX3X%2BwtcXttLIvw8PoPNofun5xFSbXDSHBNEwwYnIzSvbFzpuouo2jrQVv%2Fc1bPKJrvdnxsRs9Fv8lYlT%2FGkMBvcDUzUAE8gfc6xKNfaSl9s1pqlLiScHdJV5lLpkP2hPo1OEdMfbOX6wACaiqImL%2BQO21iqHZJDbBHW3pTXAwtdDUvoTvCeNtDbnMULDOoWRYtH9eft1ccTGuN%2Bi0n4pKS6LqPcoU0Na4zqUrHt9j1RbizcoM%2B3v%2FFFVBbdiAJrj9rsJNs%2BnWMUPrCCys00xBAobEldjUKEqTH9JXPsPZYBL2W9RkD8TITGEnrxR%2Bk7jvZQeBMBsIhkwYILOHPs0vZ72JgmARyEQV3kmHfRbApfjEJAUu1j0grxfUCz2N9%2FKjW0bE6H8jhrWhm%2B6cU%2By6g4CLxFxCcGrx9lHiGDG4lJ7XOsw5PqhZXIED3VvwUXH%2Fm8tN9%2BgQF838EbH5x9MscMujLM%2F72Lwd5bc%2FzkwptnUyAY6pgE9HOoiLW36asjFK%2FyHEy%2FSSFXRUc%2F7U7XJxCIS9DH42zWCCo3iy%2BZZgd13a5l%2Flm2if33ypIPuGvEov1ib2XfJL2SwH7Dv8xk%2FU7RMK14c09Iaw0K81sY5pO52N2DwZX0ttpJhIokcq0uOvWf5G8MgZq%2BfwZVy2wPodx3BKsw00llLi%2Bd0ZBxrkAD1F7hgFGmianestmqRS0ZIboxS%2By7%2BPJv%2BUbJg&X-Amz-Signature=9a79c5042b546e1feef3658a3674ef8d1cc8a45fe4bf29a703164145b93f389b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
