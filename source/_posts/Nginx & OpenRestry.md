---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSVKGBNV%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T070048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDwm99TOpEzHuLySKJStdD3UxTzVGWU8xv%2BVGOSJM7ISgIhAODTw3zAXMu3HPN4tCzFMsCkZmKZfOiyI4Lzb5oykpiqKv8DCAcQABoMNjM3NDIzMTgzODA1IgwtlII96tRo%2BLw5CBUq3AMF6HgQlq7khBIAYpjE97Zro7LFzb9ePQ3YAReGQOmcQX95L4V5X8xndKbJy3P58PidzwV%2BoPNVhujZRsYkSdpYk6F9E6nKllhrmA8lSpohDkcwAFVxkAOgyvRt%2FIbNMKLyJABqIQFEM4o9auJUZKg1IoRZhTYnSbF3UEXZK5shjx87qtoPSPFB%2FYEVPt6ZTG6X%2BqyLo00VQQ%2FbcA%2FQWa%2FZGwsfbVpeN%2BYm5y9NL6GlVVcvQR9eCy%2B3nNqpsiWmrC0A6NdKTZvmTa%2FNmxiThmUp9UAbbiNAwVbbBJNOrEgP1VnaibArMPxrbSn32dUZ9hz46p6ZiepYD2ExsLfhm71cjMjOBSf%2BIdJ1oF8TNCrAykA17iwr43K6Fku37NreagC72pe%2BJTtB0UJHxcKeiC6ixT2t%2Ffz6QQGZbNgmf5ygVyyh9JbhgrvQngNnse7RAr1MvRRpdbz2dxqoR7fp%2BrHWWRha73Y%2FOFZdC%2BsbNEEjgq5GP5USfWEXVl0M%2F49%2FFR5ApHbvaRJPta8Sxn%2BLXMRqd38xcxWRfAVdKiXrp%2FG3J2oxI3mVZmkHUE2Sf6zCd8uqf8KmLRf9cDx4KeX5%2FQlAyd%2FYh76%2Fn1VU6EmGZmXhSWPGcpb5F%2F6ljDevZjDVgoDJBjqkAUGpnLh1VNQjcDgnrd85Wh%2Bd6eGxk%2FS52vlyk6LGyOkZfjs8wNOgn5jt6wu69F6EY7gS8n56ueto9eDw6tyf9KwMoGv0e8NqaGnt7XB3qhL61Yga44KNndMtYg4k4Uv%2BDpMzkyJp8%2FWbr6mOrHzRMwvGeGF%2B%2FS%2Bau5vKmU0lc4fkFmQtFaJ5XTxngkI6E7s3mH2pFTABIvuPwriG5ZP42w6cp4dw&X-Amz-Signature=c67fe7366c7040fb4044f90a7b8f764b25199700f10ee85ee67452e5076baa46&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
