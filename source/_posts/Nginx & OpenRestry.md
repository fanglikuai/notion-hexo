---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YRHIBART%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T030049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGIOEvEByZT%2F2WbDADEh1pnrtzo%2BtuPzrUQJ5PkqHrP2AiBfkMOUiyAFGIvSxZ1XXtsiZsAJ0HFcXm8C9JAHj%2FfkhCqIBAiD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMv5y%2BGy1ru6T78bk9KtwD6Gbg%2FjykGXroOXyeINcy0mLliHYhdKx8q%2Bxt4EpS0xmYqq6Qdq5bAm9GmRGgfWySD9IJUjHAgyFcaFubmv6SnCL%2F6Zh8cgRJ7ld%2F%2Fxz%2BfX3WrdtXZ%2FAkKcISfiFLi9rMWohmI54tPk6tDTIzslOBxQSksvS3%2BA51QGa%2BWEwO8IGTAuAY3va7hIyk7cjjWWgkNnEaBQUU4Az5nxfY0Z3hNgRFQ9oqX82yD9TGFsWBvChQBdBgEuBRqW5lTycAHnN3p9YuLp1T0bVAJGbyiuhz6wBJ6tudNBh8brhLAeygy2iTsUTr8jDh0lmB3RVie%2Bnv6Oj0bv3g78mEbYzorn77B6u6uKZ8JxkqOTE2rpcLiStlmxaVOjBUpgWhPVrbqlzjXmM0FFSRwZ1E3NYHvknXFL%2FlCCHzuq3Z5WSYZWho0bIJoHkrKjxPt1T4JLM2OtUVxEPDvmw1AWQtVacf53SodNuw79eSGMm6Bnw5VswakB7q6k9xHejP004mwKtqUpThEgXHioM311rFJ9vZOillk8WpKPF9mWgsGCRL7xV0826kThIpNfuclVv%2FcSNqhXpZ3r4CqEX%2BoJNMOOHbqNZbOxKxRrcKua6YZW3jw7sYoTmeBpjBtRIndCbFVmswgp7BxwY6pgFAh79SSgED5GXoEOe06%2BO6OE5p64iSsk06PcqHNOdfeghMIoS264NPL9hAR8%2B8sJCHBkc35cU8cHZi3wYf6KZsep3mo1hEHAdQHgJ2Lt4F%2F3zLIg0jp8N8ZjqohHR4%2BPcTPraJnOw3sdONC%2F0%2B2EJ9qQZ6E4pLkougFuiI%2BqgGNtt%2B%2BuAHkdYpyk0orNcYJJi%2F1O%2FvSM4vLfWdYYlsO2L3AvKX6oud&X-Amz-Signature=e8faf1af97004ae8b0ae3fcb554c57fc4bfbeaf2ff8c595d99b2aa039f8108bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
