---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZLJRR6DV%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJIMEYCIQD7pYjByHqyIan9ZkQHDLEvcLmfhjhMw5B8EbAbK%2B5wYgIhAIVZgd6ZmTW%2BX7ZWHGq%2Fc8ndfRw5HmwXmZnn952l1m7OKogECKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzXPPktjyoa%2B4%2FFBzMq3APO5Mg8fu4WCxMfdBuc61RHHhToi1%2BS8nT91tHVlLEkwQ9b1sa1Fr%2BtF3VOM4R3OqDAP4CDQD9NMLzaeVYsTIn7U5uEGafvMKUNJjx33%2B6qiwwrSmQQA0WE3U%2FKzPQW2O%2BuTSMC3VwjD8DDLBjy4ZTU15y0soWQ8LQ5wCn9XSvNfUHH9tR%2FCPn1aABTVVr%2FlzDWNVnrfzGkZHrUHrJ%2BvH0ZwmmRbBdXwITcL4g0Ngska%2FPoJkuJsTFZuVF9n1CaIiScYgMI8oFHTmNhKExBBMkFTyjkKQlWVyVBkTZ4b6iUq5P7ZjKPQMaUM2Ku%2B6AaoTAfvaBALn3GoP1%2Bf2KWAUE0B7NLc4zPsED1Mq7MyAYhZXk%2FkeqYQTxRZC%2BTyfuv%2Fv%2BT6dDe%2B9jFI6%2B1PS3lJJGq857AGpnOHLxCNrgvuszCOCLOT%2BAeI3bDGJPripQheP8E8tV5rdjBoEMmLVBhSclC56RfihA6MnKalN4qAO9PxoAvUEtOMnXiiZQajlsg9bQXsNPEGYel7cDqy048UzxvDbD4zrY6pK2tv3R79DVysp7x6CnqUbRmAajqpvHL8Tmk9itSu5V%2Fsd5Le08dpiL%2B1j4ewwZCsu7OXQ18CidTec1tXpqga0fY4Ku5CDCg%2FJPHBjqkAe9aTpzs8xl%2Fe68EVMpAF2sxNDISxylGtYlRBrpn2UDULC5Ekw5rXHLo5mKGvf5xYjIYd1iypdeASfBaOkzjNZwSXmzOq4yIQWzz5b7gci3Aj0JA6WKql3GFPK0gfpBMRgklOe69Uj%2Fl%2FvkezWJKXoR4LZkFb4Sr9v3789Oo9l5QSU1652wF3rzkpbdzNaJX%2FTFgbSWbyw1HgoLrcAkhaRWHc3o8&X-Amz-Signature=abd63cbb9821cb8c3fda1adf9de82786eff800cb2a022ebc57cbc387bf2aae7a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
