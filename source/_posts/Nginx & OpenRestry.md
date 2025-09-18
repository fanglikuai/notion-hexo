---
categories: 整理输出
tags:
  - nginx
sticky: ''
description: ''
permalink: ''
title: Nginx & OpenRestry
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b5169eb9-ccc9-44c2-9041-4163c757fcfc/90406427_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQJENITT%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T110037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDEWsSGAwt0VhuI6RwzGzRRvWJwmPxJCzNsz5opdn3lTwIhAP0yHzWQbFhqTtEtvyMKXLi2g0wrHwmCg2Z0IKVzOTqPKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzZsUensOEp%2B9BCZL0q3AP%2F91abowWy88GGQNKXI3T7ywHeIjCfEYIHYeIH92B7ynXzC4aILntZWvyQNc9K4G7q6XcWy1cfDE43MXclxIrF6ja0rkUOtim5esqzVjaYyvT5YL6tNKXS%2B8tjiewyfP8F9iKgsjjSZ7mAfWNjmzZQRMZWG2FlD3YtiiWe4O7%2F1mUUC1kDj%2FoR1mcqiWzUXEmaMCGWHR79N0pEA12Emp0Pmkd3q4qeNTcyaQ7OVjVinVq2e4Brsaef7GOYXI2tkPqxIOyKvFnVPf45r%2FDETon9QXAZi8z6OlomF2BHRwOHyzQDs1avxJsg74BdrC4AFoz5nQwyuHql1k0mkHUYqEwQJeUoxEQlhmHdhKZzY3y7ABbl1PGKDj6NR40aoVg4pH0w6o4nEIf6M2R%2FSBCY8zj%2FMsIA0UqD2zOg91CwDUgqUcGG0BqsrdyBD5q9gnlTVUX4Qr7X0fRdqtEooygvJgXvQpT0aZf8jeSsR6MYkOKFwun6Y176kFk3yX%2FF58X0XckFKLus6e1AUdt0U%2BTShuYEKhtjI2JFTB%2B00nZWdz2%2BpJhDGL8gPu8YTK7uVsxFWmzjGEgYGHxPFdzGIAKMFsGhU5tjeniewOOsv0fkKydqNfEDWK0sexTIvFNRMzCgua7GBjqkAbeq0ErZyU32Pqo50uvoAWdXKE9t0XyJTO4Pezyr7frfgLGEYlUfITbb%2FOujgWZ2W7kA3zMGGkW1Yd5Yo8%2Fjx66jXJHUZWyV%2FWkiEsrDy%2BQdWXWGirlGFZFNS48JXKD0mydy1iMRtjG%2FsEtLi1GCFt%2BUXEcdPHvmoeH4hf6ZSMXhh9bg7wkqW4ZD53NdOMQEEDWmkezyC3ine0N7Ii8cj3N%2FYIr%2F&X-Amz-Signature=e36ba85db7a1780332930389e2b227c4a3aa9477a6b26f66fc4274369ec397ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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
