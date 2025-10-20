---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XRWQACQN%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T050038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLXdlc3QtMiJHMEUCIESfE9JGoGOgrLpYtME8gVjVSpCSqNmaswFbS8B4MOpsAiEArGvxgi3x9GWaxwBw3h4zXxaKiqn3iH8osc1qE4pk4%2FYqiAQI4f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM3EkFO5a2pOqQRWVCrcA8YqnF9anthRjrTYAQJ8jHNhg4OqkcbK4N2IV7a3bH2t66Mm1DeHkKlkXPDOWUwMMikeAaEilJ2s1%2BTthe2jIOQotNtQ5tMgIpLdICb6Bwa39750bFpLYGpKyGlFnWzWL820eQp%2F%2F4ypRd613w4v%2B9FEsQ11T6pvA81HVd3AFW3IzkfbR6bH6Cq0O5YPVlkmfsQvUckZh5XwyidM3h2QIoqtTxmQxcf%2BHuHSHWjk6hSZwGhQvZ0P8lbt8GMvm74Otk3GmwY5nUNIBO06gKICriu1iRsHo8XSbRehTg%2Fd2IaAasrbtkPpiLAVKsYA0SGpJT6zdey%2BFPVgTQLGMwGr47MpGwvQOueR%2BaWEsm7j7mbItTinSR%2BLDiwXMmkBlfK0GjI%2FM7ebq046uwqtgOwC%2BxpwOYEKkykLCsaQtpqf8koWcpz7p%2FayHpeB34f%2BXjDXCZ1trKBIodcq%2FqahilaDNj39sxSlGDeq%2BbLw8b95O5r6sakGi1GYYlyyB%2F6ikmYb5RiB5X8c74bTjssoYbx3Hexe3tjQA5ByxmMLFlzo0y0VB8PViFfUvSa3duZ8ARiWanjBumfhvyCqtuoTgvZPzshVwGdQoPZPkFaK6ueXwOuzueBSnSared3xbcpQMK%2F31ccGOqUBlXI36RZvt4ny6QkmBY%2BmeizgaVNWUGzC51Ih6qRppy3MEjlYFTDdHPZ33cXFQuVoVXZaakHmV9Gv3KCqvL3kR6rqDWHNvf0K5pX14C%2FBXQlQJ6Sd2UWN%2FUBKHcScd4YRvnMJo6IaHP4L5Du3KKqOVL5kJjSSULNLG3nBRLiK0zDBBbixym764bco7Ay0Zxwo%2BnI5%2FtjmOXXFgaT5gYbvTojol%2B96&X-Amz-Signature=63582c8714bf3a4e88c00f7e8fca3650168429f516f1fd8821a8551932aac628&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
