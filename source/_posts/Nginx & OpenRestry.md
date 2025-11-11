---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLNFM7Y4%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFYaCXVzLXdlc3QtMiJIMEYCIQCqvorJOwlMRh6A%2FZjrEcVe1YToZQkVbBxUe4ubV3OkWQIhAKawUggLRx72WVOyX07Dc2yaK8gf2LUm7eOgoMLLjxzNKv8DCB8QABoMNjM3NDIzMTgzODA1IgxzNzcfZf6cEVA%2FInAq3AN51eQPZ5ZlV7Ynd2JtFEgaWvxWTtZ8j771rLi1R%2FsEEf119O1zcyOYZ4SxHumkQeiF1Us6MMa%2BaiIh%2Flign3ovdOxXSFvYRvT8CdvT79ejHxfcvEhfv%2Bk7Eiq5U1U8YcB9CrlEmh9MhwXYBJKT7sD3%2BMgvyI%2FAFG48a%2FyQL%2FLAeVizoQ3YFk4RC3G58yy8MUW6eP05nO0Szp0OTwGPUXQ%2FU9DcuO7qnZSB9fbDz%2BcqzIAvhDv6OYLVHs6D%2Fp3pzmR3HbJ%2BZoRncbEYquZjjLJUyBWcTqJfSH3feUZGkWNXwAcx3baTri7TM6YvBelIlEg7x9MlqxZ9PuuYP0I5g1BH%2FCLDa3CGah%2Fh5pVczA8eBOgoDyaps6xuuHvDYgF7s1PxTj2VQGAvjGhZXWSZymvtBuahCxpwYhhyeFlPnS5vpmLkP7V1iAL3Ex6Nfdt61%2F%2F89CD58vKPRAphpdEkFx6r9dEoxWj2tMc3f%2BhJouw0qrHtgOZT2wFleQjQnRMoGstmvrx%2FcLxqPJAqJwzofND%2B%2Bo%2BpFlyzJ%2Fr5dDdPx4PJ%2BOUyZ%2FMsytepCSEaWT8%2BdDGLn%2Fgxxq0dUsfEXbzY5ZU%2BSfdWOhcAuDGPbRdF%2BpTv2c5KUAxiewc%2F5eF3BjCD8szIBjqkAdlyKnKoPGF19zpUqXzZrDYHuC762Y8YfwWFKFWoWmZr05ixFi0GZKR%2FASUcCDHYh2F11Ze2odMKkZqC8LNGqakIfn8DDDVvaD06n0DEVi9PtHuJ6rXwsREqcMuhAvVcjsZc%2FRN9GH%2FeAhYR0%2BY%2FjJ0rijhIaQkaKOak6kP3SuordpzTL%2BBrkJfZQFUHTVgS5lPedomz8tZZNH%2BqHhJpKPZswJIY&X-Amz-Signature=9dcb5caeee6ff1a79a7246c21869f6aba23c50ffa5a7bf4fa6a22c07f2bfcdb9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
