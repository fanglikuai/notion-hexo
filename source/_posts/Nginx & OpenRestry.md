---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662KF6JAR7%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T200048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFQaCXVzLXdlc3QtMiJHMEUCIHVvEMns%2BimOVd9Ov12nSmhK%2Bpdu1iEiR8lAPzlQoQcFAiEApIh5ruJSW%2BJ5ErF5sIOreGRsJdG7aPJu1h2JIP55tEIq%2FwMIHRAAGgw2Mzc0MjMxODM4MDUiDFNqyuLbMdtRn4gVpSrcAwcmnraMoIbhZYwEQ6PrfSLItEUjEJA1o7%2FSbuss%2B26O0ZoBhEGsvvU5mm4CD%2FNrH0Q7iyXxLS4Oeyqd0qa4AC4kD46u%2FYDk%2FmhybyDx9eMBLhcnk8PWF%2B0%2BGoUCgFhrODNF4WsSi2Sd33%2BJlwOZwv7bneJTjJjqeJ6kkXoChquP0%2Bm%2BbBV2Gbeg9iAjgm8STdO76fs4NkjtMR7CtoRdcBMw5hoNzpITr28PU9%2Fyf5RaVBnmHOWdHXPN%2FurVfxEMzUb1KvKITlNdkbkaZ6XIuv6axsQgNxlfYNZo23WyNnmtqZrcqIZ%2FxQEtfCvw27lQ%2BbSeTPQEJL7HvGC6n6343WcWYn%2F8beRvTkLgV%2FC%2FCKnHOCLKS9jL7PYndBolErrJ%2FE6HWwuF6Am7L4AuQvMY7LW4EFWPEfLk1neJMePy3UWPDnryJ0MiZSphcK4InYAtpxH39nYjmxIz2uWwpRgXk%2Fvw3PZD4K24Uz5A43HBpRbNNMAch9szasDa0MDFKM99ZsMM1r%2FRuiisR9Fox4DHPzoYHHkaA958erpgIIIrvxrFx8txJvGkZEk0DcoFAaWNCib8zTigSEW7Be%2B1IVlmoNcJkOhLh1sJmzzyxAC5WmPJhuhtTYtGgxVjq7EnMKOilMgGOqUBohA0WEegpRFcSEJKp06%2B1z6Ps%2B%2BQGcDR%2Fe%2Bdpvji8AtzVUgpg%2BsPVM9z5WuR%2BxOu2nljBSsIw8Z%2BuJSSpb58xfvRK2Aqd1t2XyW2lz8OgElK19iWu5PhYSX09no9uaSe6EuozC1Lg8WF5ATGlSZyICZ9hTt%2BFeekfJmmQ3NEYEow%2FOr3h7%2BG9GDE4m9CBkiuH3QoTwGziWFLLWtFh8u9vHjWL2Ck&X-Amz-Signature=db7f4dd0c9711534f6a78335ab3543e00410aef4309bf3942cf7ee74113b7718&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
