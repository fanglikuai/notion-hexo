---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666RUK7COD%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T010042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDp2u5PFDH4fIxvhfwligAqLHaMiKNL7Z6ApWTyRSTo5AiEA7Kj9XSUWGYy51DUHcP4CIb7as60EbvPrj%2BDcKbHh89gq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDBwvfM3Kpf5SXm%2B%2FSCrcA5auXnkByyc%2BKIhhkulNKONLmEkzXiCy6w0Eo6zdA4XlpIBXQjE96dBuNbPT194mmK112DJOUXraip085k07KryHEY%2BG%2FqKuy%2Fpl0hf6ycaklWV8EZv%2BXRatHkZ1pN%2FTRdglKxQG6mN7k9rouJFvVWBKslJW7mpUO%2B2XK3QU4SovakmDHz6KPy63ta5hEKhayFl712v8rmxwDGXIjzxv2FxtYAYuAfdsMS2XLWT321HSU1%2FsjFuQlC2yqk6jINq0EukleaBl6TCxV3CR%2Bu%2FQt%2B%2BiRvkz%2BjSYyit3f5D3o1uzLnQG7WKX9cOJxDWt%2FZJQbeFvU%2BTjwEnx0HJYdLf3VrgyECSa7btWMJJARjmTsS0pbOkEPh8c9BJeze1QGprqQpAozkkRcc6shOhLgck3bP1R76Q6n5jI3s7LdSHZsUafjs6YP4O4gFclIWz02hD3lUsEbaCkLbBfaODP66gp0mJwAUozIR3ztsohUYZMBD3X8VuXXMxPU4bR6HZbDIy2RcknkaFU3iForpknGMU5JnZi5pNWM%2FMrkft4ZMA8u64p4oVgSdHd33KEBMhuW4%2Bgmu0hiLTTWRmwwSeJuTkmI0SaVoeOpM3gM1Vem75aq8agAj%2F4DVNGGzSWYEIfMJrhhscGOqUBMHDem01W43O%2BNzB5aX4R93hD7GphM02LjpuiVO9WIBnaLrsllM%2FIgTfkrpnZZK4KRhuwo7IaqtWt%2BE2xNtn4w8nkH3meyiwEeg%2FcMYFw47yQ9lfHU0FlrzbaWlRBMNpkocqUJUu44oeKrJ1Dga7Ejx5CwKVMGcMkQma4J4YtB9Rnoxa7zZlGuaGaa3dcjh%2BU8XROW1iaW2%2Bna2T50Y%2FhxtrWpA5l&X-Amz-Signature=3a2fd4fb4744117bec2ff3dbb753ec4481706cebdc6832f4b1d48b32e1006b48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
