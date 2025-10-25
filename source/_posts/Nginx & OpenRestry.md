---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3D2N4EQ%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD9GZ3a5WVA2vwfCn6BdJWVlIppIynTgD7%2FaDZ9%2FZV5swIgFQ9VBhQU3YrjzUZHFkAa8462xieYbKOcErmb8a6f03sq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDBKIlD6VTU4ZDPYh2SrcA65WysDtEZzZMl8mNlUBL4lrhHxbLkhEetJUcM1ij70%2FfeqxoHDS2nrXVpGwGBtUOG9m60nWxfZsdP2R9LNHg%2BT3pYY3ZDNKqILvsCfWyi0u3zvW0QFtRmebK%2BVG%2BYH8EUz6A5xyKHJ20tr7Mex1lnVlQW0JBWbWtQDLzcstg5MuSdm81VZNf%2BtSxAYJ2AjUuSwASCtF4gq%2BFTetCtgZjIl7zoHRDnuXVToY%2B51GzlHAz5niXIsGKxDPsw0AzygohrF%2Fk9do7seStczEsb1W%2FZVPLYVTDyS1UQW5ffM9Vyow5szsOneH8yS3U0QP4YQMn9iFWQiLuXk85SNlDuPCupqiIfVSx%2Bdn1DbyNc7jLcNIT2ju%2FVr7%2Bwpe%2Bp58KAN0Byc0zjuDMvTTHPGGjQx0Aj6YqHvwkrc1IVmkr1qQ6z9Yqh%2F3K2nA6BBxklZ%2FdEOTgYC6zbFimBcc0qrlzN0Pok%2B8Kg9a7Y9jBb0fRZ5ukhLIf7tJT2Oju83hUTPwpnLuJ5SZkFr7QaH%2Fv1c49Pt801qj4oRUfOnfKzLodbVX7PO0ac%2Bzc7i1gqKETtJHpUD6%2BcbotHqKcbaHpx%2B9T%2FepzSLC7d0o3eizrX3FURNuIOqOrkdEIlLW9pQX4jvEMP%2F088cGOqUBKPV28kSpSvmV3SyOtZhDEnf9zpbYTvRTENkoLCjUsx5wNewxZpYKXqyk%2FE6hYc%2Bogw1VB97Ze8MfzS%2FLPu069qrRZEB2ePzj5KKKyHlaNCABOMfA5hz54EvIEEoUnD2UJgCVAQIyCa3vIt%2BkjfEWou8AJ2B%2FglTw%2F1zWpJcPHpoIZihGHa5vhRjQDPH%2FSJIA4tHlD%2FQB0gacnW62NocbGg3V1Eqa&X-Amz-Signature=1eb3f31ccfadc39a3c2719d3f06d16e003127448b2f79ff75259b85f01b0cf18&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
