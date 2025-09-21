---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665A3ETUC5%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID5kekUBd97qhKo%2BoGj7l4JKrzBgmPuQbEgQjRnuQ8MGAiA90C2Z1D8i4MBQOb%2BuKOC6snN24BKV3OsMo7f3qtp9Zyr%2FAwgbEAAaDDYzNzQyMzE4MzgwNSIMaaijw2e19L2hb172KtwDUbGqGZDtWUx%2FqMdzGBrK2lqgwMSymlJlOQ0M9xZTQLrLds177MHUAGD7CrMNXRvrsrMbD3MATGWXMzWQNr8K1I9NOdzimIaqSLKkzX8t1XSoS4U%2BOso6%2BZEhZ7BuKZybVWfz8m8gRao9HdtC6ztBYWyL%2Ffugul3GK9TpIWaAKJAWdYVkoUrykYiF7%2BTuEs0Lu8kIz1X%2F85ZKwP5Jd3OJqVhIMovb6r965E9UupSrTgPPyp8EL9Doo8KafgWu1n2iDzn9ZFkP3fYWw3AtFKwJbDMd3QI0Wgn9lP5Sr0VZhbkC5i3RM0eCWh6WTfliwKbZ2SRTwBUkNHpCVhka6f5vdqiVVuR7gL%2ButNmM9JMtvyLJ19fpKI12cmeBX9A2xQh300BT101jGCXarGIggkUsI%2FMKh2otd%2BRYwKxCIljzae3Y1K5NZkHMr5AcULGdV%2FonwOtZCf4YMSVKf4QICue%2B4Elll%2FKJ8tHieXM1P2FopZ7rDUagLmyUSY7wfZbdSLz9zJyiMkN9%2FqoZz6Ef6hBkI0tAqDJ0JDGhvn1InnGwUeKETvLsfYRNIjFliOrJo1i1oYerBoYqQAKIXPa%2F9hU894YxxnZcQlWhDivQax%2FwAuSDKaFYVu3zXQb0cWQw3OnAxgY6pgFcJN2%2FOPK7zd6r9bSiDW%2F0nRiGWzhKPxq%2FePU1nbber%2FQ5372Wg9fEVdoWK%2BjxQa9VzQ%2Ffv5Z9bN%2BHcBlenMGjpGdrUKh1Shsszit4P0Lhh3Nb1o7AK%2BDAy3MMwknb71b6x2%2FtK7GEjnHZWKnozRD6sioOCtvRfJFCGgZBo99mvzBjnuOE1Ocr4FpNGScZ45TGyfWrT8imepYgRJu%2FrSSWGEZ9qmZn&X-Amz-Signature=b76cf0e749baca86e07bda0ffd61682d864db38b44efd077c26f41f4051f68b0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
