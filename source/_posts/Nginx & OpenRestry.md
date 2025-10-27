---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GU6KW5W%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFW1NVokSZdlK7ecr5F0%2Fp86mjxPgUN22JGf8LmIqh%2FaAiApem3RU57p6rSjRVgrxDfZdWrBCBA1eRiiYnHN%2B6%2FLsyqIBAig%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYqs3TRotHhvlf5jDKtwD8k2bwjjNNm6qY5GCOecw%2BvG%2B5ZXpJ2ROHgwAvS2SgE0li9sI0OYxXmYJJ7KXKRliKEKDoFojmFAJQTjP%2BUIf%2F%2FWnyFV8KCS6wym%2BT%2BYZYHgIGW01491ykK0frboRjUrh7LYWk3lwZy5WkB0ks0pyT%2BRRxcMdGpRiNNu4Tt1J%2FmYevOeD%2B92wmJqaxksAHcBbtSwIlhM3EbNnjh2iZ8iJL3ELDqZdUD8Z5kFZD%2BckqIZGgWT2%2FTf6VZc8HyCM0qFeLCUqK9wMkR397181%2F7VwPtLO5z5B53xHDWRDy%2B3BpHhcXIExfWRiv5vZcY3jcY9EoPLsleKsg86FjPc1fRm%2F557tFiBM%2FewCmeWFlt6BlD8Vm3NmFv9hG7O6wFE6emktSW6M3cldj%2BuXV%2B0tYCUhimkwEWuegkQActiqFvH%2B0uN8mlSIXwf1Jmcu4BwORzJAQbrevNIt%2BKNV42Y4z9cFuylYjf%2F8IZbbxX6Dmp4BNb7z9aL0A9%2F04UosgntFOJ4SKnqtU9LlR9SMPha%2FWxhEZqkSNMsO2uExnsvd2tXZkFpV7xZVVdJyZzZRMjlsiQUw%2FvA89VLobHrf%2FQmW5Zu6FD72POxX1aXDDT5Hg4bZdDrkRWHrtg8lT2qv1L8wprD8xwY6pgEgsFNpyLdE1%2BIsxMFa5iPpMs4JGBDjLCU4g9vSGeOMWP7sfWo0z3XeAPvHWQrDDNaO8MdIKj7lLUXHVjnypqzcPvYvLOGukBkb8AJrDs8uZsb%2BoTDGYplb%2Bco3LmVp9Ltw6n0Qq5nHUvjqDNr0AiwoD%2Bcf5o4Wf9BJ67wA6S3OjAM2cd6WbGDdz%2FR8K7YZ6Xk5pZs7PZg%2BrwXjWU1kcd2Vv%2BAvrzIW&X-Amz-Signature=3ca15d4f59e05c6e7745c2308c7d5af117d36ce4fed47486dde72708713a6f8b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
