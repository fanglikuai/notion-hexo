---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666DEJBU65%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T120054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDiXcStMOZh3n%2FxAlC7K1bH3lQLsklVBRH42lIkjRyJlgIhAIO1JJkNGCwlPo4QZFP7vsvge3BpH0MgNST8UDUZfLw8KogECKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwIYSVYeth1ejLwgCUq3AMAEHjlpXz6aYsTOR7m0Zx4IhJNW2VCK9e6fQ6HvzSOnt3cRQdOBF5iLlBlhf5PeBK68Q9Zilic%2F50tsqIyo1EqltJSJmnrRIy90Yj1JEscI1zgCNMX2dQorsOugdFdGzJudjckLTUl6cG8CqQfFBCfg8h6FV9O%2FON33Dc557LZroCahE9mo4T7LZivHBu8O8XAtRkiiXxocURVuxzFi5z2f5%2FDaUE%2F3ryQdDAhJ%2BqUfK4W%2BSzeTMGc60jM94cNJh14IkkqqDZAAYG4604VtJMAzfqCxl9dv2cf9l4N8F1MKCffm%2BiLAA8bU5OJyfJCekNEASr1lbZfXJyvr4hYEgS%2BiJHMj2Z9ZPH0bilxFhaXkwtGsyRSIb39fXNLj7zauMcoJIAqRRKnO4g7bpzL47OnIFCVVZWHMcWefvWh0ph0IyPJG3VEi5h3ASZ0njwrp4%2BjkR%2Bb9TLjYVuIZ0oZPpX8GYMXEYFtUN7MSdNadPQZYCk9%2BHYiNw%2Fy4Erw5ApK9NSpMwxALkr2SM9bzuOjECzoLegfAmBSoTip6ythGGHjB4HQHHYTHbhvWJF%2F0ptdhULNPjQ2%2BKHU3g9nCDORu4UFlKMdmncU59M%2FDuIj72YPCKSMM1p60J1o9kog9TC5kv3HBjqkAehZa8%2FPLFsWkrbQTfyIW30nyko%2Fl2T4Xh%2F9971qJgg9jVRxeroJtPW5JTH2cmHb%2Fih%2FRmmNUEb1jM8MPWB3PhZ3Gs%2BTxmiNfJtwH85hLg6k6lGN4J%2B9WGLszdM%2BaURnpui9FpsqAJymBAnf9%2FdSdixBBvpffhWCW8WWy6jeKR3Nm6KRmDdGkKCUZKkLD%2Fg2yNHykUtybOfaRpkiwoXzhJ25V2m2&X-Amz-Signature=cc63ba95eb140a25cc6998ead931d6d95769676c713ac0975bb71c27e6a0e7a4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
