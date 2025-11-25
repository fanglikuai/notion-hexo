---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VCZCBOAC%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDKLWd%2FXpfOgmOT2hxKeAQg7R2IJ%2BcjMU1ZAzuQ4eRvGgIhAJlD9B6JXCcH2GkRl3vFgVvCVLOvpypNDguoKGaZJTX7Kv8DCGMQABoMNjM3NDIzMTgzODA1IgzEFLQs9dVO2tPtUU4q3APE%2F4hhz4szsxmsH5KIdZAktmL79mvq1NUbOpBt9ZOFQHhRAAeOmKrwzsD9WG9RwFu2DCmbn6CZZ5nUuaCf8v4aKQL90JDrtG9%2Fq0Kz%2FSTsrNkH4Yb1f7JebDIgy2Dqknku77hRdFvP3%2FMnF%2BlmpC0HTME0kyF4kWPzTmw%2BHQDVrbwMI6NZJFN9%2FetaLMU5wr8W2x%2BbtEYyUse8tIfEJ7W%2F89TjT8jkrv7KrOW88PYwrWwufec1MKK5rLkvCNNe7KJAEdSIJ1bAlFyMqtWowvbSGy8MFAlZvRTJhkffvNLkVbczozEwcVlMRSHKV4M6VzLicoejehZS7Kx7%2F24hCEhwMage4esM%2BeOG6xh18tSW9wTsFUgtbAZs2wP%2BSx3vZ0IZfXJZRp9Gt5z5N6qrl6XbQseOgkhm2rLk9O2o0G04s1u3rdWUpJtzPilE6aRYvE6qd6BcMAtNW%2F3PGtF5HXm1tSmymX3q1u6m4%2F6AnkClsGHljH3OVg%2Fz6rsimavcXNEZyY9uEFqaEpSTjSeLuJy%2B3AoG7OvxrOP3jT56xDu3601y3%2BsoYCxj6MqwkT0uyH3y7Zu62vT1ejNmmnIVYpWrwynApQcgNnakke89YdbdI2AIiVMIF7jktQVtOzDwmJTJBjqkAaPVhz%2FIH9j5neDtFBNG%2BS3WrJ%2BFdhYBKTxcLHae4jA93T75sVd7ht6ZAyMAHS5XC%2FNuXe%2Fm58YU0RM3FDP9Fc8eb1L0RNW7XdJnwQ9JcaiGrbU8w4QtLG7in1MofoXKgEkITX5EH91M%2FyUSicVLMUXLt3EvyX7WyegCdDazS%2BOkgSJRjtt4lRbQnfig4Is75R2VQ7NqWe4HRPQNJxx%2FGrJLM7ms&X-Amz-Signature=024d84cb1fd59b6b54d4d7e86ee4bd4fb4454157496f7746600714c38f4ce311&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
