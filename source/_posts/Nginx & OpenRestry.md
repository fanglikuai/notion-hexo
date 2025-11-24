---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666TRXVTIX%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T010053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD78vd986mDTwYniVb5C8Du6Rg1gN%2FaohLYWaobIUdLzQIgKz017NuC%2F9%2BIpdNDGpFK0AZZmE%2BvdU1YNGQpTxILmJQq%2FwMISRAAGgw2Mzc0MjMxODM4MDUiDBPSRZsRLoBPvPGTpSrcA1ZtboVYrZIZcUYOVtMjCR6aVo2TMj2Il4n9f%2Bxdy5%2Bg7eMl5GQ5zSuldOdnSgK35G%2FkB8KHAlGVwvA%2BXcyVnVoqPvwz%2B6fHM2T3p84VhD5qh7Qaj0hMwktJGAoflEaGC5%2Fdhbvh6nWpvifhbULe%2Fo2BSQBADCaRwLOc3r2tMt0f%2F5ORVsKLL0jl%2BxWW8i65alz7qLfa8MIyO1npO8oK6CbsKXerw1qpikLui72KLPa1H0obRPFxV6WQ5f40TtnuvkV2ys0wVGH7iM2aoRJra6%2BRqjtubSX1WMJLUY%2Bb%2Bc%2F4AD%2FwBbH15aNy6kwdqXaVimxmNxqluQvh8NBJyC19XYXbXlC%2FTrDAiBmizHLMImzkilkfP9Z%2FmHOeB8ETTWtPfv3c9ZWqyBSTXGsZni1itYIOOE%2Bq2scmGKRkhVNil9r9oMIfuHnB2HF3zHnCWn1YfCvOPj6R2KHMSXow6w8wSmA31TWAhMIUqrdaMrIbv8MJBL93FKlwOlmXoxr7T9CzG86IZblhDtDb4t2VOYHfsXE6%2BRma8qD30MKAf6T9%2FWwV2zxZ5WuNfMqaK%2FsD2FMNkkKP9aaTrOZi6JZaEkuVsXrsVIEp83mshsCIAynUthVxVUV0zlt8Eb8VAzUsMNLJjskGOqUBGPrVUqkmUPn8Ny1MjswVlr3RxqmE4ZdSKGyWdzMOMfIQOHemAm%2BV184vxyhDaIjN2QWSjxkJqR119OaS9BdHceKQQ1WDvnGrkpwiy%2BGCUlRoHzk1HnsfoHrIKZVzsrIdBmX7VrIbHpl%2BeiUYsIRMBI%2BuxcUzmIOHyXEywRP%2BiQYVlCWROkJ1U5jfEydvSFJk3giWpMJWbVfS6O5OOBXleb2KpwBc&X-Amz-Signature=b5beb90a1e86f98a189d071adca079c6c3396f8924f9592e02b238a362d0b7c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
