---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46625ZFMQC7%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T020040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJGMEQCIE%2BxShgdyLC5mWWqB7Dvi0Li2GP%2BskNjWNOFKaBcrEOZAiAJqcv%2BBmrxkRojLzpruqXaln0ilJt8kL8t7%2FwMhmZm5yr%2FAwgfEAAaDDYzNzQyMzE4MzgwNSIMt7Y7vpBBxWN2yAvHKtwDfYnroJvP4HSTtgEH9w7ifcndWfw4ROSgFFDWN%2B%2FK1VMWlOnM9SGHEzhwIjWDWn172lvUmw8Kj72H9WY1W5a6xqrFh1qOwRf9ks%2B982JHE5d28f4%2BfcRu8%2BC2ZLymG8fCIJsqYvmlxmzsHvWotoK7FrXgl6Y33%2FbH56rkn6Cf%2B2pbKTQfXd8rszSLB9dy%2BAZt0wSI6oEqDKF5dapa6rOMBJqCiq%2BuWvYXB20TIoNMQYEX0cnNJ8tEbD17os6QVoDYHcDE%2BQyCBss3eDbzXqtbkA1Y1SxyZZu8NI4GN8dfDCHNxo99yyhoE0LGut1jv6%2F0FAFt1OVpLYu%2FGSw8%2FzeBq36SzkIQHm130%2FcvoqeRy4ZKdFqEazs6pFM6X1FHdXZxyG0OOOIp9AazE0FN0kE%2Bzzq%2FRv60QmUooZNF9topqP4p2Ashc%2FNCeHg4k%2FvomKXutyBT4gxsXhoX%2BBw7XgFAZSQfFXCgFB6CaVB7yGw7PTewDLIvyFDBgxxK0wYmQVbL4JqJ2EVxx01gUEOoDTw0gmUdwI2Y0lnQ5jy7g2kyOPUKE5eOz%2Fk6w8lmfxbSLCqCF2VLkcezzo5c5MB7j7xgrtX1vffFpE%2Fv%2FmzvTdPeDYR8m2HwiMZXtEJYK8kw1KarxwY6pgFSTMWUt5VP3oGX2%2Bu0R%2FWogtpxbmRANK%2FNdtYbP7nsZ1yRAi0E8t7ZUYTg20CAMelXxwN%2BHX3P%2FPNzw5%2BM2OXv2opl00JKbvM7hpcNZBFqi0UBSpHb%2Bns7pDAVFGo%2Bd%2FWBRMAwDLrNxZMAU6rGgUcI80h2DAjjk2R9YQxFyVdvyW1deBbrNd8Vmo73tIasN%2FdagMTzVh6X0OnpEWNu8fstJUHdpJcA&X-Amz-Signature=8ad2c0acc6c13ee74bef289b97c4ab180e3c86f8ce8f73dc75a41167cc070084&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
