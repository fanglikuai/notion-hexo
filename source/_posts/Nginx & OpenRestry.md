---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTT66ZS6%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T220038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA4aCXVzLXdlc3QtMiJIMEYCIQC3%2Bzc61VGGey1aLSyP62uU2uJZ1h2iLjLFQLLd21F85wIhAIxGoPMJHc6chQuSyZpby5z5mkrYL5p9mjZfH5jD8oefKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzhcUc7W33TpSfSXJEq3AMpPNj33%2FcrAd0qmTczRK1yKC6Y5C1OJsxnqNrRxDVyYPuI09f%2B4dkxtP0qq3KfVvhJQUgFYIFoZQKB1tTbQ7%2BgOpoOW3hFZk5QYwMJqITrIioTZhGNKwDT6PHDPmWWi6NaGR58PyJ%2BUNAcZj2ArEmp2Y80viRHa1ymTgZIThWCwG8JLHazuOA5I6%2F7%2BBznuGvorfAwA5dOqWCuN6lVA3schfPlP6Am8y%2Bcca2kgmblzcq4CDp%2FZhVvmYLU7dJbPsjKWUfDvHKfWMZDcL44N5jhVw4cpNzfl9Mjw3nBPrmNJZytBhu4EJfs08fMvYCC%2BS84Yq1ILkqVQzPpTlfJLTL%2FTdGRbcPj2P6tVvFj0RpgVVQk1eGvhZ9zOKMgcT2gc6dgfSN6A0C1lW7HgUdXT%2FvGIXl%2Bp48RSKacKMj7j0rtLBUPnsPoYXUVP%2BwsTdhAGBiTOD%2F5OfDT5G40g9yyNLTEUklEfHkF3snbB7WC6mvnJpM93NRm8SAU%2FvA5LrjYk8vvPs0drtLsI63e%2FwCiG2MG%2BQqViRe4jjHWiHLCLC%2B%2B8fzlWKqzkZGKFoLBsK%2FQxUKhLv9p8CeXGtI6UYIl5KmQGW0tTXs4q8pM7JTDRh35DwUW0NvM5edw2efGLDDclNzGBjqkAZyptRPqsC3jiAo4Bd%2BmnrIeU1W%2B2VqiyzvO3kpvCrruO%2F1HOXceiOKrZI7P8sr8EKcsR9jXIpLvS6%2FsN%2BXDiryvM5G%2BnuKqHM%2Bl1cQxBDlqGIwF0m3FtRqBylZ%2BWKLJMLi3J%2Fh6REc0lLNMr6Aq6o4AEBmWI48mPlPDXvtg6HDISTsBNcjzXJRVPSOPMCTnnUtH8pNdIUwPnI8mBIosxXMZM5z2&X-Amz-Signature=f597c11349b1f41d7b3f106862a0c12e7e0abe1a24f458959265c684b04d7d66&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
