---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLD5FNHD%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJIMEYCIQDEkkw7tyWUE%2BJCkO7aFhMpfEHL8D0UF3Kt%2Ft6rJdwCFQIhAORs79g3NnktSULK7jCtBsPrZtM47ZFTzPtz8gGtdDJLKv8DCB8QABoMNjM3NDIzMTgzODA1IgwiA%2BymixOuBieeC28q3AMLqhoTNXcbr6sQ07HSqCMyX%2FDmR3pkmQGeO4V35Xr%2FRJxwLrV80GBfHg92I%2FEw7hlBDE%2BiotlM8BW5QtT0zpaAEI8C1JpoCQ0OEFSYc7kU0eOv5%2BuY4evXENbgZdMDpbkVLq9IEvG%2BZJZrN2j2inJqdif6%2BI%2FLge%2BSzXmAeYCt8g3vS5XEOFtYV2jHgHl2Rjlejg%2Bqm7yVbdVFkZ2CSPrRCzg575JmeMNyIu2y8sKMtZexaOE4xO2Z68CNO1HPuKX8Z5QqkKhXGeeWkm9bPU2k6wVcp2vxmfCexU0GMNDYDpt3rsQBXLJtXtvL%2Bv74UFLOTv7HHw%2FlnRwB5X3Qzj6GPBid8T5qftZ5ROr%2B8atkJjsmujGqJGFVtnKPJi7XfH0v%2BxGAnj7VdzwKwZFzX3HmTHaqxkGNAGnl2IPXTYvjaDJz%2FCPJ6rb0nojIZOqrPaljaDb5ROYCAQT0SBVpTmfMTz66sPOGmcHa0PN85BZeHI1riDYzmxIlZBAEctbY35%2Bp8MnPihNk%2FvoSY6a5CQhnurmswKYiVIszLby5CYET4g97hvuHuOvAuXHOU1VJy65sQLOVq2jQ%2FyGGYHUOblrGUsBBEUbxL6vW%2F2v8K%2F2S6a969clNCo3E9RrRRTCl9d%2FHBjqkAZPUxG5Vynh6J%2Bd0GyT%2BtEX6oFEYWegL2py0qd6B3optsUbBEhTX0RfSCbKEwBJveCa144bgBOp5SwhhAjb4zOQgjkQvjZ6B69IEr3ZvlCNLdjT939VorKA4FiK52L9EV0tQzliJhgkenvanv25wIpkceG28dh%2FecYxrLmzT%2BmfVvm%2FrwqcNoGHHcPX%2FBN%2BNaVfk5SPZWeTs%2FuA4%2BA%2B2IpagFOZU&X-Amz-Signature=abfbc3beab1c187e70632b45e0de91e1cbe9338b11028016c54d638d894d468f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
