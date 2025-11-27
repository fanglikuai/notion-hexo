---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FJSMTAR%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T170038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB6KopfUNGgDGz6jzcuk0G2OAQo6qgX9CdeMlsJ%2FzyTrAiBDXQaTTSsh6BPQlXd4ZPGy%2FdzRjqevsRg3I7cjRTlJDCqIBAih%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMB6QS%2B8jvmr6%2BR2KGKtwDv%2FYfGiyMgLSDSBfO9UG68rBMHL12F6Blz9bOfL5hFGiTbLBlxAnmPm5dkGcOaOtmpxbjBJVEiTAEXpjqekOlyJxl8ETIww2VxiuM8%2FA8BqoN4E5JglWxaJYWwQpfLhEuzgcYAx%2FVZ1IBqkwOtYzf%2B8D0xOtZKF9USA3OOkiEZibKTgAQDXCvokDicwyxkQahdtMoiNMMZPpLLRt78gKc%2F2rS%2Bew9bS5vhKlGaJcn2XZ%2FkBvGhh%2FdGwBxwxP%2Brx2a8dbrOEJXHt6pseC8M9HoVLZpLGXlqyrLKjM%2FJD3dxehYQMBf9D48NO5OgLqYBejGfGqSOF4cCrJG4ychM3gSLEgs0Il8ndfvkWdoboTS%2FMXgCqRcJW0hILsBf1m6Bcz7wI9Oytj%2FxDu7MYqDXxG6OKb9w%2BRPbeiyo3aDDlnEsGnyCF3TEoa1sVJyw5HB815fId%2B%2FNxifM2rJ7H9yKl3Nd8DN5lWlFPGJjenVbDYq94WpcZuS3x%2BBuufGCO2NbmyAd%2Fl7aijJjXVyWNyYC4RHhB7o9eRngEIVOgoR7fbASgZTXcL0Q7wEv55oQakT1NsDISvvimJXPpketnSAUBb5XIVXq3NuXZiQHsTiGRrvAgp8Esb4Zz2h%2BGMRxosw8e%2BhyQY6pgGCzUmQ1wm83wMVf6ovMqtxBYrhjcDSqexXcZYK9UlUHCeBCcOpZOg5EiBkE5q0R5SV6%2BQ9mqfP0A7LtZSD%2F41g5U%2Fp4LFR31xh07zVhENjQCovq8J1iiMc%2BuWtDuH32fxqFPLBpqcmDWcDTZ2yHQfohpkawcz0S2Rtf%2BHNjMy7J3NQZtAwbK0u%2Fvr48mmTcvipuu2p%2FotUfudAwYccmFtYraTyYYPT&X-Amz-Signature=a80f31c9b85ae915c01eb00815370f57323963145fedbbabcbc5a64489cb38c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
