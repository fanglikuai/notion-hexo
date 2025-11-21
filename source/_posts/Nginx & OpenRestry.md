---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZPI3PRL3%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T020038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDC%2B14XTYx1me5CmllXDTZugsIwTZwl6vFNynld9mzdSQIgEpzivkhaQaS1PB3qXOItX9I%2F3nr1HRbJ%2Fk3GU0nf2Wcq%2FwMIAxAAGgw2Mzc0MjMxODM4MDUiDKbTMTG1njIHBL9JtyrcAxQCuqiKp5HGEfvRVNBXzeKhliR080QuZLty8aHg81ws3aBU0On1L47y6heoZ3qO4qCCbNb9doJGpNpu7A1ZT8XQNMvKE2Y0MmaKxSu%2BUzR2L0kSzlFv7KzseQZmSChfTOFJtJ7OPVGiik4cn%2BDsQfhw5m5gAig15eTMe9wTYxIqveAy%2FlSXMi128W0MPow3yQPDP5nI%2FMwkJ2wqH%2BMVpQU6lWzQva9vwWz6zJlEalNZC9fGPuJYbQ3XSQfuNKQ2okwy%2Bb%2BuH%2B%2FcePipYMvIDHapm%2Bz3o6u7bAFjPa7Q%2FoHBRr237r5tmx%2BoIRF5ctAjuz8tcwPAqm4GiaurG8vSH33PqnBSehr%2FRw70GJhxH6rSANb5zmFiTSdKH8hEp21JB%2FUyTZWcWa7cLiqK0tf8R%2B8dDjBbPgoO3Oh5DtMePSm2GafD4fmO6vs9SuyKx8lT4LxkAcJ5D5LJ%2FwLEA4orf3CVqL4MCslBA9YvTqTrm8zHNi%2FbOevsGtdTd3QloGwHE6S3P105X3k3Yn7fo9QKdSnkJgWLA6q%2FMv6Op4eIDc%2BrN4cAczhadhheWKYhQaDTJ1k6x6%2B0I%2Bh50%2FbjgXTFYuttpaxaAsDxNYH9uFClT0taGvdMkBVA1iqk%2FqV3MOyL%2F8gGOqUBpzXOmVakJwMaRAM5l8qSjGB1HG3JKvRig%2BomCo1W4bvuQMFNABFbCFQnGYdZmwfyqDdpw6ZAI9hzBPq9s5d5FHDNpPemiH5AlmjguKhuGt3CE4IKsoSLikkzuUpg5xGa1KqvbI4LlBSpJSDhcFe7%2F%2F6u9ZYhxKyI%2B8irMBeY8n2Ts2jiNuXDuTa0%2FEJ%2BgLaYuMQLjblWcd7%2Fd0D8ydFMLi1lz3NN&X-Amz-Signature=0be8f2fc6d58b6eeddc91a9d9f40da515821d43b9c6a2989e21e6bac62ad1890&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
