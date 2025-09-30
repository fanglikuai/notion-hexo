---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663MRH375V%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T160105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJGMEQCICxJJkA4jAbZJxOMr63ad35pZlHo0eph9bkub3ErqxghAiASxzNOrMJTSGojHx0CNmwCfx2Rwc7PN71puUutb%2BHCUSqIBAjx%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMX7WErT2%2BMn7H3kzQKtwD6mkJ4yLEFAabILonvHSdnQXkwQOtMwE9AwEedL8Uu8OVjs%2BFffK%2BMrD5WFBANTsJniOzVthljFzXf1YM82RxaLGiyCnMRV6VGwi99u76G3yuIZVrQszmniXlkzoNv22y9H53R61lnYsUCgw%2B6m95X7l5kvVWOeefB42PDuaSvh%2BMCorwUCHMR9spETWesICfPiyVvJkaF2PVig8Bev%2FKo%2BFmnZfDZV7uQ1plFsdHJqPMoGhhaLjWYHoaC7rA%2FtLe%2B6I1REfbKRe21LrHRgMoGmhCjIZgEefX1Ty2fLZUJ0L3CYyVuZ%2Bmhe965mQ67P2lIVKh4gB8VAmfJD1uDtZGmWsidMN25PtIZ70SH5CF7v2QJuJsXcM41wCDeWDKPYK%2Bom9BaaEifi0dyKOzWjwDEdvrgx8yMYThFn%2FpYhFsMjFz4M90qsY7DyU6F4okUdHHHsVcZF0GKIqQyU%2FRDLaJfXdalrTrmUQ4535UjPgokw6q%2BeIkEQbj7R4KKIveUH3LuP9qtiJowYmiOiMlS3g7cFQxY%2BvObGZMZslgTa6T%2FQLH%2ByMvml%2FyqZAqCDw9jwnFZDTB%2F4x5YzYBmOTpRIUhRVeAKkVIZLDjj2SvDgtlDAmEvFM%2BnPb1LU6q7NwwnvrvxgY6pgE1mGQabyBBn53og29iDcoaIkmu%2BVwsXhM18HBMcGBSnptCiIS%2BUW0ErmBUb%2FLQ%2F%2B4V%2FraqNm8qFOhPzfEO7YzaNAV9qPXKpJlbkDI8IsaXhyZVyLGIXyFWxAiGlacYZmWx%2BaWfZ8TFEW2CLZFyNZFuUFgWKWAlLp4QwOvf%2FzAUqoHtjAqgwRj%2FSUKoOOj4LeON4GeWK1OlgPcQ5y6jEn9%2BA26BtEYQ&X-Amz-Signature=34ddd54aacb6703fc24edf1402c990700d30918496b7425d0e4a0c2874cbf628&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
