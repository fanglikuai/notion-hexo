---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SOQWMDDG%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T020052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQCQGZYemQTgo7AUw69HhRe7TQzY8rlvV9sqzK3oN0wcbQIhAO6m7yN8FHbd%2BqfQc3eMM7OSjsdV%2F68RqerYv%2BDxgcg5KogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyJw2sl1n2cdriZ5MAq3APH9TRyVqGqk1zjER6r0MuuAh6KJTJ9gXTDFCr6GLg%2Bn5X%2FyJIzzJ%2FaRuMnSA%2FqJMhtGXiSW8Edj42tKrR3gw1k24x6K5nt3AMiayQ58mjWXXHsU3BgRLQrbybFgNEIbDsbrDOxmROaohaw%2B4wQsyqpO0PzvPKPy9CV204q0GECgKuiyLKiNg1d71NcdKV%2FdYf9bYyUWo6KEdpBYIR8qkytzhWVaY%2BdzRkoqg03REl8eIR0g84BzrGlJdgv0X5D7nauGL6YPhuQ4C2pC1TNTz%2BKjWlng7PciON9iS1j1N%2FSLi3bIUnz53GkzGS76j5WZbkw46hOeq0ugHrzYyFwyKjpcl3X%2FknNCmjS7YRICfcSgm7Dku%2BqOwF40%2FhzgRVEktNzI7iTu8MWHlUEC1E0YGtZKCgKL4ZOCbyMhryXN9FfqcsRYj%2BwsDLqcgEx1rm0M21dnS7wKkfZPX4Sp7bJfZH4%2BSg2l%2BJLWcxYd5XfdwieronHnhCSvZr6rvh%2BU7IQu7F3rRxG%2B5xuG1RpRJA5WmkIlkoHP%2F1KL4tYgPgKQY4Pjxf5Z7NI5v9gRc%2BGTpyvWXJ7Z0qzo4lMNvtuztroyD6mjZGG3QUs4Kqod9q5GrKpxRt3WoBRTSiKOz2ebzCu7sTIBjqkAdXrE%2FmHVYKF6qeXZA6DLIa%2BnkQgKNXhQofnAQNAHocvsvB2Uahf%2B6digqAlS7xbBAoGuygJzb0wIYoS0OATTRpkaYSPkehUKIA1WbiErt%2BTEHAJZY%2F6EuXUxZttAiZJScb95Yt9k1oT7m0XYpf0dYsVuBW0hk5HyMvTe7WgGdHalaD1ZXILYZal%2BZI9LkD2OlJiV4%2Fla1LaKNSm4ZFlqgsEzJcv&X-Amz-Signature=a7aaddaf4e0317be103e609490daea6a20d8b24d3bf090da04719b9e404f0d8c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
