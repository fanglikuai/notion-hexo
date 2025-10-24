---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YEZEXL2H%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T160042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEVx%2Bjf1%2BF4U%2FUs9BnH9MeVzoD67mS8iy30gGHv2i%2Bv4AiEA%2FYa8lxPSC2QDRg%2FX0a7eBflQBksh4LencqomoghfqSIq%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDLRNxxdefUAtN0AjFSrcAy1m9FwZXzqxj66vJ%2FPEzJesvNv3GdR6l1XjrOPZK4%2BRH%2FYJeQeAaMchBeUAt64AEzQoGvHfjNXV7y6JaswjhRQRWONd%2FAvOdqh4rbN4VbDE5N8YhO42O6BdtlcvY0yBk3bmitKVtTpO20Jo%2FItjWDpFjnlYsnR9PTVAxEH3v5cty0K6AX6kE2Ecu5st%2FIGDsqLmpP6lhjpG8qiQmfRpJOOlSgUBy6deFSx%2F7f5Trh8G5WPg3t%2FNfUMYsTsVTkq2%2FGPfpseHBbhBqNuY8ROg0YWo0nG93S2TcX1AtrNHaEprzYio%2B2JGSsHCYxrXhu5n9FnCy%2B6fL7qSdPGuRKwawi99zCRpquHqfhTsCbgtavEXld8iMFAjgbZ2DSFLIE8OdgF9FvuwPbCUjtUKukBuYpHJd%2FfwIWfUvvUMlZtgd4iXJ0y0DGcOjVw6w3981bplo6yVZLZRFtZnwIUAqxgJMeTYhoU22QAf2iqCsSDuNnz8q%2B3eviBer9AS%2BfvhsAlRRJvb8WYak3xSKaTJg9%2BBQhplSbf6qBYJUmEskcrBdQMwq7KjDBlYGejzEYWMQMNTiTqwEfhJfm7b0pjYPmlFLJ5mcA2tG%2F7DUbaa3vEGkWtcxDNXNIdmf0nvxvYnMMqu7scGOqUBoBq%2FDX8MdL70s8g3RkVxcOsw5Z3PR4xw1hnhOTzzJrEVsHWX8uQgJWHqjlomfu7tWdrfFNG11f1be65h8QBW9DZmSnmW6AbkdU9l5n34r8LLQxL9qvrdy8VeNNLR3kOSUc%2BinlA7o09CpcnOXnxbe4vcc6UaMzE1ErxhRBBwm4vWZ24k0dsWMtbLAWqYJH%2BDP3ReaD6Aa0fVtrUGki3XzxVuHzaH&X-Amz-Signature=28e7f437ed56d164a06ee73d519abe65fd5dc39705fb34abb67a82255cf3b371&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
