---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RNCZADRK%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T170044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDAaCXVzLXdlc3QtMiJGMEQCIGHApIZqHYhQRUd5VnaVBtY%2F98Hcc3MS4bAPPgg9f%2Be6AiAo%2FyhkTtKGbiyqwsyqUbSDq%2FUxIz4LhQTdsgyg9T5gJCqIBAip%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXGJ5xFPBiv6Jegr2KtwD%2F6JSuX1jtKbvFyj79F0lYcOy9kx5XdSm3%2FzeUm1QxXNFwFMQHLCargYcSYkkFNSYcE7s2NKOa2dTQrk9i8GbK12ceIkS8R5UE8tkH3Aivq%2FXAiicCA2UUG127qKPbKgpKiIG%2BgAK92hhSjG9eapOf41rPmOYwMSgoQMy0C90gMiLjARFccgY28YXk%2BV4PEyUnuFzht03Zs%2B1J1oJBmwjmRxtyA3Rp4xttGGNEyFXayOP8B2ptDfPpZnmtsgoFwqQ1ajaP7VKO46PBa46qtifM%2Bi16ymiVIWioGFKU3ShMpGN2nda5UG%2BmjmjKPmAzxHHoAQ3SCzTYhTkJQyNhNxOSh7WU2IC2aWgIVNZNMGnC3oXJxeFAJG1Dc3G3rmR5J0%2BpduClGcKjtvssKxmnYXxUCsiQYoSPH2L0dHutyvQ7eqKrXfwgKQMxCNUG%2BmBijEkTo52FwxsjFHi5irVmQbgm%2BJAQKb4S%2BUFZGOlwVi8hDfEE37ong5cL1XxcDqzsx%2Bunw6esA3CQBzqWoA9tM9UajAALTIw%2FsFkkSb4X2fCW%2FVTSNfNQvSctgT5ZKDPxLxVvFZuRzEvb2Tk0xhf7VVW5RkvzcFMLWEBgZBEi16E%2BdxDFih4G%2FYcWzJBO04wnqyrxgY6pgHl5h19K829lH4NmrqmuPE40HumPteLdlJJQP69UOlBWoDh63pYsK8o%2BagyUAm2Ri7YJu1m1GlUcOnCXU2X%2B0yOjs7RHMyhSv0EPeZzFsNWBRFzu33eA2JJjP9p%2B4JE3zT0XBGL9uE%2FOYMYHd5957EbdHQ2Y8mb3ICxXTFB5ocksPr8DvAQMw5ACSL99NCbcCdEp6ISOI%2FvDsfKJSHLprQ9bwDC5OJx&X-Amz-Signature=b181ca15574063b82f32a5cdf296b58b1d4a143a3d6e9031ec92b6e072f70ba8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
